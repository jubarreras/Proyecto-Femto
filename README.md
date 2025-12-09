# Femto RISC-V 32

## Introducción
En este informe se presenta el diseño y análisis de un procesador basado en la arquitectura RISC-V, tomando como modelo base la familia FemtoRV. El diseño modular y bien documentado del FemtoRV ofrece una visión clara de las etapas involucradas en el procesamiento de instrucciones y la manipulación de datos, elementos esenciales para comprender y desarrollar procesadores modernos y eficientes, por medio de diversas herramientas de software se mostrará cómo se sintetiza, simula y valida el funcionamiento del procesador, permitiendo observar de manera práctica el flujo completo de diseño digital. La base conceptual y arquitectónica del proyecto se apoya en la imagen que se muestra a continuación, la cual servirá como referencia para entender la organización interna del núcleo.

<p align="center">
  <img src="images/Estrutucura_base.png" alt="Estructura FemtoRV" width="400"/>
</p>

<p align="center"><em>Figura 1. Arquitectura base del procesador FemtoRV</em></p>

En la imagen presentada del diagrama de bloques del procesador RV32I se pueden identificar varios módulos principales, cada uno con una función específica. La Unidad de Comunicaciones (ubicada a la izquierda) se encarga de las transferencias de datos entre el procesador y la memoria, gestionando operaciones de carga y almacenamiento (load/store) para diferentes tamaños de dato. El módulo central, que incluye registros como rs1, rs2 y el registro de destino (dest_reg), administra la ejecución de instrucciones aritméticas y lógicas, así como el flujo de datos internos. La ALU (Unidad Aritmético-Lógica) realiza operaciones matemáticas y lógicas de acuerdo con las instrucciones del programa, mientras que otros componentes, como el PC (Program Counter), controlan la secuencia de ejecución de instrucciones, incrementando su valor según el tipo de operación realizada (salto, llamada, instrucción secuencial, etc.).

El procesador FemtoRV se distingue por su simplicidad y eficiencia. En su variante más elemental, denominada FemtoRV-quark, implementa el conjunto de instrucciones RV32I utilizando únicamente unas 400 líneas de código Verilog, lo que facilita su estudio, despliegue y personalización, convirtiéndolo en un excelente punto de partida para este proyecto.

## Herramientas y Prerrequisitos
El desarrollo y la implementación de un procesador requieren un conjunto de herramientas de software que cubren todo el flujo de diseño digital: desde la síntesis lógica y la simulación funcional, hasta la visualización de señales y la generación del layout físico. A continuación se presentan las principales utilidades empleadas en este proyecto, junto con sus funciones y enlaces oficiales para instalación y documentación.

