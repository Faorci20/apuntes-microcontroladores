# 📚 Apuntes de Clase – Comunicación Serial SCI (UART)

**Trabajo realizado por:**
**Fabián Ortiz, Daniel Cruz y Santiago Bautista**
**Curso:** Microcontroladores – Cuarto Semestre
**Docente:** Ing. Jorge Eduardo Cote Ballesteros
**Año:** 2025

---

## 🧭 Introducción General

En la clase de hoy se desarrollaron los fundamentos de uno de los pilares esenciales de los sistemas de comunicación digital: la comunicación serial, y más específicamente el módulo SCI (Serial Communication Interface) que se implementa en muchos microcontroladores como UART (Universal Asynchronous Receiver Transmitter).

Esta forma de comunicación permite la transferencia de datos entre dos dispositivos usando una cantidad mínima de líneas físicas, haciéndola ideal para conexiones simples, de largo alcance, o donde el ahorro de pines sea una prioridad. Además, su implementación por hardware y sus múltiples opciones de configuración hacen que UART siga siendo uno de los métodos más utilizados tanto en aplicaciones domésticas como industriales.

La UART es fundamental en proyectos de sistemas embebidos, comunicación entre dispositivos periféricos y procesadores, o incluso en transmisiones de datos en entornos industriales. Entender su estructura y funcionamiento es clave para lograr sistemas de comunicación confiables, eficientes y escalables.

La clase cubrió desde los principios de comunicación digital, hasta el diseño e implementación práctica de un sistema de transmisión UART en el microcontrolador PIC18F4550, incluyendo ejemplos, configuración de registros, problemas comunes y cómo resolverlos.


---

## 🔌 ¿Qué es la Comunicación Serial?

La comunicación serial consiste en el envío de bits uno tras otro a través de un único canal o línea de transmisión. A diferencia de la comunicación paralela, donde múltiples bits se transmiten al mismo tiempo a través de múltiples líneas, la comunicación serial es más eficiente en cuanto a uso de pines y es más robusta frente a interferencias, especialmente a largas distancias.

### 📈 Características principales:

* Solo necesita 2 líneas principales: TX (transmisión) y RX (recepción).
* Utiliza un protocolo que define la estructura de la trama.
* Puede funcionar en modo **asíncrono** (sin reloj compartido) o **síncrono** (con reloj compartido).

### Comparación: Serial vs Paralela

| Característica    | Comunicación Serial | Comunicación Paralela             |
| ----------------- | ------------------- | --------------------------------- |
| Bits transmitidos | Uno por vez         | Múltiples al mismo tiempo         |
| Costo en hardware | Bajo (menos pines)  | Alto (más pines)                  |
| Velocidad         | Más lenta           | Más rápida (en distancias cortas) |
| Robustez          | Alta frente a ruido | Baja a interferencias externas    |

---

## 🔁 Modos de Transmisión

### Full Dúplex

Permite transmitir y recibir datos simultáneamente. Cada dirección de comunicación tiene su línea propia (TX y RX).

### Half Dúplex

Solo puede haber una transmisión activa a la vez. La línea de comunicación debe compartirse.

### Simplex

La comunicación es unidireccional. Un dispositivo transmite y el otro solo recibe.

---

## 🔀 Tipos de Comunicación

### 🟢 Comunicación Síncrona

* Se requiere una línea adicional de reloj.
* Permite una sincronización precisa entre emisor y receptor.
* Mayor velocidad de transmisión.
* Ejemplo de protocolo: **SPI**.

### 🔵 Comunicación Asíncrona

* No requiere reloj externo.
* Usa bits de inicio y parada para sincronización.
* Es más tolerante a diferencias mínimas en el tiempo.
* Ejemplo clásico: **UART**.

#### 🧠 Apuntes del profe:

* “En UART, cada extremo tiene su propio reloj, así que tienen que ‘acordar’ la velocidad.”
* “SPI es más rápido, pero UART es más fácil de implementar para conexiones simples.”

---

## 🔗 Topologías de Comunicación

### Punto a Punto

* Solo dos dispositivos están conectados.
* No se requiere direccionamiento.
* Muy común en UART.

### Bus Compartido

* Varios dispositivos comparten el mismo canal.
* Requiere algún sistema de dirección o prioridad.

---

## 🧭 Arquitectura de Comunicación

### Maestro - Esclavo

* Solo el maestro puede iniciar la comunicación.
* Se usa en protocolos como SPI e I2C.

### Todos Iguales (Peer to Peer)

* Todos los nodos pueden iniciar comunicación.
* Requiere manejo de colisiones o arbitraje del canal.

---

## ⚡ Condiciones Eléctricas de la Transmisión

### Transmisión Single-Ended

* Mismo potencial de tierra compartido.
* Más susceptible al ruido a largas distancias.

### Transmisión Diferencial

* Usa la diferencia de voltaje entre dos líneas.
* Mucho más robusta frente a interferencias.

---

## 📡 Protocolos Comunes en Microcontroladores

