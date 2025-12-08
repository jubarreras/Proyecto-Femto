# Simulación  
## 1️⃣ Generación de VCD y visualización (Icarus Verilog y GTKWave)

### Introducción
El propósito de esta primera simulación es verificar la correcta integración de los módulos del procesador **FemtoRV** junto con sus periféricos y memorias, generando un archivo de ondas (`.vcd`) que nos permita observar el comportamiento de las señales internas.  
Este flujo inicial asegura que el diseño RTL se compile sin errores y que podamos inspeccionar las transiciones de las señales en **GTKWave**, antes de avanzar hacia simulaciones post-síntesis.

---

### Comandos utilizados

#### ⚙️ Compilación con Icarus Verilog
```bash
iverilog -DFUNCTIONAL -DUNIT_DELAY=#1 \
  -o sim_femto.vpp \
  -s femto_TB \
  femto_TB.v \
  OpenLane/src/*.v \
  cores/sim_spi_flash/spiflash.v \
  cores/sim_spi_ram/spiram.v
```

- **`-DFUNCTIONAL`** → Usa modelos funcionales de librerías.  
- **`-DUNIT_DELAY=#1`** → Asigna un retardo unitario a las compuertas.  
- **`-o sim_femto.vpp`** → Nombre del ejecutable de simulación.  
- **`-s femto_TB`** → Define el testbench top-level.  
- **Archivos `.v`** → Incluyen:  
  - Diseño principal: `femto.v`  
  - CPU: `femtorv32_quark.v`  
  - Periféricos: `perip_uart.v`, `uart.v`  
  - Memorias: `MappedSPIRAM.v`, `MappedSPIFlash.v`  
  - Modelos de prueba: `spiflash.v`, `spiram.v`

Posterior a esto, se realiza la ejecución de la simulación, con:
```bash
vvp sim_femto.vpp
```
- Corre el binario generado por Icarus Verilog.
- Produce la salida de simulación y el archivo de ondas (`femto_TB.vcd`).

Por ultimo, con este comando se realiza la visualización de señales en GTKWave:
```bash
gtkwave femto_TB.vcd &
```
- Abre el archivo de ondas en GTKWave.
- Permite inspeccionar gráficamente las señales del diseño.
- El `&` lo ejecuta en segundo plano para no bloquear la terminal.
#### Visualización de señales del archivo .vcd generado:

<p align="center">
  <img src="gtkwave.png" alt="sim gtkwave" width="800"/>
</p>

<p align="center"><em>Figura 1. Visaulización de señales en GTKWave</em></p>


En la imagen se observa la simulación utilizando *GTKWave*. Las señales listadas corresponden a diferentes módulos y buses de comunicación del procesador FemtoRV y su entorno de pruebas (testbench).

Las señales clave incluyen:

- **`clk`**: Señal de reloj que sincroniza todas las operaciones del procesador.  
- **`resetn`**: Señal de reset activa en bajo, utilizada para inicializar el sistema.  
- **`spi_clk, spi_cs_n, spi_mosi, spi_miso`**: Señales asociadas a la comunicación SPI, usadas para conectar memorias externas (como flash y RAM SPI). “spi_clk” es el reloj SPI, “spi_cs_n” es la señal de chip select (“n” indica activo en bajo), “spi_mosi” y “spi_miso” son las líneas de datos para envío y recepción respectivamente.
- **`TXD, RXD`**: Líneas de transmisión y recepción serial UART, relevantes para la comunicación externa del sistema.
- **`PC`**: Contador de programa, que indica la dirección de la instrucción que está ejecutando la CPU en cada ciclo.
- **`RAM_MEM, RAM_rdata`**: Señales que muestran el contenido de la memoria RAM utilizada por el procesador.
- **`registerFile`**: Muestra el contenido de los registros internos, en este caso el grupo de 32 registros de propósito general típicos de arquitecturas RISC-V.

Para comprobar que el procesador está funcionando correctamente durante esta simulación y antes de proceder con la síntesis física, se debe observar varios aspectos:
1. *Integridad de señales*: El reloj (clk) debe oscilar correctamente, y el reset solo debe estar activo al inicio.
2. *Ejecución secuencial*: El PC debe incrementar coherentemente, lo cual indica que las instrucciones están siendo ejecutadas en secuencia o según saltos programados.
3. *Transferencia de datos*: Las señales relacionadas con carga/almacenamiento en memoria y comunicación por SPI/UART deben mostrar actividad conforme a las instrucciones del programa cargado.
4. *Cambios en los registros*: Los valores de los registros internos y posiciones de memoria deben modificarse acorde a los resultados esperados de las instrucciones, permitiendo validar operaciones aritméticas, lógicas y de control de flujo.
5. *Resultados esperados*: Comparar la evolución de señales y salidas con los resultados esperados del programa de prueba (testbench), asegurando que no existan estados indeseados como valores indefinidos (X) o inactividad donde no corresponde.

Si todas las señales muestran un comportamiento coherente y esperado de acuerdo con la especificación funcional del procesador, se puede concluir que la simulación es exitosa y que el diseño está listo para pasar a la etapa de síntesis para implementación en hardware físico.
