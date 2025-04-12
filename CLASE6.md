# Apuntes de Clase: Temporizadores y Timers en Microcontroladores

**Estudiantes:** Fabian Ortiz y Daniel Cruz  
**Profesor:** Ing. Jorge Eduardo Cote Ballesteros  
**Curso:** Cuarto Semestre - Microcontroladores  

---

## ⏱️ 1. Introducción a los Temporizadores

Los **temporizadores (Timers)** son módulos internos de los microcontroladores que permiten medir el tiempo o contar eventos. Aunque se les llama temporizadores, técnicamente son **contadores**, ya que su función principal es incrementar o decrementar un valor con base en una señal de reloj.

### Características Generales:
- Se encuentran en todos los microcontroladores modernos.
- Suelen tener resoluciones de **8 o 16 bits**, aunque algunos modelos permiten 32 bits.
- Pueden operar en modo temporizador (cuentan el tiempo) o modo contador (cuentan eventos).
- Algunos modelos permiten configurar temporizadores para que trabajen con señales de entrada externas.

> 📌 Son esenciales en tareas como la **medición de tiempo**, generación de **delays**, generación de **señales PWM**, y control de **eventos periódicos**.

---

## ⚙️ 2. Aplicaciones de los Temporizadores

Los timers son fundamentales en una amplia gama de aplicaciones:

- ⏳ **Temporizador:** Medición de tiempo entre eventos.
- 🕰️ **RTC (Reloj en Tiempo Real):** Usando osciladores externos para llevar la cuenta del tiempo.
- 📡 **Medición de frecuencia:** Determinando cuántos eventos ocurren por segundo.
- 🔁 **Control de procesos:** Ejecución de tareas periódicas (como muestreo de sensores).
- 🔔 **Generador de señales:** Generación de pulsos para PWM o señales de reloj.
- 📥 **Contador de eventos:** Medición de cuántas veces se produce un cambio de estado.

> ✅ En sistemas embebidos, los temporizadores permiten ejecutar tareas sin intervención constante del procesador, aumentando la eficiencia.

---

## 🔢 3. Funcionamiento del Módulo Timer

Un temporizador es básicamente un **registro** que se incrementa o decrementa con cada ciclo de reloj. Al alcanzar un valor máximo (overflow) o una coincidencia con un valor preestablecido (match), puede:

- Generar una interrupción.
- Reiniciar su conteo.
- Ejecutar una acción en hardware (como cambiar el estado de un pin).

### Estructura Básica
- **Registro TMRx:** Almacena el valor actual del contador.
- **Prescaler:** Divide la frecuencia de entrada.
- **Postscaler (algunos modelos):** Genera interrupciones tras un número determinado de overflows.

---

## 🔎 4. Resolución y Lectura de Timers

La resolución del timer depende del número de bits:

| Bits | Máximo Valor | Tiempo de Conteo (sin prescaler, 1 MHz) |
|------|---------------|------------------------------------------|
| 8 bits | 255 | 255 µs |
| 16 bits | 65535 | 65.5 ms |

> 🧠 En sistemas de 8 bits como el PIC18F4550, leer un timer de 16 bits puede ser problemático si no se sincroniza bien. Se recomienda leer los registros en el mismo ciclo o usar registros temporales.

---

## 📡 5. Fuentes de Reloj

Los temporizadores pueden usar diferentes fuentes de reloj:

- **Reloj interno:** Derivado del oscilador principal del microcontrolador.
- **Reloj externo:** Señal aplicada directamente a un pin.
- **Oscilador externo de cristal:** Para aplicaciones de alta precisión como RTC.

> 📌 Algunos temporizadores como el Timer1 pueden configurarse para trabajar con señales asíncronas usando cristales externos.

---

## ⚙️ 6. Prescaler y Postcaler

El prescaler permite dividir la frecuencia de entrada del temporizador, y el postcaler (cuando existe) permite determinar cada cuántos eventos se activa una interrupción.

### Ejemplo:
- Reloj = 4 MHz → Tiempo de instrucción = 1 µs
- Prescaler = 8
- Cada incremento del timer: 1 µs × 8 = 8 µs

Esto permite prolongar el tiempo que se puede medir antes de que ocurra un overflow.

> 🎯 El uso de prescalers es clave cuando se necesitan **intervalos largos de tiempo** con timers de baja resolución.

---

## 📈 7. Granularidad del Temporizador

La granularidad es la menor unidad de tiempo que puede medirse con un temporizador:

```math
Granularidad = T_{inst} * Prescaler
```

### Ejemplo con Timer0 de 8 bits:
- Fosc = 4 MHz
- Tiempo de instrucción = 1 µs
- Prescaler = 1 → Granularidad = 1 µs
- Prescaler = 1024 → Granularidad = 1.024 ms

---

## 🧪 8. Timer 0 en PIC18F4550

Timer 0 puede operar en modo temporizador o contador. Es uno de los timers más versátiles.

### Características:
- Resolución: 8 o 16 bits
- Prescaler: 8 bits
- Fuente: Interna o externa (pin RA4)
- Interrupción por desbordamiento

### Registro de configuración:
- TMR0H y TMR0L: valor actual (16 bits)
- INTCON: Bandera de interrupción TMR0IF

> ⚠️ La bandera **TMR0IF** debe limpiarse manualmente en la ISR para evitar múltiples interrupciones.

---

## 🛠️ 9. Ejemplo de Temporización con Interrupciones

**Objetivo:** Encender un LED del puerto D cada 3 segundos.

### Pasos:
1. Configurar Timer0 en modo 16 bits.
2. Calcular el valor de precarga.
3. Cargar TMR0 con ese valor.
4. Activar la interrupción.
5. En la ISR, alternar el estado del LED y recargar TMR0.

### Cálculo de precarga:
```math
T_{conteo} = T_{inst} * Prescaler * (65536 - TMR0)
```

---

## 📋 10. Código de Ejemplo Simplificado (en C - MPLAB XC8)

```c
void __interrupt() ISR() {
  if (TMR0IF) {
    TMR0IF = 0;
    TMR0H = (60536 >> 8); // valor para 3s
    TMR0L = 60536 & 0xFF;
    LATD ^= 0xFF; // Alternar LEDs del Puerto D
  }
}

void main() {
  TRISD = 0x00;
  LATD = 0x00;
  TMR0H = (60536 >> 8);
  TMR0L = 60536 & 0xFF;
  T0CON = 0x80; // Habilitar Timer0, 16-bit
  TMR0IE = 1;
  GIE = 1;
  while(1);
}
```

---

## 📚 11. Timers Avanzados (Timer1, Timer2, Timer3)

- **Timer1:** 16 bits. Puede usar cristal externo para RTC.
- **Timer2:** 8 bits. Se usa comúnmente con PWM.
- **Timer3:** Similar a Timer1, útil en tareas concurrentes.

> En aplicaciones de mayor complejidad, se pueden usar varios timers en paralelo para manejar múltiples tareas de temporización simultáneas.

---

## ✅ 12. Conclusiones y Recomendaciones

✔️ Los timers son herramientas vitales para cualquier proyecto de microcontroladores.  
✔️ Con su correcta configuración, se pueden realizar tareas de alta precisión sin depender constantemente del procesador.  
✔️ Es crucial entender cómo configurar **prescalers**, **registros de conteo**, y **banderas de interrupción**.  
✔️ La selección del timer dependerá del tipo de tarea (PWM, conteo externo, RTC, etc.).

---
