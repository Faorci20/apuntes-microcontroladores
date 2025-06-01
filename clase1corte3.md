# 📛 Apuntes de Clase – Módulo Captura / Comparación / PWM


**Trabajo realizado por:**
**Fabián Ortiz, Daniel Cruz y Santiago Bautista**
**Curso:** Microcontroladores – Cuarto Semestre
**Docente:** Ing. Jorge Eduardo Cote Ballesteros
**Año:** 2025

---

## 🧭 Introducción General

Durante esta clase se explicó el uso y configuración de los módulos especiales que poseen los microcontroladores para la gestión de señales de tiempo y control: **Captura**, **Comparación** y **PWM (Modulación por Ancho de Pulso)**. Estos tres bloques son fundamentales para sistemas embebidos en tiempo real, como sensores, actuadores, motores, generación de señales y más.
En esta sesión se abordaron tres de los módulos más importantes que incluyen los microcontroladores modernos: Captura, Comparación y Modulación por Ancho de Pulso (PWM). Cada uno de ellos cumple funciones esenciales en sistemas de control y aplicaciones de ingeniería electrónica donde es necesario medir tiempos con precisión, generar señales periódicas o simular salidas analógicas desde pines digitales. El correcto entendimiento y configuración de estos módulos es fundamental para lograr una comunicación efectiva entre el microcontrolador y el mundo físico.

Estos periféricos están diseñados para aprovechar al máximo los recursos del microcontrolador y facilitar tareas que de otra manera requerirían mayor complejidad de programación o recursos externos. Por ejemplo, sin el módulo PWM, sería muy difícil regular la velocidad de un motor de forma precisa sin hardware adicional. Igualmente, sin el módulo de captura, no podríamos detectar eventos en tiempo real con alta resolución.

---

## 🔹 Módulo de Captura (Capture)

### 📌 ¿Qué es?

Es un periférico del microcontrolador que permite detectar un evento (como un flanco de una señal) y registrar automáticamente el valor de un timer en ese momento exacto.

El módulo de captura permite al microcontrolador almacenar el valor actual de un temporizador cuando ocurre un evento determinado. Esto es extremadamente útil para medir la duración entre eventos externos, tales como pulsos generados por sensores, botones, o señales de otros dispositivos digitales.

Funciona asociando un pin de entrada (por ejemplo, RC2 en muchos microcontroladores PIC) a una unidad de captura. Cuando ese pin detecta un cambio de nivel lógico (por ejemplo, de 0 a 1), el módulo automáticamente registra el valor actual del temporizador. Esto puede generar una interrupción para que el procesador actúe inmediatamente, o simplemente almacenar el valor para su posterior análisis.

### ⚙️ Características clave

| Característica      | Detalle                                                |
| ------------------- | ------------------------------------------------------ |
| Fuente del evento   | Externa o interna (como ADC en Atmega16)               |
| Evento detectado    | Flanco de subida o bajada                              |
| Registro de captura | Guarda valor del timer (TMR1, TMR3, etc.)              |
| Interrupciones      | Puede activar interrupción al detectar evento          |
| Uso común           | Medición de tiempo entre eventos, frecuencia, TON/TOFF |

### 🧩 Aplicaciones comunes
Medición de la frecuencia de una señal periódica externa.

Cálculo del tiempo de duración de un pulso.

Sincronización con señales digitales externas en tiempo real.

Captura de eventos generados por sensores de velocidad o distancia.

Aplicaciones de cronometraje en sistemas industriales.


🧠 Apuntes del profe:

“Este módulo es como una cámara fotográfica que toma la instantánea del tiempo exacto en que sucede algo importante.”

“Recuerden que la captura también puede venir desde eventos internos, no siempre desde los pines.”

“Una de las preguntas típicas de examen es: ¿cómo medirías la frecuencia de una señal con un solo pin? Aquí está la respuesta.”



---

## 🧲 Granularidad y Precisión

### 📀 ¿Qué es la granularidad del timer?

La granularidad representa la resolución mínima con la que puede trabajar un timer, es decir, la unidad de tiempo más pequeña que puede medir entre dos eventos consecutivos. Esta granularidad depende directamente de la frecuencia del reloj del sistema y del prescaler configurado.

