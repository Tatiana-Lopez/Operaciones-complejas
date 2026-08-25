# Operaciones con vectores y matrices complejas

Cuaderno de Jupyter con la implementación y verificación de 18 operaciones de
álgebra lineal sobre los números complejos, usando el tipo `complex` de Python
y la librería `numpy`.

**Autor:** _(tu nombre)_
**Curso:** _(nombre del curso)_
**Institución:** Escuela Colombiana de Ingeniería Julio Garavito

## Contenido

El cuaderno `operaciones_complejas.ipynb` cubre:

| # | Operación |
|---|-----------|
| 1 | Adición de vectores complejos |
| 2 | Inverso aditivo de un vector complejo |
| 3 | Multiplicación de un escalar por un vector complejo |
| 4 | Adición de matrices complejas |
| 5 | Inversa aditiva de una matriz compleja |
| 6 | Multiplicación de un escalar por una matriz compleja |
| 7 | Transpuesta de una matriz/vector |
| 8 | Conjugada de una matriz/vector |
| 9 | Adjunta (daga) de una matriz/vector |
| 10 | Producto de dos matrices |
| 11 | Acción de una matriz sobre un vector |
| 12 | Producto interno de dos vectores |
| 13 | Norma de un vector |
| 14 | Distancia entre dos vectores |
| 15 | Valores y vectores propios de una matriz |
| 16 | Verificación de matriz unitaria |
| 17 | Verificación de matriz hermitiana |
| 18 | Producto tensor (Kronecker) |

Cada operación incluye su definición matemática, una implementación propia
basada en la definición y la verificación contra la función equivalente de
NumPy. Al final hay una batería de pruebas automáticas.

## Requisitos

- Python 3.8 o superior
- numpy
- jupyter

## Ejecución

```bash
pip install numpy jupyter
jupyter notebook operaciones_complejas.ipynb
```
