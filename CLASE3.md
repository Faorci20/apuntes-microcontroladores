# Apuntes de Clase: Conversión Analógica-Digital (ADC)

**Estudiantes:** Fabian Ortiz y Daniel Cruz  
**Profesor:** Ing. Jorge Eduardo Cote Ballesteros  
**Curso:** Cuarto Semestre - Microcontroladores  

---

##  1. Introducción a las Señales Digitales y Analógicas

En sistemas electrónicos, las señales pueden ser **digitales o analógicas**:

- **Señal Digital:** Solo tiene dos valores posibles, **0 (bajo)** o **1 (alto)**. Se usa en circuitos lógicos y microcontroladores.  
- **Señal Analógica:** Puede tomar **cualquier valor** dentro de un rango. Ejemplo: temperatura, luz, sonido.  

Las señales analógicas deben convertirse a digitales para ser procesadas por los microcontroladores.  

---

##  2. Diferencias entre Entradas Digitales y Analógicas

| Tipo de Entrada | Características |
|----------------|----------------|
| **Digital** | Solo admite valores 0 o 1. Usa un Schmitt Trigger para definir el nivel lógico. |
| **Analógica** | Puede tomar cualquier valor dentro de un rango definido. Necesita un ADC para ser procesada. |

Si se conecta una señal analógica a una entrada digital, el microcontrolador la forzará a un valor lógico (0 o 1) mediante un **Schmitt Trigger**.  

---

##  3. Conversión Digital a Analógica (DAC)

Algunos microcontroladores pueden generar **señales analógicas** desde valores digitales:

- **Mediante PWM (Modulación por Ancho de Pulso):** Se genera una señal cuadrada con un filtro que suaviza la onda.  
- **Usando un Circuito de Ponderación Resistiva:** Cada bit se convierte en una salida de voltaje ponderada.  

###  Ejemplo de Conversión DAC con PWM
```c
PWM1_SetDuty(128); // PWM con 50% de ciclo de trabajo
```

---

## 4. Conversión Analógica a Digital (ADC)

Los microcontroladores requieren **convertir señales analógicas en valores digitales** para poder procesarlas. Esto se hace mediante un **Conversor Analógico-Digital (ADC)**.  

- **Ejemplo de Uso:** Sensores de luz, temperatura, voltaje, etc.  
- **Proceso:** La señal se muestrea y se representa en valores discretos.  

###  Procedimiento de Conversión ADC
1. **Muestreo:** Se toman valores de voltaje en intervalos regulares.
2. **Cuantización:** Se asignan valores discretos a cada nivel de voltaje.
3. **Codificación:** Se representa el valor digital en código binario.

Ejemplo de codificación para un ADC de **2 bits**:

| Voltaje (V) | Binario |
|------------|--------|
| 0.00V | 00 |
| 0.75V | 01 |
| 1.50V | 10 |
| 2.25V | 11 |

---

##  5. Tipos de Conversores ADC

| Tipo de ADC | Características |
|------------|----------------|
| **Flash Converter** | Muy rápido, pero costoso. Usa comparadores para cada nivel. |
| **Tracking Converter** | Usa un contador que sigue el voltaje de entrada. Lento para cambios rápidos. |
| **Successive Approximation Converter** | El más común en microcontroladores. Usa un registro de aproximación sucesiva. |

---

##  6. Características del ADC en Microcontroladores

- **Canales Múltiples:** Un solo ADC multiplexa varias entradas.  
- **Modos de Conversión:** Simple, continuo o por interrupción.  
- **Referencia de Voltaje:** Se puede configurar para definir el rango de medición.  
- **Codificación de Señales Negativas:** Se usa complemento a 2 o exceso.  

Ejemplo: **PIC18F4550**

- Resolución: **10 bits**.
- Entradas: **10 pines (28 pines) o 13 pines (40/44 pines)**.
- Rango de entrada: **0-5V, 0-3V o -3V a 3V**.

---

##  7. Configuración del ADC en MPLAB (PIC)

###  Pasos para Configurar el ADC
1. Configurar las entradas en modo analógico y la referencia de voltaje.
2. Seleccionar el canal ADC.
3. Configurar el tiempo de adquisición.
4. Ajustar el reloj de conversión.
5. Habilitar el conversor ADC.
6. Iniciar la conversión.
7. Leer los valores de los registros.

Ejemplo de Código en C:
```c
ADCON0 = 0x01; // Habilitar ADC en canal AN0
ADCON1 = 0x0E; // Configurar referencia de voltaje
ADCON2 = 0xA9; // Configurar tiempo de adquisición
GO_nDONE = 1;  // Iniciar conversión
while(GO_nDONE); // Esperar finalización
resultado = ((ADRESH << 8) + ADRESL); // Leer el resultado
```

---

##  8. Tiempo de Conversión y Resolución

El tiempo de conversión depende de:
- **Frecuencia de reloj.**
- **Tiempo de adquisición.**
- **Configuración del prescaler.**

Ejemplo de cálculo para un **PIC18F4550 con cristal de 4MHz**:

```math
T_{osc} = \frac{4}{F_{osc}} = \frac{4}{4MHz} = 1µs
```

Para una conversión estable, el **TAD mínimo** debe ser **0.8µs**, y:

```math
T_{AD} = \frac{ADCS + 1}{F_{OSC} * 2}
```

Si **ADCS = 6**, entonces:

```math
T_{AD} = \frac{6+1}{8MHz} = 0.87µs
```

---

##  9. Conclusión

El **Conversor Analógico-Digital (ADC)** es un módulo esencial en los microcontroladores para procesar señales analógicas como temperatura, voltaje o sonido. Comprender su configuración y optimización permite una mejor precisión y eficiencia en sistemas embebidos.