- **Yosys**  
  Herramienta de síntesis lógica para convertir diseños en Verilog a netlists optimizadas. Permite aplicar transformaciones, verificar consistencia y preparar el diseño para flujos posteriores de implementación.  
  [Repositorio oficial](https://github.com/YosysHQ/yosys)

- **Icarus Verilog**  
  Compilador y simulador de Verilog que facilita la verificación funcional de módulos digitales. Es ampliamente usado en entornos educativos y de investigación por su simplicidad y compatibilidad con estándares.  
  [Repositorio oficial](https://github.com/steveicarus/iverilog)

- **GTKWave**  
  Visualizador de formas de onda que permite inspeccionar señales generadas en simulaciones. Es esencial para depurar y analizar el comportamiento temporal de los circuitos digitales.  
  [Repositorio oficial](https://github.com/gtkwave/gtkwave)

- **ngspice**  
  Simulador de circuitos analógicos y mixtos basado en SPICE. Permite integrar modelos digitales con analógicos, lo que resulta útil en co-simulación y validación de sistemas mixtos.  
  [Repositorio oficial](https://github.com/imr/ngspice)

- **OpenSTA**  
  Herramienta de análisis estático de temporización (Static Timing Analysis). Verifica que el diseño cumpla con las restricciones de tiempo y asegura la correcta operación a la frecuencia objetivo.  
  [Repositorio oficial](https://github.com/The-OpenROAD-Project/OpenSTA)

- **Magic**  
  Editor y verificador de layouts VLSI. Permite visualizar, modificar y comprobar reglas de diseño físico, siendo una pieza clave en el flujo de implementación de ASICs.  
  [Repositorio oficial](https://github.com/RTimothyEdwards/magic)

- **OpenLane**  
  Flujo automatizado de diseño físico que integra múltiples herramientas (Yosys, OpenROAD, Magic, etc.) para llevar un diseño desde RTL hasta GDSII. Es la base de muchos proyectos de fabricación abierta.  
  [Repositorio oficial](https://github.com/The-OpenROAD-Project/OpenLane)

- **TinyTapeout Build System**  
  Infraestructura de construcción y automatización para enviar diseños al flujo de fabricación de TinyTapeout. Permite integrar proyectos en el ecosistema colaborativo y generar los artefactos necesarios para tapeout.  
  [Repositorio oficial](https://github.com/TinyTapeout/tt-support-tools)

## 💡 Descripción de Hardware
El procesador Femto RISC-V 32 se basa en una arquitectura modular inspirada en la familia FemtoRV. En esta sección se introduce la organización interna del núcleo, sus principales módulos y la forma en que interactúan para ejecutar el conjunto de instrucciones RV32I.  
✅[Ver más detalles en `docs/hardware.md`](docs/hardware.md)✅

## ⚙️ Síntesis, Simulación y Visualización
El flujo de diseño digital requiere validar el comportamiento del procesador mediante síntesis lógica, simulación funcional y análisis de señales. Aquí se describe cómo emplear herramientas como Yosys, Icarus Verilog, ngspice y GTKWave para comprobar la correcta operación del núcleo.  
✅[Ver más detalles en `docs/simulacion.md`](docs/simulacion.md)✅

## 🔬 Diseño en Tiny Tapeout
Tiny Tapeout ofrece un entorno colaborativo para llevar diseños digitales a fabricación en silicio. En esta sección se explica cómo integrar el procesador Femto RISC-V 32 en el flujo de Tiny Tapeout, incluyendo restricciones, preparación de archivos y envío del diseño.  
✅[Ver más detalles en `docs/tinytapeout.md`](docs/tinytapeout.md)✅


## Conclusiones

### 1. Importancia de la simulación y verificación temprana
La simulación en **Xyce** y demuestra la relevancia de validar cada bloque antes de integrarlo al sistema completo. Este enfoque modular permite detectar errores de descripción RTL, confirmar la correcta propagación de señales y garantizar que las operaciones aritméticas críticas funcionen como se espera. Al realizar simulaciones aisladas y luego post-síntesis, se asegura que el diseño no solo es correcto en teoría, sino también en su implementación práctica dentro de un flujo de síntesis y mapeo hacia celdas estándar. Esta metodología reduce riesgos, facilita la depuración y fortalece la reproducibilidad del proyecto, lo cual es esencial en entornos académicos y de fabricación real como para Tiny Tapeout.

### 2. Síntesis y descripción de hardware como puente entre teoría y práctica
El proceso de síntesis con **Yosys**, acompañado de la verificación de conexiones y el mapeo a librerías estándar como las de Sky130, evidencia cómo la descripción en Verilog se transforma en un netlist optimizado y físicamente realizable. La documentación detallada de los módulos (`femto.v`, `mult_32.v`, periféricos UART, SPI RAM y SPI Flash) muestra que cada bloque cumple una función específica dentro del procesador Femto RISC-V, y que la lógica de chip-select integra todo en un espacio de direcciones coherente. Este puente entre la teoría del procesador y la práctica de síntesis asegura que el diseño sea portable, escalable y verificable, permitiendo que la arquitectura se materialice en silicio con fidelidad respecto a la intención original del diseñador.

### 3. Fabricación en Tiny Tapeout como culminación del flujo
La posibilidad de llevar el diseño del Femto RISC-V a **Tiny Tapeout** representa la culminación del flujo de diseño digital: desde la descripción RTL, pasando por simulación, síntesis, verificación post-síntesis y documentación, hasta la fabricación física en un chip real. Tiny Tapeout democratiza el acceso a la fabricación de circuitos integrados, permitiendo que proyectos como este pasen de ser ejercicios académicos a prototipos tangibles. La integración del multiplicador de 32 bits, la correcta gestión de periféricos y la validación del top module `femto.v` aseguran que el procesador pueda ejecutarse en hardware real. Este proceso no solo refuerza la teoría aprendida, sino que también brinda experiencia práctica en el ciclo completo de diseño de sistemas digitales, desde la idea hasta el silicio, consolidando conocimientos en simulación, síntesis y verificación como pilares fundamentales.

## Referencias

- [Xyce Documentation & Tutorials – Sandia National Laboratories](https://xyce.sandia.gov/documentation-tutorials/)  
- [Ngspice User Manual (PDF oficial en SourceForge)](https://ngspice.sourceforge.io/docs/ngspice-manual.pdf)  
- [Tiny Tapeout – Fabricación accesible de ASICs](https://tinytapeout.com/)  
- [GitHub – BrunoLevy/learn-fpga](https://github.com/BrunoLevy/learn-fpga)  
- [Yosys Open SYnthesis Suite Documentation](https://yosyshq.net/yosys/documentation.html)
- [Repositorio oficial del curso - Temas Digitales Avanzados](https://github.com/cicamargoba/VLSI/tree/main)
