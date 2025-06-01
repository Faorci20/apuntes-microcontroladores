# 📚 Apuntes de Clase – Comunicación I2C

**Trabajo realizado por:**
**Fabián Ortiz, Daniel Cruz y Santiago Bautista**
**Curso:** Microcontroladores – Cuarto Semestre
**Docente:** Ing. Jorge Eduardo Cote Ballesteros
**Año:** 2025

---

## 🌛 Introducción al I2C (Inter-Integrated Circuit)

El protocolo **I2C** fue desarrollado por Phillips para facilitar la comunicación entre circuitos integrados, como microcontroladores y periféricos digitales. Se trata de un **bus de comunicación serial, síncrono y half-dúplex**, que emplea una arquitectura maestro-esclavo.

### Características clave:

* Utiliza **solo dos líneas**:

  * `SCL`: Serial Clock Line
  * `SDA`: Serial Data Line
* Compatible con múltiples dispositivos conectados al mismo bus.
* Ideal para cortas distancias dentro de placas electrónicas.

---

## ⚡ Velocidades de Comunicación

I2C define distintos **modos de operación según la velocidad**:

| Modo            | Velocidad máxima |
| --------------- | ---------------- |
| Standard Mode   | 100 kbit/s       |
| Fast Mode       | 400 kbit/s       |
| High-Speed Mode | 3.4 Mbit/s       |

📌 *Nota del profe:*

> “En un mismo bus puedes tener dispositivos con distintas velocidades. El maestro ajusta su ritmo.”

---

## 🔗 Arquitectura y Funcionamiento

### 🧱 Estructura del bus:

* Todos los dispositivos comparten las líneas `SDA` y `SCL`.
* Pueden existir **múltiples maestros**, aunque comúnmente se usa uno.
* Cada esclavo tiene una **dirección única** (7 o 10 bits).

### Modos de dirección:

* **7 bits**: Los 4 más significativos son definidos por el fabricante.
* **10 bits**: Para ampliar el número de dispositivos conectables.

---

## 🧢 Niveles Lógicos I2C

| Estado lógico | Rango de voltaje relativo a VDD |
| ------------- | ------------------------------- |
| Bajo (LOW)    | -0.5 VDD a 0.3 VDD              |
| Alto (HIGH)   | 0.7 VDD a VDD + 0.5             |

* Se utiliza lógica directa.
* En **estado idle**, tanto `SCL` como `SDA` permanecen en nivel alto.

---

## 📏 Señales I2C y su Temporización

* Las señales I2C son de tipo open-drain.
* Se requiere que las resistencias pull-up estén bien dimensionadas.
* La temporización de las señales es fundamental para garantizar compatibilidad.
* El tiempo de subida (`t_rise`) no debe exceder los 1000 ns en modo estándar.

---

## 🧮 Niveles Lógicos I2C

| Estado lógico | Rango de voltaje relativo a VDD |
| ------------- | ------------------------------- |
| Bajo (LOW)    | -0.5 VDD a 0.3 VDD              |
| Alto (HIGH)   | 0.7 VDD a VDD + 0.5             |

* Se utiliza lógica directa.
* En **estado idle**, tanto `SCL` como `SDA` permanecen en nivel alto.
* Cuando ambos están en alto, el bus está libre.

---

## 📡 Proceso de Transmisión

1. El maestro inicia la transmisión bajando SDA con SCL alto (**condición START**).
2. Se transmite la dirección del esclavo + bit de lectura/escritura.
3. El esclavo responde con un **ACK (acknowledge)**.
4. El maestro o esclavo transmite datos.
5. Se confirma cada byte con un nuevo **ACK**.
6. El maestro finaliza la transmisión enviando la condición **STOP**.

```text
 START --> Dirección + R/W --> ACK --> Dato --> ACK --> STOP
```

---

## 🛠️ Estructura del Byte en I2C

```text
  MSB                           LSB
 ┌────┬────┬────┬────┬────┬────┬────┬────┐
 │  7 │  6 │  5 │  4 │  3 │  2 │  1 │ R/W│
 └────┴────┴────┴────┴────┴────┴────┴────┘
```

* 7 bits para dirección del esclavo.
* 1 bit para definir si se desea lectura (1) o escritura (0).

---

## 🧪 Código completo de ejemplo en C (Maestro)

```c
#include <xc.h>

#define _XTAL_FREQ 8000000

void I2C_Init(long freq) {
    SSPCON = 0x28;
    SSPSTAT = 0x00;
    SSPADD = (_XTAL_FREQ / (4 * freq)) - 1;
    TRISC3 = 1; // SCL como entrada
    TRISC4 = 1; // SDA como entrada
}

void I2C_Start() {
    SEN = 1;
    while(SEN);
}

void I2C_Stop() {
    PEN = 1;
    while(PEN);
}

void I2C_Write(unsigned char data) {
    SSPBUF = data;
    while(!SSPIF);
    SSPIF = 0;
}

unsigned char I2C_Read(void) {
    RCEN = 1;
    while(!SSPIF);
    SSPIF = 0;
    return SSPBUF;
}
```

---

