# Apuntes de Clase: Microprocesadores y Microcontroladores

**Estudiantes:** Fabian Ortiz y Daniel Cruz  
**Profesor:** Ing. Jorge Eduardo Cote Ballesteros  
**Curso:** Cuarto Semestre - Microcontroladores  

## 1. Introducción a los Microprocesadores y Microcontroladores

Los microprocesadores y microcontroladores son componentes fundamentales en la informática y la automatización. Mientras que los microprocesadores se usan en computadoras y sistemas de propósito general, los microcontroladores están diseñados para aplicaciones específicas en sistemas embebidos.

### ¿Qué es un Microprocesador?
Un **microprocesador** es un circuito integrado que ejecuta instrucciones de un programa almacenado en memoria. Actúa como el cerebro de una computadora o sistema computacional, procesando datos y controlando dispositivos.

Se le conoce como una máquina de estados de propósito general, ya que su arquitectura permite ejecutar múltiples tareas dependiendo del software que se cargue en él.

###  Evolución Histórica del Microprocesador

1. **1971 – Intel 4004:** Primer microprocesador comercial. 4 bits, 2300 transistores, 108 KHz.  
2. **1972 – Intel 8008:** Primer microprocesador de 8 bits.  
3. **1973 – Intel 8080:** 500,000 operaciones por segundo.  
4. **1975 – Motorola:** Microprocesadores de bajo costo.  
5. **1976 – Intel 8048 y Motorola 6805R2:** Primeras microcomputadoras de un solo chip.  
6. **1980s – División entre microprocesadores y microcontroladores.**  

###  Componentes de un Microprocesador
- **Unidad de Control (UC):** Coordina instrucciones y flujo de datos.  
- **Unidad Aritmético Lógica (ALU):** Realiza cálculos y operaciones lógicas.  
- **Registros:** Pequeñas unidades de memoria temporal.  
- **Buses:** Comunicación interna del sistema.  

###  Funcionamiento de un Microprocesador
1. Obtiene la instrucción de memoria.  
2. La decodifica.  
3. Ejecuta la operación en la ALU.  
4. Almacena el resultado.  

### Set de Instrucciones
- **CISC:** Instrucciones complejas (x86 de Intel).  
- **RISC:** Instrucciones simples y rápidas (ARM en dispositivos móviles).  

###  Arquitecturas
- **Von Neumann:** Memoria compartida (puede generar cuellos de botella).  
- **Harvard:** Memoria separada para datos y programa (mayor rendimiento).  

### Memoria
- **Volátil:** RAM, SRAM, DRAM.  
- **No Volátil:** ROM, PROM, EPROM, EEPROM, Flash, NVRAM.  

### Características de un Microprocesador
- Frecuencia del reloj.  
- Tamaño del bus de datos e instrucciones.  
- Set de instrucciones.  
- Memoria de programa.  
- Entradas y salidas.  

## 2. Introducción a los Microcontroladores

### Definición de Microcontrolador
Un **microcontrolador** es un circuito integrado programable que incluye CPU, memoria y periféricos de entrada/salida en un solo chip. Se usa en sistemas embebidos para controlar dispositivos en tiempo real.

### Diferencias entre Microprocesador y Microcontrolador

| Característica | Microcontrolador | Microprocesador |
|--------------|-----------------|-----------------|
| Longitud de palabra | 8, 16, 32 bits | 16, 32, 64 bits |
| Memoria | Integrada en el chip | Externa |
| Consumo de energía | Bajo | Alto |
| Uso | Control en tiempo real | Computación general |
| Susceptibilidad a ruido | Baja | Alta |

### Aplicaciones de los Microcontroladores
- **Robótica:** Control de extremidades y sensores.  
- **Automotriz:** Climatización, inyección electrónica, alarmas.  
- **Doméstico:** Televisores, lavadoras, microondas.  
- **Industrial:** Controladores programables y automatización.  

### Clasificación de Microcontroladores
- **Por longitud de palabra:** 4, 8, 16 y 32 bits.  
- **Por arquitectura:** Harvard o Von Neumann.  

### Ejemplos de Microcontroladores por Fabricante

| Fabricante | Familia |
|------------|---------|
| Motorola | Freescale |
| Microchip | PIC |
| Hitachi | H8 |
| Atmel | AVR |
| Texas Instruments | MSP430 |
| Intel | MSC |
