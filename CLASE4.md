# Apuntes de Clase: Interrupciones en Microcontroladores

**Estudiantes:** Fabian Ortiz y Daniel Cruz  
**Profesor:** Ing. Jorge Eduardo Cote Ballesteros  
**Curso:** Cuarto Semestre - Microcontroladores  

---

##  1. Introducción a las Interrupciones

En un sistema embebido, un **microcontrolador** debe realizar múltiples tareas simultáneamente. Una interrupción es un mecanismo que permite **detectar eventos y responder sin afectar la ejecución del programa principal**.

###  Ejemplo de Aplicación
Un sistema de **banda transportadora** requiere:
- Detectar cajas en el inicio y activar el motor.
- Verificar el final de la banda para accionar un robot.
- Registrar la entrada de datos desde un teclado.
- Mostrar información de sensores en una pantalla.
- Comunicarse con una base de datos.

Si el microcontrolador verifica continuamente cada evento, el sistema se vuelve ineficiente. **Las interrupciones resuelven este problema permitiendo que el microcontrolador reaccione solo cuando sea necesario**.

---

##  2. ¿Cómo Funcionan las Interrupciones?

Cuando un evento externo o interno activa una **interrupción**, el microcontrolador:

1. **Pausa la ejecución del programa principal**.
2. **Guarda el estado actual** de la ejecución.
3. **Ejecuta la rutina de servicio de interrupción (ISR, Interrupt Service Routine)**.
4. **Vuelve a la ejecución del programa principal**.

###  Beneficios
 Ahorra recursos del sistema.  
 Permite manejar eventos en **tiempo real**.  
 Reduce la carga de procesamiento en el programa principal.  

---

##  3. Componentes Claves de una Interrupción

###  **Bits de Control**
| Bit | Función |
|-----|---------|
| **IE (Interrupt Enable)** | Habilita interrupciones en el microcontrolador. |
| **IF (Interrupt Flag)** | Indica si ocurrió una interrupción. |

Cuando ocurre una interrupción, el **bit IF se activa (1)** y el sistema ejecuta la ISR. Dependiendo del microcontrolador, el **bit IF puede resetearse automáticamente o manualmente por el programador**.

###  **Tipos de Eventos que Activan una Interrupción**
1. **Externos:** Cambio en un pin de entrada digital (ejemplo: presionar un botón).
2. **Internos:** Finalización de una conversión ADC, temporizadores, comunicación serial, etc.

---

##  4. Tipos de Interrupciones en Microcontroladores

###  **Interrupciones Externas**
- Se generan cuando hay un **cambio de estado** en una señal externa.
- Detectan **flancos de subida o bajada** en un pin de entrada digital.
- Pueden verse afectadas por **ruido externo**, por lo que se recomienda usar **filtros o debounce**.

###  **Interrupciones Internas**
- Se generan dentro del microcontrolador.
- Ejemplos:
  - **Temporizadores (Timer)**: Activan una interrupción en un período definido.
  - **Conversión ADC**: Indica que la conversión de analógico a digital ha finalizado.
  - **Comunicación Serial**: Indica que un dato ha sido recibido o enviado.
  
---

## 5. Implementación de Interrupciones en Microcontroladores

###  **¿Cuándo usar interrupciones y cuándo verificar puertos?**

| Usar interrupciones cuando… | Usar verificación de puertos cuando… |
|----------------------------|---------------------------|
| Eventos ocurren de forma **aleatoria**. | Eventos son **periódicos** y predecibles. |
| Necesitas **respuesta rápida** al evento. | No hay problemas si el evento se detecta con retraso. |
| Se necesita **ahorrar procesamiento**. | Hay **pocas tareas** en el programa principal. |
| Eventos tienen **pulsos muy cortos**. | Los eventos tienen **pulsos largos**. |
| El evento es generado por **hardware externo**. | Se requiere interacción manual (botones, teclado). |

---

##  6. Vectores de Interrupción

Cada interrupción tiene asociado un **vector de interrupción**, que es la dirección en la memoria donde se encuentra la ISR.

- **En algunos microcontroladores (ejemplo: PIC)**, el programador asigna manualmente la ISR.
- **En otros (ejemplo: ATmega16)**, el hardware del microcontrolador se encarga de llamar automáticamente la ISR correspondiente.

###  **Prioridad de Interrupciones**
Si varias interrupciones ocurren al mismo tiempo, la prioridad **determina cuál se ejecuta primero**.
- Algunos microcontroladores permiten **asignar prioridades manualmente (ejemplo: PIC)**.
- En otros, la prioridad está determinada por la **tabla de vectores de interrupción**.

---

##  7. Configuración de Interrupciones en PIC18F4550

###  **Interrupciones Externas**
- **INTx**: Interrupciones en pines específicos (ejemplo: flancos de subida o bajada en RB0, RB1, RB2).
- **PORTB Interrupt-on-Change**: Se activa cuando cambia el estado de ciertos pines del puerto B.

###  **Interrupciones Internas**
- **Temporizadores (Timer 0, 1, 2, etc.)**.
- **Módulo ADC** (finalización de conversión analógica-digital).
- **Módulo PWM**.
- **Módulo de Comunicación Serial (UART, SPI, I2C)**.

---

##  8. Ejemplo de Código en MPLAB (PIC18F4550)

### Configuración de una Interrupción Externa en RB0
```c
void __interrupt() ISR(void) {
    if (INT0IF) { // Verifica si la interrupción proviene de RB0
        INT0IF = 0; // Limpia la bandera de interrupción
        PORTD++; // Incrementa el valor en el puerto D
    }
}

void main() {
    TRISB0 = 1;  // Configura RB0 como entrada
    INT0IE = 1;  // Habilita interrupción en RB0
    GIE = 1;     // Habilita interrupciones globales
    while(1);    // Loop infinito
}
```

---

##  9. Conclusión

Las **interrupciones** permiten a un microcontrolador responder a eventos externos o internos de manera eficiente, sin desperdiciar tiempo de procesamiento en verificaciones constantes. Su uso adecuado optimiza el rendimiento y garantiza el correcto funcionamiento de sistemas embebidos en tiempo real.