## 🧠 Apuntes del Profe (Extras)

* “Si el esclavo no responde, revisa si la dirección es correcta y si tiene alimentación.”
* “El orden de los eventos es START → Dirección → ACK → Datos → ACK → STOP.”
* “No se olviden de colocar las resistencias pull-up, sin ellas no hay transmisión.”
* “Con Proteus pueden simular I2C fácilmente. Conectar SDA/SCL a múltiples esclavos.”
* “Si tienes un solo maestro, la implementación es mucho más sencilla.”


## 🛡️ Proceso de Transmisión

1. El maestro inicia la transmisión bajando SDA con SCL alto (**condición START**).
2. Se transmite la dirección del esclavo + bit de lectura/escritura.
3. El esclavo responde con un **ACK (acknowledge)**.
4. El maestro o esclavo transmite datos.
5. Se confirma cada byte con un nuevo **ACK**.
6. El maestro finaliza la transmisión enviando la condición **STOP**.

---

## 🧱 Trama de Comunicación I2C

```text
START ─ Dirección (7/10 bits) ─ R/W ─ ACK ─ DATA ─ ACK ─ ... ─ STOP
```

📌 *Dato importante:*

> “El ACK es vital. Si no se recibe, probablemente el esclavo no reconoció su dirección.”

---

## 🌀 Control de Velocidad desde el Esclavo

* Algunos esclavos pueden **modular la velocidad** de transmisión.
* Permiten al maestro **esperar** antes de continuar el flujo de datos.
* Últil cuando el esclavo requiere más tiempo para procesar datos.

---

## ⚙️ I2C en el PIC18F4550

El microcontrolador **PIC18F4550** utiliza el **módulo MSSP (Master Synchronous Serial Port)** para implementar I2C.

### Características del MSSP:

* Soporta **modo maestro y esclavo**.
* Genera interrupciones por condiciones START y STOP.
* Compatible con direcciones de 7 y 10 bits.
* Pines utilizados:

  * `RB1` – SCL
  * `RB0` – SDA

---

## 🔍 Modos de operación del MSSP

| Modo                    | Descripción                               |
| ----------------------- | ----------------------------------------- |
| Maestro                 | Genera reloj y controla comunicación      |
| Esclavo 7 bits          | Dirección fija, responde solo si coincide |
| Esclavo 10 bits         | Igual, pero para rangos mayores           |
| Interrupción Start/Stop | Notifica eventos clave en esclavo         |
| Firmware Controlado     | Comunicación personalizada en software    |

---

## 🔢 Cálculo de SSPADD (Velocidad)

Fórmula:

```
SSPADD = (Fosc / (4 * BitRate)) - 1
```

Ejemplo para 4 MHz y 100 kbit/s:

```
SSPADD = (4MHz / (4 * 100k)) - 1 = 9
```

---

## ⚖️ Pasos para configurar I2C en PIC18F4550

1. Configurar pines SDA y SCL como entradas (`TRISBbits.TRISB0 = 1`, etc).
2. Calcular y cargar `SSPADD` según velocidad deseada.
3. Configurar `SSPCON1` para modo maestro o esclavo.
4. Habilitar interrupciones (opcional).
5. Enviar señal START (`SEN = 1`).
6. Transmitir dirección del esclavo.
7. Esperar ACK.
8. Transmitir/recibir datos.

---

## 💪 Funciones comunes en C

```c
void I2C_Init(long freq);
void I2C_Start();
void I2C_Stop();
void I2C_Write(unsigned char data);
unsigned char I2C_Read();
void I2C_Ack();
void I2C_NAck();
```

📌 *Tip del profe:*

> “Siempre verifica si el esclavo responde antes de transmitir más datos.”

---

## 🧪 Ejemplo de Código (Maestro)

```c
void I2C_Init(long freq) {
    SSPCON1 = 0x28;
    SSPCON2 = 0x00;
    SSPSTAT = 0x00;
    SSPADD = ((_XTAL_FREQ / (4 * freq)) - 1);
    TRISB0 = 1; // SDA
    TRISB1 = 1; // SCL
}
```

---

## 📊 Resultados Esperados

* Señales `SCL` y `SDA` oscilando en el osciloscopio.
* Recepción de ACK por parte del esclavo.
* Transferencia correcta de bytes y datos consistentes.
* Detección de condiciones START y STOP en simulación.

---

## 🧐 Apuntes Extra del Profesor

* “En I2C hay que tener cuidado con las resistencias pull-up, son obligatorias.”
* “Pueden usar múltiples esclavos siempre que tengan diferentes direcciones.”
* “Para simular I2C, usen Proteus y validen con lógica el comportamiento.”

---

## ✅ Conclusiones

I2C es un protocolo sencillo pero poderoso, especialmente últil para conectar múltiples periféricos con solo dos líneas. Su versatilidad, combinada con las capacidades del PIC18F4550, permite el desarrollo de soluciones escalables y eficientes.

El dominio de sus registros, configuraciones y tramas es fundamental para el éxito en sistemas embebidos.

---

📘 **Fin de los apuntes – Comunicación I2C**
