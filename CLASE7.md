# Apuntes de Clase: Error de Estado Estacionario

**Estudiantes:** Fabian Ortiz y Daniel Cruz  
**Profesor:** Ing. Jorge Eduardo Cote Ballesteros  
**Curso:** Séptimo Semestre - Control  

---

## 🧠 1. Introducción al Error de Estado Estacionario

El **error de estado estacionario** (steady-state error) es una medida fundamental en el análisis de sistemas de control. Representa la diferencia entre la entrada deseada y la salida del sistema cuando éste ha alcanzado el equilibrio, es decir, cuando el tiempo tiende a infinito (`t → ∞`).

Este error surge debido a las limitaciones del sistema para seguir ciertas entradas, y se convierte en una herramienta clave para evaluar el **desempeño del sistema** bajo diferentes condiciones de entrada.

> 💡 Un sistema ideal tendría un error estacionario de cero, pero esto no siempre es posible o eficiente. Por eso es importante saber cómo estimarlo y controlarlo.

---

## 🔍 2. Función de Error del Sistema

Considerando un sistema de control en lazo cerrado, la función de error en el dominio de Laplace se define como:

```math
E(s) = R(s) - Y(s) = \frac{R(s)}{1 + G(s)}
```

Donde:
- `E(s)` es la transformada de Laplace del error.
- `R(s)` es la entrada al sistema (referencia).
- `Y(s)` es la salida del sistema.
- `G(s)` es la función de transferencia en lazo abierto.

> 📌 Este resultado se obtiene considerando que el sistema está en lazo cerrado sin controlador. Si hay un controlador, `G(s)` incluirá dicho controlador.

---

## 📏 3. Teorema del Valor Final

Este teorema permite calcular el valor final (cuando `t → ∞`) de una función temporal `f(t)` utilizando su transformada de Laplace `F(s)`:

```math
\lim_{t \to \infty} f(t) = \lim_{s \to 0} sF(s)
```

Aplicado al error:

```math
E_{ss} = \lim_{s \to 0} sE(s)
```

> ⚠️ Este teorema solo aplica si todos los polos de `sE(s)` están en el semiplano izquierdo (i.e., el sistema es estable).

---

## 🧩 4. Tipos de Entradas y Cálculo del Error Estacionario

Los sistemas de control deben responder correctamente a distintos tipos de entradas. Las más comunes son:

### 📈 Entrada tipo Escalón (`R(s) = 1/s`)

El sistema intenta alcanzar un valor constante.

```math
E_{ss} = \frac{1}{1 + G(0)}
```

✅ Error de Posición `Essp`

### 📉 Entrada tipo Rampa (`R(s) = 1/s^2`)

Se espera que la salida siga un incremento constante en el tiempo.

```math
E_{ss} = \lim_{s \to 0} \frac{1}{sG(s)}
```

✅ Error de Velocidad `Essv`

### 📐 Entrada tipo Parábola (`R(s) = 1/s^3`)

El sistema debe seguir una aceleración constante.

```math
E_{ss} = \lim_{s \to 0} \frac{1}{s^2 G(s)}
```

✅ Error de Aceleración `Essa`

> 🎯 A medida que aumenta el orden de la entrada, se requiere un sistema más complejo (mayor número de polos en el origen) para evitar el error.

---

## 🧪 5. Ejemplo de Cálculo

Supongamos:

```math
G(s) = \frac{s + 4}{(s + 2)(s + 1)}
```

### ➕ Evaluación en `s = 0`:

```math
G(0) = \frac{4}{2 * 1} = 2
```

### 🔍 Error de posición:

```math
Essp = \frac{1}{1 + 2} = \frac{1}{3} \approx 0.333
```

### 🔍 Error de velocidad:

```math
Essv = \lim_{s \to 0} \frac{1}{sG(s)} \to \infty
```

### 🔍 Error de aceleración:

```math
Essa = \lim_{s \to 0} \frac{1}{s^2 G(s)} \to \infty
```

