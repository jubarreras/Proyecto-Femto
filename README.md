# Femto RISC-V 32

## 📖 Introducción
En este informe se presenta el diseño y análisis de un procesador basado en la arquitectura RISC-V, tomando como modelo base la familia FemtoRV. El diseño modular y bien documentado del FemtoRV ofrece una visión clara de las etapas involucradas en el procesamiento de instrucciones y la manipulación de datos, elementos esenciales para comprender y desarrollar procesadores modernos y eficientes, por medio de diversas herramientas de software se mostrará cómo se sintetiza, simula y valida el funcionamiento del procesador, permitiendo observar de manera práctica el flujo completo de diseño digital. La base conceptual y arquitectónica del proyecto se apoya en la imagen que se muestra a continuación, la cual servirá como referencia para entender la organización interna del núcleo.

<p align="center">
  <img src="Images/Estrutucura_base.png" alt="Estructura FemtoRV" width="400"/>
</p>

<p align="center"><em>Figura 1. Arquitectura base del procesador FemtoRV</em></p>

En la imagen presentada del diagrama de bloques del procesador RV32I se pueden identificar varios módulos principales, cada uno con una función específica. La Unidad de Comunicaciones (ubicada a la izquierda) se encarga de las transferencias de datos entre el procesador y la memoria, gestionando operaciones de carga y almacenamiento (load/store) para diferentes tamaños de dato. El módulo central, que incluye registros como rs1, rs2 y el registro de destino (dest_reg), administra la ejecución de instrucciones aritméticas y lógicas, así como el flujo de datos internos. La ALU (Unidad Aritmético-Lógica) realiza operaciones matemáticas y lógicas de acuerdo con las instrucciones del programa, mientras que otros componentes, como el PC (Program Counter), controlan la secuencia de ejecución de instrucciones, incrementando su valor según el tipo de operación realizada (salto, llamada, instrucción secuencial, etc.).

El procesador FemtoRV se distingue por su simplicidad y eficiencia. En su variante más elemental, denominada FemtoRV-quark, implementa el conjunto de instrucciones RV32I utilizando únicamente unas 400 líneas de código Verilog, lo que facilita su estudio, despliegue y personalización, convirtiéndolo en un excelente punto de partida para este proyecto.

## 🛠️ Herramientas y Prerrequisitos
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

## 🔩 Descripción de Hardware
El procesador Femto RISC-V 32 se basa en una arquitectura modular inspirada en la familia FemtoRV. En esta sección se introduce la organización interna del núcleo, sus principales módulos y la forma en que interactúan para ejecutar el conjunto de instrucciones RV32I.  
👉 [Ver más detalles en `docs/hardware.md`](docs/hardware.md)

## 🧪 Síntesis, Simulación y Visualización
El flujo de diseño digital requiere validar el comportamiento del procesador mediante síntesis lógica, simulación funcional y análisis de señales. Aquí se describe cómo emplear herramientas como Yosys, Icarus Verilog, ngspice y GTKWave para comprobar la correcta operación del núcleo.  
👉 [Ver más detalles en `docs/simulacion.md`](docs/simulacion.md)

## 🖇️ Diseño en Tiny Tapeout
Tiny Tapeout ofrece un entorno colaborativo para llevar diseños digitales a fabricación en silicio. En esta sección se explica cómo integrar el procesador Femto RISC-V 32 en el flujo de Tiny Tapeout, incluyendo restricciones, preparación de archivos y envío del diseño.  
👉 [Ver más detalles en `docs/tinytapeout.md`](docs/tinytapeout.md)


## 📌 Conclusiones
<!-- Reflexiones finales, aprendizajes y próximos pasos -->

