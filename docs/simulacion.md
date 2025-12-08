# Simulación  
## Generación de VCD y visualización (Icarus Verilog y GTKWave)

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

