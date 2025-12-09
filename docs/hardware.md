## 📂 Descripción y Construcción de los Archivos Verilog (.v)

El diseño del procesador **Femto RISC-V** se organiza en múltiples archivos Verilog (`.v`), cada uno encargado de describir módulos específicos de la arquitectura. Estos archivos representan desde bloques aritméticos básicos hasta unidades de control más complejas, y su correcta estructuración es esencial para garantizar la síntesis y simulación sin errores.  

La construcción de cada archivo sigue una metodología modular:  
- Se definen las **interfaces** (entradas y salidas) que conectan el módulo con el resto del sistema.  
- Se describe la **lógica interna** mediante asignaciones combinacionales o secuenciales.  
- Se asegura la **compatibilidad jerárquica**, de modo que los módulos puedan integrarse fácilmente en el diseño superior.  

En esta sección se documentarán los archivos más importantes, explicando su función dentro del procesador y cómo se construyen paso a paso. Esto permitirá comprender la relación entre los bloques individuales y el sistema completo, además de facilitar futuras modificaciones o depuración del código.  
