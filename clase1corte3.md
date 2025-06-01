# Apuntes de Clase - Módulo Captura / Comparación / PWM

**Profesor:** Ing. Jorge Eduardo Cote Ballesteros  
**Curso:** Microcontroladores - Cuarto semestre  
**Año:** 2024  

---

## 🧠 Módulo de Captura

- El **módulo de captura** permite registrar el valor actual de un timer cuando ocurre un evento.
- Es usado principalmente para **eventos externos**, como flancos de subida o bajada.
- Al detectar el evento, se guarda el conteo del timer en el registro de captura.
- Puede generar una **interrupción** que avise la ocurrencia del evento.

### 📥 Captura de eventos internos

- Algunos microcontroladores permiten asociar el módulo de captura a **eventos internos**.
- Ejemplo: en el **Atmega16**, se puede usar el conversor A/D como fuente de captura.

### ⏱️ Granularidad del Timer

- El tiempo entre cambios en la señal debe ser **mayor que la resolución del timer**.
- Si no, se repite la misma marca de tiempo hasta que se detecte un cambio válido.
- Se recomienda usar un **prescaler lo más pequeño posible**.

### 🧮 Tiempos de lectura

- Se debe tener en cuenta el tiempo entre:
  - Ocurrencia del evento.
  - Llamado a la rutina de interrupción.
  - Lectura del segundo evento.

---

## 🧩 Módulo de Comparación

- Este módulo **activa una salida** cuando el contador llega a un valor específico.
- Se compara el conteo del timer con un **registro de comparación**.
- Puede tener una función de **reset automático** al alcanzar el valor.

### ⚙️ Tipos de eventos posibles

- Salida a **nivel alto**, **nivel bajo**.
- Activación en **flanco de subida o bajada**.
- **Interrupción**.
- Todos pueden generar también un **reset del Timer**.

---

## 📶 PWM - Modulación por Ancho de Pulso

### Definición

- Es una señal cuadrada donde el **ancho del pulso** (TON) varía.
- Representa un **valor de tensión analógica equivalente**.
- **Frecuencia constante**, solo varía el **duty cycle** (ciclo útil).

### 🧾 Fórmulas importantes

- Ciclo útil (Duty Cycle):  
  \[
  DC = \frac{T_{ON}}{T_{ON} + T_{OFF}} \times 100 = \frac{T_{ON}}{T} \times 100
  \]

- Valor promedio de tensión:  
  \[
  V_{avg} = \frac{1}{T} \int_{0}^{T} v(t) dt = \frac{A \cdot T_{ON}}{T}
  \]

---

## 🛠️ Módulo PWM en Microcontroladores

- Se basa en los **módulos de timer y comparación**.
- Utiliza dos registros:
  - Uno para definir la **frecuencia (PR2)**.
  - Otro para el **ciclo útil (CCPRxL)**.
- En algunos casos, solo se configura el tiempo en alto (TON) y la frecuencia es fija.

### ⌛ Tipos de contadores

- **Ascendente**.
- **Ascendente - descendente**.
- El periodo depende de la resolución del timer.

---

## 🔧 Aplicación en el PIC18F4550

### 🔹 Captura

- Usa **TMR1 y TMR3** (16 bits).
- Puede capturar en:
  - Todo flanco de subida.
  - Todo flanco de bajada.
  - Cada 4to o 16vo flanco de subida.

### 🔹 Comparación

- Registro comparado constantemente con TMR1/TMR3.
- Eventos:
  - Estado alto, bajo.
  - Cambio de estado (toggle).
  - Mantener estado (latch).

### 🔹 PWM

- Resolución de **10 bits**.
- Utiliza **CCP1** y/o **CCP2**.
- Configuración:
  1. Definir periodo con `PR2`.
  2. Definir ciclo útil con `CCPRxL`.
  3. Configurar `TRISC` para usar pines PWM como salida.
  4. Activar `TMR2` y configurar su prescaler.
  5. Configurar el módulo CCPx en modo PWM.

### 🔍 Ejemplo de configuración

- Se desea frecuencia de 1 KHz:  
  \[
  PR2 = \frac{PWM\_Period}{4 \cdot T_{OSC} \cdot TMR2\_Prescaler} - 1
  \]
  Si \( T_{OSC} = \frac{1}{4MHz} \), prescaler = 1:
  \[
  PR2 = \frac{1}{1kHz \cdot 4 \cdot 0.25 \mu s} - 1 = 999
  \]

- Con prescaler = 16:
  \[
  PR2 = \frac{1}{1kHz \cdot 4 \cdot 4 \mu s} - 1 \approx 61.5
  \]

- Precarga para TON:
  \[
  Valor = \frac{TON}{T_{OSC} \cdot Prescaler}
  \]

---

## 📄 Código de ejemplo

### Configuración básica:

```c
void setupPWM() {
    TRISCbits.TRISC2 = 0;        // RC2 como salida
    PR2 = 249;                   // Periodo para 1kHz con Fosc = 4MHz y prescaler 16
    CCP1CON = 0b00001100;        // Modo PWM
    CCPR1L = 125;                // Ciclo útil ~50%
    T2CON = 0b00000111;          // TMR2 con prescaler 16 y activado
}