Por ejemplo, con un reloj de 4 MHz y un prescaler de 1:4, cada ciclo del timer corresponde a 1 microsegundo (µs). Si los eventos que se desean capturar ocurren con más rapidez que esta unidad de tiempo, el sistema no podrá detectarlos adecuadamente.

La elección del prescaler debe equilibrar entre rango de medición (tiempo total que puede medirse sin desbordamiento) y resolución (precisión del tiempo medido). En aplicaciones prácticas, se requiere calcular cuánto tiempo representa un conteo del timer y estimar si es suficiente para los eventos del sistema.

#### Ejemplo:

Si el reloj del sistema es 4MHz y el prescaler es 1:4:
$\text{Granularidad} = \frac{1}{4 \text{ MHz} / 4} = 1\mu s$

#### 🧠 Apuntes del profe:

“Un prescaler muy alto te deja ciego a los detalles. Uno muy bajo te limita el alcance.”

“Usen prescalers pequeños cuando la precisión es crítica, como en sensores de alta frecuencia.”

“No olviden que el timer se puede desbordar, ¡y eso hay que manejarlo con cuidado!”
---

## 🔸 Módulo de Comparación (Compare)

### 📌 ¿Qué hace?

El módulo de comparación se utiliza cuando se desea generar un evento en una salida digital al alcanzar un valor específico en un temporizador. A diferencia del módulo de captura (que reacciona ante eventos externos), el de comparación actúa de manera proactiva: se programa un valor y cuando el timer lo alcanza, se ejecuta una acción.

Por ejemplo, se puede encender un LED exactamente 10 milisegundos después de que empieza una operación, sin tener que hacer un delay manual. El módulo de comparación puede programarse para generar interrupciones, hacer flancos en salidas o reiniciar contadores automáticamente.

### 🧼 Tabla de configuración

| Elemento                | Descripción                                       |
| ----------------------- | ------------------------------------------------- |
| Timer usado             | TMR1 o TMR3 (16 bits)                             |
| Registro de comparación | Comparado continuamente con el valor del timer    |
| Tipos de evento         | Salida alta, baja, toggle, latch, interrupción    |
| Reset del timer         | Opcional, al alcanzar el valor se puede reiniciar |

### 🗒️ Ejemplo funcional

* Configuramos un valor de comparación de 20000.
* Cuando el timer TMR1 llegue a ese valor:

  * Se activa un pin de salida.
  * Se reinicia el timer (opcional).
  * Se ejecuta una interrupción si está habilitada.

#### 🧠 Apuntes del profe:

* “En comparación, ustedes programan el momento, como una alarma.”
* “Sirve para señales cuadradas exactas.”

---

## 🔶 PWM – Pulse Width Modulation

### 📌 ¿Qué es?

La modulación por ancho de pulso (PWM) es una técnica utilizada para controlar la cantidad de energía entregada a una carga sin necesidad de usar conversiones analógicas reales. Se basa en generar una señal cuadrada digital cuya proporción de tiempo en estado alto respecto al periodo total (duty cycle) varía para representar un valor.

PWM se utiliza cuando se quiere controlar la velocidad de un motor, la intensidad de una lámpara LED, el voltaje de salida simulado o incluso la posición de un servo. Al variar el duty cycle, el promedio de la energía entregada cambia, sin cambiar la frecuencia de la señal.

### 📊 Fórmulas clave

1. **Duty Cycle**
   $DC = \frac{TON}{TON + TOFF} \times 100 = \frac{TON}{T} \times 100$

2. **Valor promedio de salida (si A es amplitud)**
   $V_{avg} = \frac{A \cdot TON}{T}$

### 🧼 Tabla de valores PWM

| DC (%) | Señal equivalente (a 5V) |
| ------ | ------------------------ |
| 0%     | 0V                       |
| 25%    | 1.25V                    |
| 50%    | 2.5V                     |
| 75%    | 3.75V                    |
| 100%   | 5V                       |

#### 🧠 Apuntes del profe:

* “PWM está en todos lados: ventiladores, LED, motores, DAC.”
* “No usen resistencias para bajar potencia, usen PWM.”

---

## 🔧 Implementación de PWM en PIC18F4550

El PIC18F4550 incorpora módulos CCP (Capture/Compare/PWM) que permiten generar señales PWM con resolución de hasta 10 bits. Utiliza el temporizador TMR2 como base de tiempo y dos registros principales:

