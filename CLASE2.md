# Apuntes de Clase: Entradas y Salidas Digitales (Digital I/O)

**Estudiantes:** Fabian Ortiz y Daniel Cruz  
**Profesor:** Ing. Jorge Eduardo Cote Ballesteros  
**Curso:** Cuarto Semestre - Microcontroladores  

---

## 1. Señales Digitales vs Analógicas

- **Señal Digital:** Tiene solo dos valores posibles (0 o 1). Su forma de onda es cuadrada.  
- **Señal Analógica:** Es continua y puede tomar cualquier valor en el tiempo.  

Las señales digitales permiten una mayor precisión en los sistemas electrónicos y son esenciales en los microcontroladores.  

---

##  2. Puertos de Entrada y Salida (I/O)

Los **puertos de entrada y salida (I/O)** permiten la comunicación del microcontrolador con el entorno. Sus características principales son:

- Son **bidireccionales** (pueden recibir y enviar datos).  
- Se usan para **monitorear y controlar hardware externo**.  
- Se conectan mediante **pines físicos** que transmiten señales eléctricas.  
- Pueden ser de tipo **digital o analógico**.  

###  ¿Cómo funcionan los I/O Digitales?

- Los microcontroladores interpretan señales como **"1" (nivel alto)** o **"0" (nivel bajo)**.  
- Un multímetro puede mostrar un voltaje continuo, pero el microcontrolador digitaliza la señal.  

---

##  3. Puertos Digitales I/O

- Un **puerto digital** generalmente tiene **8 pines de conexión** (aunque puede variar).  
- Puede configurarse como **entrada, salida o bidireccional**.  
- Microcontroladores comerciales tienen entre **8 y 32 pines de I/O digitales**, aunque hay modelos con más de 90 pines.  
- Los pines pueden ser utilizados por otros módulos del microcontrolador.  
- Funcionan generalmente con **lógica directa** (0V = 0, 5V = 1).  

###  Componentes Típicos de los Puertos Digitales

| Registro | Función |
|----------|---------|
| **DDR (Data Direction Register)** | Configura el puerto como entrada o salida. |
| **PORT (Port Register)** | Almacena los valores de salida del puerto. |
| **PIN (Port Input Register)** | Almacena el estado actual de los pines (solo lectura). |

Ejemplo: En los **microcontroladores PIC**, estos registros pueden modificarse bit a bit o byte completo.  

---

##  4. Entradas Digitales

- Se usan para **leer niveles de tensión** desde hardware externo.  
- Dependiendo del microcontrolador, los niveles de tensión son distintos. Ejemplo en **ATmega16**:
  - **VCC = [4.5V, 5.5V]**
  - **Low = [-0.5V, 0.2VCC]**
  - **High = [0.6VCC, VCC+0.5V]**
  - **Undefined = [0.2VCC, 0.6VCC]**

###  Muestreo Digital
- Si el puerto usa **latch o flip-flops**, los cambios solo ocurren en el **flanco de reloj**.  
- Puede haber un **retraso de un ciclo de reloj** entre la lectura y el cambio de estado.  

###  Rangos de Tensión Indefinidos
- Pueden llevar a:
  - **Nivel alto (1).**
  - **Nivel bajo (0).**
  - **Oscilación inestable.**
  - **Nivel indeterminado.**
- Se usa un **Schmitt Trigger** para evitar señales inestables.  

###  Cancelación de Ruido
- Señales ruidosas pueden causar cambios no deseados en los estados del microcontrolador.  
- Algunos microcontroladores verifican varias muestras consecutivas antes de cambiar el estado.  

###  Resistencias Pull-Up y Pull-Down
- Se usan para definir un **estado estable** cuando el pin no está conectado.  
- Se habilitan mediante registros internos del microcontrolador.  

---

##  5. Salidas Digitales

- Manejan ciertos rangos de tensión dependiendo del microcontrolador.
- Ejemplo en **ATmega16**:
  - Máxima tensión en estado “low” = **0.7V**.
  - Mínima tensión en estado “high” = **4.2V**.
  - Rango de corriente en salida: **4-20mA**.
- Se recomienda usar **protecciones para evitar daños en los puertos**.  

###  Características de las Salidas Digitales
- Si el registro **DDR** está en salida, el pin se conecta al **PORT**.  
- No tienen protecciones internas → **es necesario añadir resistencias o diodos externos**.  
- Es importante conocer los valores **por defecto** de los pines tras un reinicio.  

---

##  6. Programación en MPLAB (Microcontroladores PIC)

###  Configuración en MPLAB
1. Abrir **MPLAB** y seleccionar **File > New Project**.
2. Elegir **Microchip Embedded > Standalone Project**.
3. Seleccionar la **familia y modelo** del microcontrolador.
4. Elegir el **compilador XC8**.
5. Asignar **nombre y ubicación** del proyecto.
6. Crear y agregar archivos de código.  

###  Configuración de Bits y Puertos
- Se configuran desde **Window > Target Memory Views > Configuration Bits**.
- Se puede generar automáticamente el código de configuración.  

Ejemplo de **compuerta XOR en PIC**:
```c
PORTD1 = RB7 ^ RB6;  // XOR entre RB7 y RB6, resultado en PORTD1
```

---

##  7. Microcontrolador PIC 18F4550 y Puertos Digitales

###  Puertos Digitales en PIC 18F4550
- Tiene varios **puertos digitales** con **funcionalidades alternativas**.
- Los puertos cuentan con **tres registros principales**:

| Registro | Función |
|----------|---------|
| **TRIS (DDR - Data Direction Register)** | Define si el pin es entrada (1) o salida (0). |
| **PORT (Port Register)** | Permite leer el nivel de voltaje del pin. |
| **LAT (Latch de salida)** | Guarda el último valor escrito en el puerto. |

###  Diferencia entre **LAT y PORT**
| Registro | Uso |
|----------|---------|
| **PORT** | Se usa para **leer** el nivel eléctrico del pin. |
| **LAT** | Se usa para **escribir** y mantener un estado lógico estable. |

---

##  8. Conclusión

Los puertos de **entrada y salida digitales** son esenciales en los microcontroladores, permitiendo la interacción con el mundo exterior. Su correcta configuración es clave para el funcionamiento estable y eficiente de cualquier sistema embebido.  
