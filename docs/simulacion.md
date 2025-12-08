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
  <img src="gtkwave.png" alt="sim gtkwave" width="950"/>
</p>

<p align="center"><em>Figura 1. Visaulización de señales en GTKWave</em></p>


En la imagen se observa la simulación utilizando *GTKWave*. Las señales listadas corresponden a diferentes módulos y buses de comunicación del procesador FemtoRV y su entorno de pruebas (testbench).

Las señales clave incluyen:

- **`clk`** y **`resetn`** →  Reloj principal y señal de reset del sistema.  
- **`spi_clk`, `spi_cs_n`, `spi_mosi`, `spi_miso`** → Interfaz SPI utilizada para la comunicación con memorias externas.  
- **`cs[6:0]`** →  Líneas de selección de chip para identificar dispositivos conectados.  
- **`mem_address[31:0]`** y **`mem_rdata[31:0]`** →  Dirección y datos de lectura en la memoria principal.  
- **`mem_wdata[31:0]`** y **`mem_wmask[3:0]`** →  Datos y máscara para escritura en memoria.  
- **`mapped_spi_flash_rbusy`** →  Estado de ocupación de la memoria flash SPI.  

Para validar el correcto funcionamiento antes de la síntesis, se deben comprobar los siguientes puntos:

1. El reloj oscila correctamente y el reset solo se activa al inicio.  
2. Existe actividad coherente en las señales de SPI y de memoria durante los accesos.  
3. Las direcciones y datos de memoria cambian conforme se ejecutan las instrucciones, sin valores indeterminados.  
4. Los resultados observados coinciden con los esperados del programa de prueba.  

Si estas condiciones se cumplen y las señales muestran la actividad esperada, el procesador está listo para la etapa de síntesis en hardware.