PR2: define el periodo de la señal PWM.

CCPRxL + CCPxCON: determinan el duty cycle.

La configuración correcta implica establecer el periodo deseado, seleccionar el prescaler adecuado y configurar el pin correspondiente (RC2 para CCP1, por ejemplo) como salida.

### 📌 Recursos

| Módulo     | CCP1 o CCP2          |
| ---------- | -------------------- |
| Timer      | TMR2                 |
| Resolución | 10 bits              |
| Registros  | PR2, CCPRxL, CCPxCON |

### 🛠️ Pasos para configurar PWM

1. Configurar `PR2` para establecer el periodo.
2. Configurar `CCPR1L` para el duty cycle.
3. Configurar `TRISCbits.TRISC2 = 0` para salida.
4. Inicializar `T2CON` con prescaler y encender TMR2.
5. Configurar `CCP1CON` para modo PWM.

### 📋 Cálculo ejemplo de PR2 (1 kHz, Fosc = 4MHz, prescaler = 16)

```c
PR2 = (1 / (1kHz * 4 * Tosc * Prescaler)) - 1;
// Tosc = 1 / 4MHz = 0.25us
// Resultado: PR2 = 61.5 ≈ 62
```

---

### 🕛 Código completo de configuración PWM

```c
void configurarPWM() {
    TRISCbits.TRISC2 = 0;        // RC2 como salida (PWM)
    PR2 = 62;                    // Periodo para 1kHz
    CCPR1L = 31;                 // Duty Cycle del 50%
    CCP1CON = 0b00001100;        // PWM mode
    T2CON = 0b00000111;          // TMR2 on, prescaler 16
}
```

#### 🧠 Apuntes del profe:

* “No olviden habilitar el pin de salida. Si lo dejan como entrada, no sale PWM.”
* “Cambios en `CCPR1L` se reflejan solo cuando `TMR2 == PR2`.”

---

## 🧪 Observaciones del laboratorio

| Observación           | Resultado                    |
| --------------------- | ---------------------------- |
| Periodo               | 500 us                       |
| Tiempo en alto (TON)  | 105 us                       |
| Señal en osciloscopio | Estable y repetitiva         |
| Cambios de duty cycle | Aplicados tras `TMR2 == PR2` |

---

## ❓ Dudas frecuentes

| Pregunta                                        | Respuesta rápida                     |
| ----------------------------------------------- | ------------------------------------ |
| ¿Qué pasa si configuro mal el prescaler?        | El PWM será errático o incorrecto.   |
| ¿Puedo usar PWM para audio?                     | Sí, con filtrado pasabajos adecuado. |
| ¿Qué módulo usar para medir frecuencia externa? | El módulo de captura (Capture).      |

---

## 🧠 Apuntes adicionales del profesor (completos)

* “Usen prescaler pequeños para mayor resolución.”
* “Comparación es como ponerle una alarma al timer.”
* “PWM está en todos los sistemas modernos: eficiencia energética pura.”
* “TRIS mal configurado = señal muerta.”
* “No crean que el multímetro sirve para PWM, usen osciloscopio.”
* “El PR2 define la frecuencia; CCPRxL define el duty.”
* “Duty cycle de 100% = señal constante en alto.”
* “El secreto de un buen sistema embebido es medir bien y actuar mejor.”

---

## ✅ Conclusiones

* **Captura** permite registrar eventos precisos.
* **Comparación** permite reaccionar en momentos predefinidos.
* **PWM** convierte una señal digital en una salida con control analógico aparente.
* Dominar estos módulos es fundamental en el desarrollo de sistemas embebidos profesionales.
* Captura, Comparación y PWM son herramientas indispensables en sistemas embebidos. Permiten interactuar con el mundo real de manera precisa y eficiente. Su correcto uso mejora la fiabilidad, reduce el consumo energético y permite crear aplicaciones más dinámicas y robustas, desde sistemas de control automático hasta soluciones para la industria y el hogar.

Estos módulos forman parte de los cimientos para implementar sistemas de control en tiempo real, y su dominio representa una gran ventaja competitiva para cualquier ingeniero electrónico o desarrollador de sistemas embebidos.

---

📘 **Fin de los apuntes – Clase completa**
