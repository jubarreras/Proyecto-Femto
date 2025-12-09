## 📂 Descripción y Construcción de los Archivos Verilog (.v)

El diseño del procesador **Femto RISC-V** se organiza en múltiples archivos Verilog (`.v`), cada uno encargado de describir módulos específicos de la arquitectura. Estos archivos representan desde bloques aritméticos básicos hasta unidades de control más complejas, y su correcta estructuración es esencial para garantizar la síntesis y simulación sin errores.  

La construcción de cada archivo sigue una metodología modular:  
- Se definen las **interfaces** (entradas y salidas) que conectan el módulo con el resto del sistema.  
- Se describe la **lógica interna** mediante asignaciones combinacionales o secuenciales.  
- Se asegura la **compatibilidad jerárquica**, de modo que los módulos puedan integrarse fácilmente en el diseño superior.  

En esta sección se documentarán los archivos más importantes, explicando su función dentro del procesador y cómo se construyen paso a paso. Esto permitirá comprender la relación entre los bloques individuales y el sistema completo, además de facilitar futuras modificaciones o depuración del código.  

---

## Descripción del Top Module `femto.v`

El archivo **`femto.v`** actúa como **módulo superior (top module)** del procesador **Femto RISC-V**. Su función principal es integrar el núcleo del CPU con los periféricos externos (SPI Flash, SPI RAM, UART, LEDs) y gestionar la lógica de direccionamiento mediante señales de chip-select.  

A continuación se detallan las partes más importantes del módulo:

## Señales del Top Module `femto.v` y Submódulos

### Top Module `femto.v`

| Señal        | Dirección | Descripción                                                                 |
|--------------|-----------|-----------------------------------------------------------------------------|
| `clk`        | Input     | Reloj del sistema, sincroniza todas las operaciones.                        |
| `resetn`     | Input     | Señal de reset activo en bajo, reinicia el sistema.                         |
| `spi_mosi`   | Output    | Línea MOSI para comunicación con memoria SPI Flash.                         |
| `spi_miso`   | Input     | Línea MISO para comunicación con memoria SPI Flash.                         |
| `spi_cs_n`   | Output    | Chip Select activo en bajo para SPI Flash.                                  |
| `spi_clk`    | Output    | Señal de reloj para SPI Flash.                                              |
| `spi_clk_ram`| Output    | Señal de reloj para SPI RAM.                                                |
| `spi_cs_n_ram`| Output   | Chip Select activo en bajo para SPI RAM.                                    |
| `spi_miso_ram`| Input    | Línea MISO para comunicación con SPI RAM.                                   |
| `spi_mosi_ram`| Output   | Línea MOSI para comunicación con SPI RAM.                                   |
| `LEDS`       | Output    | Salida hacia LEDs del sistema, indica estado.                               |
| `RXD`        | Input     | Entrada de datos seriales (UART RX).                                        |
| `TXD`        | Output    | Salida de datos seriales (UART TX).                                         |

---

### Submódulo `FemtoRV32 CPU`

| Señal        | Dirección | Descripción                                                                 |
|--------------|-----------|-----------------------------------------------------------------------------|
| `clk`        | Input     | Reloj del sistema.                                                          |
| `reset`      | Input     | Reset del procesador.                                                       |
| `mem_addr`   | Output    | Dirección de memoria solicitada por el CPU.                                 |
| `mem_rdata`  | Input     | Datos leídos desde memoria o periféricos.                                   |
| `mem_rstrb`  | Output    | Señal de lectura de memoria.                                                |
| `mem_wdata`  | Output    | Datos a escribir en memoria.                                                |
| `mem_wmask`  | Output    | Máscara de escritura (bytes válidos).                                       |
| `mem_rbusy`  | Input     | Indica que la memoria está ocupada en lectura.                              |
| `mem_wbusy`  | Input     | Indica que la memoria está ocupada en escritura.                            |

---

### Submódulo `MappedSPIRAM`

| Señal        | Dirección | Descripción                                                                 |
|--------------|-----------|-----------------------------------------------------------------------------|
| `clk`        | Input     | Reloj del sistema.                                                          |
| `reset`      | Input     | Reset del periférico.                                                       |
| `word_address`| Input    | Dirección de palabra en RAM.                                                |
| `wdata`      | Input     | Datos a escribir en RAM.                                                    |
| `rd`         | Input     | Señal de lectura habilitada.                                                |
| `wr`         | Input     | Señal de escritura habilitada.                                              |
| `rbusy`      | Output    | Indica que la RAM está ocupada en lectura.                                  |
| `wbusy`      | Output    | Indica que la RAM está ocupada en escritura.                                |
| `CLK`        | Output    | Señal de reloj hacia la RAM SPI externa.                                    |
| `CS_N`       | Output    | Chip Select activo en bajo para RAM SPI.                                    |
| `MISO`       | Input     | Datos recibidos desde RAM SPI.                                              |
| `MOSI`       | Output    | Datos enviados hacia RAM SPI.                                               |
| `rdata`      | Output    | Datos leídos desde RAM SPI.                                                 |

---

### Submódulo `MappedSPIFlash`

| Señal        | Dirección | Descripción                                                                 |
|--------------|-----------|-----------------------------------------------------------------------------|
| `clk`        | Input     | Reloj del sistema.                                                          |
| `reset`      | Input     | Reset del periférico.                                                       |
| `rstrb`      | Input     | Señal de lectura habilitada.                                                |
| `word_address`| Input    | Dirección de palabra en Flash.                                              |
| `rdata`      | Output    | Datos leídos desde Flash.                                                   |
| `rbusy`      | Output    | Indica que la Flash está ocupada en lectura.                                |
| `CLK`        | Output    | Señal de reloj hacia la Flash SPI externa.                                  |
| `CS_N`       | Output    | Chip Select activo en bajo para Flash SPI.                                  |
| `MISO`       | Input     | Datos recibidos desde Flash SPI.                                            |
| `MOSI`       | Output    | Datos enviados hacia Flash SPI.                                             |

---

### Submódulo `peripheral_uart`

| Señal        | Dirección | Descripción                                                                 |
|--------------|-----------|-----------------------------------------------------------------------------|
| `clk`        | Input     | Reloj del sistema.                                                          |
| `rst`        | Input     | Reset del periférico.                                                       |
| `d_in`       | Input     | Datos a transmitir por UART.                                                |
| `cs`         | Input     | Señal de chip-select para habilitar UART.                                   |
| `addr`       | Input     | Dirección interna del periférico.                                           |
| `wr`         | Input     | Señal de escritura habilitada.                                              |
| `d_out`      | Output    | Datos recibidos por UART.                                                   |
| `uart_tx`    | Output    | Línea de transmisión serial.                                                |
| `uart_rx`    | Input     | Línea de recepción serial.                                                  |
| `ledout`     | Output    | Señal hacia LEDs, indica actividad del UART.                                |

--- 


