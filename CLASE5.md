# Apuntes de Clase: Teclado, Pulsadores y Osciladores en Microcontroladores

**Estudiantes:** Fabian Ortiz y Daniel Cruz  
**Profesor:** Ing. Jorge Eduardo Cote Ballesteros  
**Curso:** Cuarto Semestre - Microcontroladores  

---

## 🧩 1. Introducción al Hardware en Microcontroladores

En cualquier proyecto basado en microcontroladores, el hardware asociado define cómo interactúa el sistema con su entorno. Se debe considerar:

- **Entradas:** Sensores (temperatura, luz, presión), potenciómetros, pulsadores, botones, teclados matriciales.
- **Salidas:** LEDs, displays LCD/OLED, motores, servos, relevadores, y otros actuadores.

> **Comentario:** Es importante conocer bien las características eléctricas de cada componente conectado para evitar errores como cortocircuitos, sobrecorrientes o daños al microcontrolador.

---

## 🔘 2. Pulsadores y Botones

### 🔹 Diferencias Principales

- **Pulsador (push button):** Cambia y mantiene el estado **solo mientras está presionado**. Muy útil para operaciones momentáneas como reiniciar o encender una función temporal.

- **Botón (switch):** Cambia de estado con una sola pulsación y **se mantiene hasta una nueva pulsación**. Usado comúnmente en encendido/apagado.

> Los dipswitch son una forma de botón de múltiples posiciones, común en configuración de hardware en sistemas industriales.

### 🔎 Cálculo de Resistencia

Usamos la Ley de Ohm y potencia:

```math
R1 = \frac{Vcc}{IR1} \quad P = IR1^2 * R1
```

> **Ejemplo práctico:** Si alimentamos un botón con 5V y queremos una corriente máxima de 10mA, se recomienda usar al menos una resistencia de 500 ohmios.

---

## 🌀 3. Efecto Rebote (Bounce Effect)

### ¿Qué es?

El efecto rebote se produce cuando las partes metálicas del pulsador hacen **contacto repetido en milisegundos** antes de estabilizarse. Esto genera múltiples transiciones rápidas entre estado bajo y alto.

### Problemas del Rebote:
- Múltiples activaciones no deseadas.
- Lecturas erróneas.
- Comportamiento errático del sistema.

---

## 🛠️ 4. Soluciones al Rebote

### 📎 Por Hardware (RC)

Se coloca una resistencia y un condensador para filtrar la señal.

```math
T_{carga} = (R1 + R2) * C \quad T_{descarga} = R2 * C
```

> Un tiempo de carga/descarga de 20 a 40 ms suele ser suficiente para la mayoría de los pulsadores mecánicos.

### 🧠 Por Software

Implementación sencilla en pseudocódigo:

```c
if (entrada_pulsador != valor_pulsador) {
    __delay_ms(30); // Se espera más del tiempo de rebote
    valor_pulsador = entrada_pulsador;
}
```

> **Comentario:** Esta solución es muy práctica en microcontroladores con recursos limitados.

---

## ⌨️ 5. Teclado Matricial

Los **teclados matriciales** permiten múltiples entradas con pocos pines del microcontrolador.

### 📐 Ejemplo
- Teclado 4x3 (12 teclas): 4 filas + 3 columnas = **7 pines**.
- Para un teclado cuadrado con n² teclas se necesitan 2n pines.

### 🧭 Funcionamiento:
- Se "mueve" un 0 lógico por las columnas.
- Cuando una fila detecta el estado bajo, se identifica la tecla.
- Puede implementarse con **interrupciones** o verificación cíclica (polling).

### 🧪 Interrupciones con PIC 18F4550
- Los pines RB4 a RB7 del puerto B permiten **detección por cambio de estado**.
- Estos pines también incluyen **resistencias pull-up internas**.

> **Nota:** Esto mejora la eficiencia del sistema, ya que solo se responde cuando hay una tecla presionada, ahorrando procesamiento.

---

## 🔁 6. Implementación Básica (Pseudocódigo)

```c
activar_pullups();
activar_interrupciones_estado();
activar_interrupciones_globales();

while (1) {
  mover_0_logico_columnas();
  if (interrupcion) {
    identificar_tecla();
    codificar_tecla();
  }
}
```

> **Recomendación:** Usar un contador para evitar errores por rebotes o interferencias.

---

## 💡 7. LEDs (Light Emitting Diode)

- Un LED es un **diodo que emite luz** al polarizarse en directo.
- Solo conduce corriente si el voltaje de avance es superado (~1.8V a 3.3V dependiendo del color).

### 🔍 Cálculo de resistencia limitadora

```math
Vcc - Vf = I * R \Rightarrow R = \frac{Vcc - Vf}{I}
```

> Ejemplo con LED rojo:
```math
R = \frac{5V - 2V}{0.02A} = 150\Omega
```

> **Precaución:** El no uso de resistencia puede quemar el LED o dañar el pin del microcontrolador.

---

## ⏱️ 8. Osciladores

### 📡 Tipos de Osciladores en PIC 18F4550

| Tipo | Descripción |
|------|-------------|
| **HS / XT** | Requiere cristal externo en OSC1/OSC2. |
| **EC / ECIO / ECPLL** | Señal de reloj externa. |
| **INTOSC** | Oscilador interno de 8MHz. |
| **INTRC** | RC interno de 31KHz. |
| **PLL** | Multiplica frecuencia hasta 96MHz. |

> **Comentario:** El modo de oscilador debe configurarse en los registros de configuración al momento de programar el PIC.

### 🔧 Registros de Configuración
- **CONFIG1L / CONFIG1H**: Selección de oscilador.
- **OSCON / OSTUNE**: Control y ajuste de frecuencia.

---

## 📊 9. Diagrama de Relojes y Frecuencia

- **Oscilador principal:** Usado por la CPU.
- **Oscilador para USB:** Necesita una frecuencia precisa (48MHz).
- **PLL:** Permite adaptar el oscilador base a la frecuencia requerida por periféricos como UART, SPI, USB.

> Para usar USB, se requiere frecuencia de 48 MHz, que puede lograrse desde 4 MHz con el PLL.

---

## ✅ 10. Conclusión

El manejo correcto de **entradas mecánicas** como botones y teclados, así como la configuración precisa del **oscilador**, son aspectos esenciales en el diseño de sistemas embebidos. Evitar errores de rebote, calcular correctamente los componentes asociados y usar las interrupciones correctamente optimiza el rendimiento del sistema.