| Protocolo | Tipo      | Arquitectura      | Características                                    |
| --------- | --------- | ----------------- | -------------------------------------------------- |
| UART/SCI  | Asíncrono | Punto a punto     | Simple, fiable, bajo costo.                        |
| SPI       | Síncrono  | Maestro - Esclavo | Rápido, requiere más pines.                        |
| I2C       | Síncrono  | Maestro - Esclavo | Dirección por hardware, solo 2 líneas.             |
| USB       | Mixto     | Maestro dinámico  | Hot-plug, plug-and-play, protocolos más complejos. |

---

## 📟 UART – Universal Asynchronous Receiver Transmitter

### Funciones:

* Transmitir y recibir datos seriales.
* Puede funcionar en modo **full dúplex**.
* Usa dos líneas: TX y RX.
* Integra registros internos para controlar la comunicación.

### Parámetros Configurables:

| Parámetro       | Descripción                                                  |
| --------------- | ------------------------------------------------------------ |
| Bits de datos   | Entre 5 y 9 bits según el microcontrolador.                  |
| Bit de paridad  | Verificación de errores (par, impar o sin paridad).          |
| Bit de parada   | Uno o dos bits de detención.                                 |
| Tasa de baudios | Velocidad en bits por segundo (bps). Configurable vía SPBRG. |

---

## 📋 Estándares Seriales

* **RS232**: Clásico estándar para comunicación con PCs.
* **RS422 / RS485**: Versiones diferenciales para redes más robustas.
* Codificación **NRZ** (Non Return to Zero): no hay retorno a 0 entre bits.

---

## 🔠 Nomenclatura Común UART

Se expresa con la forma: **D E O N S**

### Ejemplo: **8E1**

* 8 bits de datos
* Paridad par (Even)
* 1 bit de parada

---

## 🔄 USART – Universal Synchronous Asynchronous Receiver Transmitter

* Extensión del UART que permite trabajar en modo síncrono.
* Agrega una línea de reloj compartida.
* Proporciona mayor velocidad.

---

## ⚙️ EUSART en el PIC18F4550

### Características:

* Se conoce como **Enhanced USART**.
* Modo síncrono o asíncrono.
* Soporta **full dúplex**.
* Ideal para comunicación con PCs, sensores, otros microcontroladores.

### Pines de comunicación:

* **RC6** → TX (Transmisión)
* **RC7** → RX (Recepción)

---

## 🧾 Registros Clave

| Registro           | Función                                    |
| ------------------ | ------------------------------------------ |
| **TXSTA**          | Control de transmisión                     |
| **RCSTA**          | Control de recepción                       |
| **BAUDCON**        | Control de tasa de baudios y configuración |
| **SPBRG / SPBRGH** | Configuran la velocidad de transmisión     |

---

## 🛠️ Pasos para Configuración UART Asíncrona

1. Configurar RC6 como salida (TX) y RC7 como entrada (RX) usando TRISC.
2. Definir la tasa de baudios en **SPBRG**.
3. Activar la transmisión con **TXEN** en TXSTA.
4. Habilitar la recepción con **SPEN** en RCSTA.
5. Configurar BAUDCON si se requiere alta precisión o modos especiales.

---

## 📐 Cálculo de SPBRG

$$
SPBRG = rac{F_{osc}}{16 	imes BaudRate} - 1
$$

> Se redondea al entero más cercano.

> Luego se calcula el **error de baudios**:

$$
Error\% = \left( rac{Baud_{real} - Baud_{ideal}}{Baud_{ideal}} 
ight) 	imes 100
$$

#### 🧠 Apuntes del profe:

* “Una tasa de error menor al 2% es aceptable.”
* “Siempre verifiquen si su cristal permite calcular una tasa de baudios limpia.”

---

## 🔧 Ejemplo: Comunicación entre dos PIC18F4550

* Un PIC transmite desde TX (RC6).
* El otro recibe por RX (RC7).
* Ambos deben estar configurados con:

  * Mismo baudrate
  * Misma cantidad de bits de datos, paridad y bits de parada
* El transmisor envía datos cada cierto tiempo (ej. con interrupción de temporizador).
* El receptor puede usar polling o interrupción para capturar el dato.

---

## 🧠 Más Apuntes del Profe

* “La UART es el punto de partida de toda la comunicación moderna en microcontroladores.”
* “Dominen bien los registros TXSTA, RCSTA y BAUDCON. Son clave para resolver errores.”
* “Con una buena configuración de UART, se pueden comunicar sin problemas con sensores, módulos GSM, Bluetooth, GPS y más.”

---

## ✅ Conclusiones

La comunicación serial asíncrona mediante UART (SCI) es una técnica fundamental en sistemas embebidos. Permite enviar datos entre dispositivos con eficiencia, precisión y bajo costo. Dominar su configuración en el microcontrolador PIC18F4550 abre las puertas al desarrollo de sistemas más complejos, como redes de sensores, interfaces con computadores, y control de periféricos inteligentes.

En resumen, UART es el lenguaje universal de los microcontroladores.

---

📘 **Fin de los apuntes – Comunicación Serial SCI (UART)**
