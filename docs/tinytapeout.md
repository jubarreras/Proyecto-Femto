## Desarrollo del Repositorio para Fabricación en Tiny Tapeout

## ✅ Resultado del Proyecto en Tiny Tapeout ✅

El resultado para este caso se puede ver en el siguiente enlace:  
[Repositorio Tiny Tapeout – Femto RISC-V - jdbarrera](https://github.com/jubarreras/femto_risc)

---

El repositorio constituye la base del flujo de trabajo que permite llevar el diseño del procesador **Femto RISC-V** desde su descripción en Verilog hasta la fabricación física en un chip mediante **Tiny Tapeout**.  

Tiny Tapeout es una plataforma colaborativa que democratiza el acceso a la fabricación de ASICs, ofreciendo un flujo simplificado para que diseñadores, estudiantes e investigadores puedan materializar sus proyectos en silicio. En este contexto, el repositorio realiza la preparación de los archivos finales para tapeout.  

La estructura del repositorio debe ser clara y modular, permitiendo:  
- **Separar** los archivos de descripción RTL (`.v`) de los testbenches y scripts de simulación.  
- **Incluir** documentación detallada del flujo de síntesis con Yosys y verificación con herramientas como GTKWave o Xyce.  
- **Preparar** los netlists y configuraciones necesarias para el entorno de Tiny Tapeout.  
- **Facilitar** la reproducibilidad del proyecto, asegurando que cualquier usuario pueda replicar el flujo desde la clonación del repositorio hasta la generación de los archivos de fabricación.


### Requisitos previos
- **Diseño RTL en Verilog** completamente funcional y verificado (ejemplo: `femto.v`, `mult_32.v` y periféricos).  
- **Testbenches** que validen el comportamiento del diseño en simulación (usando herramientas como Icarus Verilog, Xyce o Ngspice).  
- **Síntesis correcta** con Yosys, incluyendo mapeo a librerías estándar (Sky130).  
- **Repositorio en GitHub** público y organizado, siguiendo la estructura recomendada por Tiny Tapeout (`src/`, `sim/`, `docs/`, `tt/`).  
- **Cuenta en GitHub** y acceso al flujo de integración continua de Tiny Tapeout.  
- **Configuración del proyecto** en el archivo `info.yaml` (nombre del proyecto, autor, descripción, tamaño, etc.).
---
## Procedimiento para Preparar el Repositorio y Mandar a Fabricar en Tiny Tapeout

El flujo de Tiny Tapeout exige que el repositorio esté bien estructurado y que cada etapa del diseño esté validada antes de enviarse a fabricación. A continuación se detallan los pasos recomendados:

### 1️⃣ Subir y validar archivos Verilog en `src/`
- Colocar todos los archivos fuente (`.v`) en la carpeta `src/`.  
- Verificar que cada módulo esté correctamente descrito y que no existan duplicaciones de señales o errores de sintaxis.  
- Ejecutar la síntesis con **Yosys** para confirmar que el diseño se compila sin errores:  
```bash
  yosys -p "read_verilog femto.v; synth -top femto; stat"
```
- Revisar los reportes de síntesis (stat) para asegurar que la lógica se generó correctamente y que no hay advertencias críticas.
- Este paso garantiza que el código RTL es confiable y listo para integrarse en el flujo de fabricación.



### 2️⃣ Validar y estructurar el archivo `info.yaml`

El archivo `info.yaml` define los parámetros del proyecto para Tiny Tapeout, revisar cuidadosamente:

- Convención de nombres: el nombre del proyecto debe ser único y descriptivo.
- Top module: debe coincidir con el nombre del módulo principal (femto).
- Tile size: especificar correctamente el tamaño de la tile asignada por Tiny Tapeout.
- Autor, descripción y versión: incluir datos claros y completos.
- Validar la estructura del archivo con herramientas de verificación YAML para evitar errores de formato.

Este archivo es crítico, ya que define cómo se integrará el diseño en el flujo de tapeout.

### 3️⃣ Nombrar puertos y top module según convención de Tiny Tapeout

Tiny Tapeout requiere que los puertos del top module sigan una convención específica para integrarse en la matriz de tiles, asegurarse de:

- Nombrar entradas y salidas siguiendo el estándar (ui_in, uo_out, uio_in/out, clk, rst_n).
- Definir el top module exactamente con el nombre indicado en info.yaml.
- Verificar que el número de pines y su dirección coincidan con las restricciones de Tiny Tapeout.

Este paso garantiza compatibilidad con la infraestructura de fabricación y evita rechazos en la integración.
### 4️⃣ Subir documentación, simulación y testbenches

Incluir en el repositorio una carpeta docs/ con documentación clara del flujo de trabajo, añadir los testbenches en sim/, junto con instrucciones de ejecución:
    
```bash
iverilog -o femto_tb femto_tb.v femto.v
vvp femto_tb
gtkwave femto_tb.vcd

```
- Documentar paso a paso cómo simular el diseño, de manera que otros usuarios o los fabricantes puedan reproducir las pruebas fácilmente.
- Incluir ejemplos de resultados esperados (gráficas, logs de simulación).
- Este paso asegura confiabilidad y transparencia, permitiendo que el diseño sea validado por terceros y aumentando la probabilidad de éxito en la fabricación.

## Estadísticas de Síntesis y Layout

En esta sección se presentan las estadísticas generadas automáticamente por el flujo de TinyTapeout a partir del archivo GDS. Los resultados incluyen el **número de compuertas lógicas utilizadas**, el **porcentaje de utilización del ruteo** y el **largo total del cableado en micrómetros**. Estos datos se muestran en la siguiente imagen para ofrecer una visión clara del rendimiento y la complejidad física del diseño.

---


## ✅ Resultado del Proyecto en Tiny Tapeout ✅

El resultado para este caso se puede ver en el siguiente enlace:  
[Repositorio Tiny Tapeout – Femto RISC-V - jdbarrera ](https://github.com/jubarreras/femto_risc)
