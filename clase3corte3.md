# 📚 Apuntes de Clase – Comunicación Serial Síncrona (SPI/I2C)

**Trabajo realizado por:**  
**Fabián Ortiz, Daniel Cruz y Santiago Bautista**  
**Curso:** Microcontroladores – Cuarto Semestre  
**Docente:** Ing. Jorge Eduardo Cote Ballesteros  
**Año:** 2025

---

## 🧭 Introducción General

La comunicación serial síncrona es un método de transmisión de datos en el que los dispositivos comparten un reloj común para sincronizar la transferencia. A diferencia de la comunicación asíncrona, donde no hay sincronización directa entre transmisor y receptor, la síncrona garantiza una transferencia más rápida y precisa mediante el uso de una señal de reloj.

Esta forma de comunicación se utiliza ampliamente en sistemas embebidos, permitiendo que microcontroladores se comuniquen con sensores, memorias, pantallas y otros periféricos digitales. Se analizan los protocolos SPI (Serial Peripheral Interface) e I2C (Inter-Integrated Circuit), haciendo énfasis en el primero.

---

## 🔌 Características Generales de SPI

- Interfaz maestro-esclavo.
- Comunicación full dúplex.
- Velocidades altas de hasta 12 Mbps.
- Requiere líneas físicas dedicadas.
- Más sencilla que I2C en implementación.

---

## 🧠 Apuntes del Profe

- “SPI es ideal cuando necesitas velocidad y precisión.”
- “Cada esclavo requiere su línea SS, eso limita si tienes pocos pines.”
- “La ventaja de SPI es que sabes exactamente cuándo ocurre la transmisión.”

---

## 🔀 Líneas de Comunicación SPI

| Línea | Descripción               |
|-------|---------------------------|
| MOSI  | Master Out, Slave In      |
| MISO  | Master In, Slave Out      |
| SCK   | Clock generado por maestro|
| SS    | Slave Select              |

---

## 📟 Aplicaciones Comunes

- Pantallas OLED y TFT
- EEPROM seriales
- Módulos de sensores (ej: MAX6675)
- Controladores de motores
- Convertidores digital-analógico

---

## ⚙️ Módulo MSSP en PIC18F4550

El MSSP (Master Synchronous Serial Port) es un módulo de hardware incluido en el PIC18F4550 que soporta tanto SPI como I2C.

### Modos:
- SPI Maestro
- SPI Esclavo
- I2C Maestro
- I2C Esclavo

### Registros Relevantes:

| Registro | Función                             |
|----------|-------------------------------------|
| SSPBUF   | Buffer de transmisión/recepción     |
| SSPSTAT  | Estado del módulo SPI               |
| SSPCON1  | Configuración de operación          |
| TRISx    | Configuración de pines              |

---

## 🛠 Código en C – SPI Maestro (PIC18F4550)

```c
#include <xc.h>

#pragma config FOSC = HS
#pragma config WDT = OFF
#pragma config LVP = OFF

#define _XTAL_FREQ 20000000

void SPI_Master_Init() {
    TRISCbits.TRISC7 = 0; // SDO
    TRISBbits.TRISB0 = 1; // SDI
    TRISBbits.TRISB1 = 0; // SCK
    SSPSTAT = 0x40;
    SSPCON1 = 0x20;
}

void SPI_Transmit(char data) {
    SSPBUF = data;
    while (!PIR1bits.SSPIF);
    PIR1bits.SSPIF = 0;
}

void main(void) {
    SPI_Master_Init();
    while (1) {
        SPI_Transmit('A');
        __delay_ms(1000);
    }
}
##🔧 Diagrama de Tiempo SPI
MOSI: ─█────█────█────█────█────█────█────█────
MISO: ────█────█────█────█────█────█────█────█
SCK : ─┐__┐__┐__┐__┐__┐__┐__┐__┐__┐__┐__┐__┐__┐_
        ↑  ↑  ↑                         ↑
     Bit0 Bit1 ...                   Bit7

##⚠️ Problemas Comunes
Problema	|Causa	 |Solución
No transmite nada	SCK no configurado correctamente	Verificar TRISx y SSPCON1
Datos incorrectos	Polaridad mal configurada	Cambiar CKP/CKE en SSPSTAT
Buffer bloqueado	No se leyó SSPBUF	Leer buffer tras cada envío
Esclavo no responde	SS no activado	Activar SS manualmente

📘 Buenas Prácticas
Verificar con osciloscopio la señal de reloj.

Leer el buffer antes de enviar otro dato.

Configurar pines con ADCON1 correctamente.

Iniciar con velocidad baja para pruebas.

📚 Comparación SPI vs I2C
Característica	SPI	I2C
Número de líneas	4	2
Dirección de esclavos	Por pin SS	Dirección de 7/10 bits
Velocidad	Hasta 12 Mbps	100 KHz a 3.4 MHz
Complejidad	Baja	Media
Full dúplex	Sí	No

📚 Bibliografía Recomendada
Microchip AN1061 – Aplicación SPI

Datasheet PIC18F4550

Manual EEPROM 25LC256

Tutorial SPI de Microchip Developer

🧠 Más Apuntes del Profesor
“I2C se usa más en sensores pequeños. SPI, para pantallas y memorias.”

“Algunas memorias requieren enviar dirección antes de enviar/leer datos.”

“No confundir la frecuencia del reloj del micro con la del SCK.”

✅ Conclusión
SPI es una de las herramientas más útiles para desarrollar aplicaciones embebidas. Dominarlo permite integrar dispositivos de forma eficiente y confiable, aprovechando la velocidad de transmisión y la simplicidad del protocolo. El uso del módulo MSSP en el PIC18F4550 facilita la implementación a nivel de hardware.

