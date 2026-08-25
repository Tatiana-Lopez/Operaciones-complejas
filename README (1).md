# Operaciones con vectores y matrices complejas

Implementación y verificación de 18 operaciones fundamentales del álgebra
lineal sobre el cuerpo de los números complejos, usando el tipo nativo
`complex` de Python y la librería `numpy`.

Cada operación se implementa **dos veces**: una versión propia construida
directamente desde la definición matemática (con bucles explícitos, para hacer
visible el procedimiento) y la llamada equivalente de NumPy. Las dos se
comparan en cada celda, y al final las pruebas automáticas.

---

## Estructura del repositorio

```
.
-operaciones-complejos.ipynb   Cuaderno principal:  implementación y pruebas
-README.md                     
```

---

## Modelo de datos

### Escalares: el tipo `complex`

Un número complejo $z = a + bi$ se representa con el tipo nativo `complex`.
La unidad imaginaria se escribe `j` y debe ir pegada a un número.

```python
z = 3 + 4j            # literal
z = complex(3, 4)     # constructor
```

**Forma cartesiana.** Es la representación interna: el par $(a, b)$ de parte
real y parte imaginaria, accesibles como atributos.

| Concepto | Notación | Python |
|----------|----------|--------|
| Parte real | $a = \mathrm{Re}(z)$ | `z.real` |
| Parte imaginaria | $b = \mathrm{Im}(z)$ | `z.imag` |
| Conjugado | $\bar{z} = a - bi$ | `z.conjugate()` |

**Forma polar.** El mismo número visto como par $(r, \theta)$ — módulo y
argumento — con $r = \sqrt{a^2+b^2}$ y $\theta = \arctan(b/a)$, de modo que
$z = r(\cos\theta + i\sin\theta) = re^{i\theta}$. Se obtiene con el módulo
`cmath` de la biblioteca estándar.

| Concepto | Notación | Python |
|----------|----------|--------|
| Módulo | $r = \lvert z \rvert$ | `abs(z)` |
| Argumento (radianes) | $\theta = \arg(z)$ | `cmath.phase(z)` |
| Cartesiana → polar | $(a,b) \mapsto (r,\theta)$ | `cmath.polar(z)` |
| Polar → cartesiana | $(r,\theta) \mapsto (a,b)$ | `cmath.rect(r, theta)` |

```python
>>> import cmath
>>> z = 3 + 4j
>>> z.real, z.imag
(3.0, 4.0)
>>> cmath.polar(z)
(5.0, 0.9272952180016122)          # r = 5, theta ≈ 0.927 rad ≈ 53.13°
>>> cmath.rect(5.0, 0.9272952180016122)
(3.0000000000000004+3.9999999999999996j)
```

La conversión de ida y vuelta no devuelve exactamente el número original: esa
pequeña deriva es error de punto flotante, y es la razón por la que en todo el
proyecto se compara con tolerancia y nunca con `==`.

### Vectores y matrices: `numpy.ndarray`

| Objeto | Representación | Forma (`shape`) |
|--------|----------------|-----------------|
| Vector de $\mathbb{C}^n$ | arreglo de una dimensión | `(n,)` |
| Matriz $m \times n$ | arreglo de dos dimensiones | `(m, n)` |
| Vector columna explícito | arreglo de dos dimensiones | `(n, 1)` |

```python
import numpy as np

v = np.array([1 + 2j, 3 - 1j], dtype=complex)          # shape (2,)
A = np.array([[1 + 1j, 2 - 1j],
              [0 + 3j, 4 + 0j]], dtype=complex)        # shape (2, 2)
```

### Convenciones del proyecto

1. **`dtype=complex` siempre.** Si se omite, NumPy infiere el tipo de los datos
   y descarta la parte imaginaria en silencio. Todas las funciones convierten
   su entrada con `np.array(x, dtype=complex)` antes de operar.

2. **Los vectores 1-D se interpretan como vectores columna.** NumPy no
   distingue fila de columna en arreglos de una dimensión: `v.T` no hace nada.
   Donde la distinción importa (transpuesta, producto tensor), las funciones
   hacen `reshape(-1, 1)` internamente.

3. **El producto interno conjuga el primer argumento**, siguiendo la convención
   de la física: $\langle u, v \rangle = u^{\dagger}v = \sum_k \overline{u_k}v_k$.
   Es lineal en el segundo argumento y antilineal en el primero.

4. **Comparación con tolerancia.** Las igualdades se verifican con
   `np.allclose(..., atol=1e-9)` a través del auxiliar `son_iguales`, nunca con
   `==`.