> ⚠️ Conclusión: este sistema (Tipo 0) responde bien a un escalón, pero no a rampas ni parábolas.

---

## 🔢 6. Clasificación de Sistemas según el Tipo

Los sistemas se clasifican según el número de **integradores** (polos en el origen) que tiene su función de transferencia `G(s)`.

| Tipo de Sistema | Polos en el origen | Essp | Essv | Essa |
|-----------------|---------------------|------|------|------|
| Tipo 0 | 0 | k | ∞ | ∞ |
| Tipo 1 | 1 | 0 | k | ∞ |
| Tipo 2 | 2 | 0 | 0 | k |

> 📘 Esta clasificación permite predecir cómo responderá el sistema sin necesidad de simularlo.

---

## ⚙️ 7. Control Proporcional para Reducir el Error

Un controlador proporcional de ganancia `Kp` modifica la función de transferencia total:

```math
G_{total}(s) = \frac{K_p G(s)}{1 + K_p G(s)}
```

Entonces:

```math
Essp = \frac{1}{1 + K_p G(0)}
```

> 📉 A mayor `Kp`, menor error. Sin embargo, **no puede eliminar completamente el error** en sistemas Tipo 0.

---

## 🧪 8. Ejemplo con Kp

### Dado:

```math
G(s) = \frac{8}{s^2 + 0.5s + 1}
```

Evaluamos `G(0)`:

```math
G(0) = \frac{8}{1} = 8
```

### Error de Posición:

- `Kp = 7` → `Essp = 1 / (1 + 56) ≈ 0.0178`
- `Kp = 4` → `Essp = 1 / 33 ≈ 0.0303`

> ⚠️ Aunque el error disminuye, un `Kp` demasiado alto puede causar inestabilidad o sobreoscilación.

---

## 📊 9. Cálculo de Ganancia para un Margen de Error Deseado

Si se quiere `Essp ≤ 10%`, entonces:

```math
\frac{1}{1 + K_p G(0)} \leq 0.1 \Rightarrow K_p \geq \frac{9}{G(0)}
```

### Ejemplo con `G(0) = 8`:

```math
K_p \geq \frac{9}{8} = 1.125
```

> ✅ Este análisis ayuda a seleccionar `Kp` sin necesidad de simular el sistema repetidamente.

---

## 🔁 10. Simulaciones y Resultados

### Consideraciones al simular:
- Incluir perturbaciones.
- Medir `Essp`, `Essv` y tiempo de establecimiento.
- Observar estabilidad.

> 🔬 Herramientas como MATLAB, Scilab o Python (con control library) permiten verificar fácilmente el error estacionario.

### Resultados simulados:
| `Kp` | `Essp` | ¿Cumple < 10%? |
|------|--------|-----------------|
| 1.1  | ~0.102 | ❌ No cumple |
| 1.5  | ~0.077 | ✅ Sí cumple |
| 5    | ~0.024 | ✅ Sí cumple |

> 📈 Aumentar `Kp` mejora la precisión, pero no siempre es conveniente desde el punto de vista dinámico.

---

## 🧮 11. Comentarios Finales

- El error de estado estacionario proporciona una medida clara del **desempeño en régimen permanente**.
- El tipo del sistema (0, 1, 2) indica el tipo de entrada que puede seguir sin error.
- El control proporcional mejora el error, pero **no lo elimina**. Para eso, se requieren controladores integrales (PI o PID).
- Las simulaciones ayudan a confirmar los resultados analíticos y observar el comportamiento ante cambios de parámetros.

> 📌 Siempre verificar estabilidad antes de subir `Kp`, especialmente en sistemas con polos complejos o resonantes.

---

## ✅ 12. Conclusiones

✔️ El error estacionario se evalúa fácilmente con el teorema del valor final.  
✔️ Depende del tipo de entrada y del número de integradores en el sistema.  
✔️ El control proporcional es útil pero limitado. Para eliminar error con rampas o parábolas, se requiere control integral.  
✔️ Las herramientas de simulación complementan el análisis teórico y permiten validar diseños.  
