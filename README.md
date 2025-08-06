
# Programación Lineal en Python

Este proyecto presenta la resolución de un problema de **programación lineal** utilizando dos métodos complementarios:  
el **método gráfico**, útil para problemas con dos variables, y el **método simplex**, ampliamente utilizado en aplicaciones industriales y empresariales.  
Ambos métodos son implementados con Python, haciendo uso de herramientas matemáticas y de visualización.

El enfoque incluye no solo la resolución, sino también un **análisis de sensibilidad** que permite explorar cómo se comporta la solución frente a cambios en los parámetros del modelo.

---

## Problema

Una empresa produce dos tipos de productos, `x` y `y`. Ambos requieren el uso de dos recursos limitados (por ejemplo, horas de trabajo y unidades de materia prima). El objetivo es **maximizar las ganancias** totales, considerando las siguientes condiciones:

- Cada unidad de `x` genera 40 unidades monetarias de ganancia.
- Cada unidad de `y` genera 30 unidades monetarias de ganancia.
- Cada unidad de `x` consume 2 unidades del recurso A, y cada unidad de `y` consume 1 unidad del mismo recurso (limitado a 100 unidades).
- Cada unidad de `x` consume 1 unidad del recurso B, y cada unidad de `y` consume 2 unidades del mismo recurso (limitado a 80 unidades).

### Modelo matemático

**Función objetivo (Maximizar):**

```
Z = 40x + 30y
```

**Sujeto a las restricciones:**

```
2x + y ≤ 100
x + 2y ≤ 80
x ≥ 0, y ≥ 0
```

---

## Implementación en Python

```python
import numpy as np
import matplotlib.pyplot as plt
from scipy.optimize import linprog

# Coeficientes de la función objetivo (en negativo porque linprog minimiza)
c = [-40, -30]

# Matriz de restricciones (lado izquierdo)
A = [[2, 1],
     [1, 2]]

# Lado derecho de las restricciones
b = [100, 80]

# Método gráfico
x = np.linspace(0, 100, 200)
y1 = (100 - 2*x)
y2 = (80 - x)/2

plt.figure(figsize=(8,6))
plt.plot(x, y1, label='2x + y ≤ 100')
plt.plot(x, y2, label='x + 2y ≤ 80')
plt.xlim((0, 60))
plt.ylim((0, 60))
plt.xlabel('x')
plt.ylabel('y')

plt.fill_between(x, 0, np.minimum(y1, y2), where=(np.minimum(y1, y2) >= 0), color='gray', alpha=0.3)
plt.title('Método gráfico - Región factible')
plt.legend()
plt.grid(True)
plt.show()

# Método simplex
res = linprog(c, A_ub=A, b_ub=b, method='highs')
print("Solución óptima (Simplex):")
print(f"x = {res.x[0]:.2f}, y = {res.x[1]:.2f}")
print(f"Valor óptimo Z = {-res.fun:.2f}")
```
### Salida en consola tras la ejecución:
<img width="377" height="163" alt="image" src="https://github.com/user-attachments/assets/d38fd458-b51f-4913-bf63-912eedd6b871" />

### Gráfico devuelto tras la ejecución:
<img width="728" height="547" alt="image" src="https://github.com/user-attachments/assets/f8b6dc7b-97dd-48da-b013-7288eca9440b" />


---

## Análisis

### Interpretación del problema

Este problema simula un escenario de planificación de producción, donde se desea **maximizar las ganancias** sujetas a restricciones de disponibilidad de recursos. Se busca determinar cuántas unidades de cada producto fabricar para alcanzar la mayor rentabilidad posible.

### Solución obtenida

- **x ≈ 40**  
- **y ≈ 20**  
- **Z ≈ 2200**

Esto significa que la mejor estrategia para maximizar la ganancia es producir 40 unidades del producto `x` y 20 del producto `y`.

---

### Análisis de sensibilidad

1. **Variación en los coeficientes de la función objetivo**  
   Si los valores 40 y 30 cambian ligeramente, es posible que la solución óptima no cambie mientras se mantenga el mismo vértice de la región factible como el punto de intersección activo.

2. **Variación en los recursos disponibles (restricciones)**  
   Aumentar o reducir los valores del lado derecho (100 y 80) afecta directamente la región factible.  
   Por ejemplo:
   - Si el recurso A pasa de 100 a 110, se amplía la región y puede mejorar la ganancia.
   - Si disminuye, se reduce la región y probablemente también la utilidad máxima.

3. **Solución múltiple o degenerada**  
   En este caso no existe degeneración ni múltiples soluciones, ya que el óptimo se encuentra en un único vértice de la región factible.

---