---

## Funciones disponibles

| # | Función | Descripción | Equivalente en NumPy |
|---|---------|-------------|----------------------|
| 1 | `suma_vectores(u, v)` | Suma componente a componente | `u + v` |
| 2 | `inverso_aditivo_vector(v)` | Vector opuesto $-v$ | `-v` |
| 3 | `escalar_por_vector(c, v)` | Escalar complejo por vector | `c * v` |
| 4 | `suma_matrices(A, B)` | Suma entrada a entrada | `A + B` |
| 5 | `inverso_aditivo_matriz(A)` | Matriz opuesta $-A$ | `-A` |
| 6 | `escalar_por_matriz(c, A)` | Escalar complejo por matriz | `c * A` |
| 7 | `transpuesta(M)` | Intercambia filas y columnas | `M.T` |
| 8 | `conjugada(M)` | Conjuga cada entrada | `M.conj()` |
| 9 | `adjunta(M)` | Transpuesta conjugada, $M^{\dagger}$ | `M.conj().T` |
| 10 | `producto_matrices(A, B)` | Producto matricial | `A @ B` |
| 11 | `accion(A, v)` | Aplica la matriz al vector | `A @ v` |
| 12 | `producto_interno(u, v)` | $\langle u,v\rangle$ conjugando $u$ | `np.vdot(u, v)` |
| 13 | `norma(v)` | Norma euclidiana $\lVert v \rVert$ | `np.linalg.norm(v)` |
| 14 | `distancia(u, v)` | $\lVert u - v \rVert$ | `np.linalg.norm(u - v)` |
| 15 | `valores_y_vectores_propios(A)` | Espectro de la matriz | `np.linalg.eig(A)` |
| 16 | `es_unitaria(U)` | Verifica $U^{\dagger}U = UU^{\dagger} = I$ | — |
| 17 | `es_hermitiana(A)` | Verifica $A^{\dagger} = A$ | — |
| 18 | `producto_tensor(A, B)` | Producto de Kronecker | `np.kron(A, B)` |
| — | `son_iguales(X, Y, tol)` | Comparación con tolerancia | `np.allclose` |

### Ejemplos

**Operaciones sobre vectores**

```python
u = np.array([1 + 2j, 3 - 1j], dtype=complex)
v = np.array([2 - 1j, 0 + 4j], dtype=complex)

suma_vectores(u, v)              # [3.+1.j  3.+3.j]
escalar_por_vector(2 + 3j, u)    # [-4. +7.j   9. +7.j]
producto_interno(u, v)           # (-4+7j)
norma(u)                         # 3.872983346207417
distancia(u, v)                  # 6.6332495807108
```

Se puede notar la diferencia entre el producto interno y el producto punto sin
conjugar, un error frecuente:

```python
np.vdot(u, v)    # (-4+7j)   correcto: conjuga u
np.dot(u, v)     # (8+15j)   NO es el producto interno complejo
```

**Operaciones sobre matrices**

```python
A = np.array([[1 + 1j, 2 - 1j],
              [0 + 3j, 4 + 0j]], dtype=complex)

adjunta(A)
# [[1.-1.j  0.-3.j]
#  [2.+1.j  4.-0.j]]

accion(A, u)                     # [4.-2.j  6.-1.j]
```


**Matrices unitarias y producto tensor**

```python
H = (1 / np.sqrt(2)) * np.array([[1, 1],
                                 [1, -1]], dtype=complex)

es_unitaria(H)                   # True
es_unitaria(np.kron(H, H))       # True: el tensor de unitarias es unitario

ket0 = np.array([1, 0], dtype=complex)
ket1 = np.array([0, 1], dtype=complex)
producto_tensor(ket0, ket1)      # [0.+0.j  1.+0.j  0.+0.j  0.+0.j]
```

```

## Ejecución de las pruebas

La última sección del cuaderno define `correr_pruebas()`, que contrasta cada
implementación propia contra su equivalente de NumPy y comprueba los casos
positivos y negativos de las verificaciones de matriz unitaria y hermitiana.

Basta con ejecutar esa celda:

```python
correr_pruebas()
```

Salida esperada:

```
Ejecutando pruebas...

  OK    01 suma de vectores
  OK    02 inverso aditivo vector
  ...
  OK    18 producto tensor vectores

Todas las pruebas pasaron correctamente.
```

Si alguna verificación falla, la línea correspondiente se marca con `FALLA` y
al terminar el recorrido se lanza un `AssertionError` con la lista de pruebas
fallidas.
