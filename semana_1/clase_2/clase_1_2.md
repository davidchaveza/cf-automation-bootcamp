
# Clase 2 - Primeros pasos con Python aplicado a pruebas

En esta clase aprenderás los fundamentos de Python que aplicaremos en la automatización de pruebas, además de cómo usar PyTest para crear y ejecutar tu primer test.

---

## 🐍 1. Repaso de sintaxis básica de Python (orientado a testing)

### Variables

```python
nombre = "QA"
edad = 30
activo = True
```

### Tipos de datos

```python
numero = 10              # int
decimal = 3.14           # float
texto = "Hola mundo"     # str
booleano = True          # bool
lista = [1, 2, 3]        # list
diccionario = {"a": 1}   # dict
```

### Estructuras de control

```python
# Condicionales
if edad > 18:
    print("Mayor de edad")
else:
    print("Menor de edad")

# Bucles
for i in range(5):
    print(i)

# Funciones
def suma(a, b):
    return a + b
```

---

## 🧱 2. Fundamentos de programación orientada a objetos

```python
# Definición de una clase sencilla

class Calculadora:
    def __init__(self):
        self.resultado = 0

    def sumar(self, a, b):
        self.resultado = a + b
        return self.resultado

    def reiniciar(self):
        self.resultado = 0
```

### Uso de la clase:

```python
calc = Calculadora()
print(calc.sumar(5, 3))  # Resultado: 8
calc.reiniciar()
```

---

## 🧪 3. Introducción a PyTest

### Instalación de PyTest

Desde la terminal de PyCharm:

```bash
pip install pytest
```

### Reglas de PyTest

- Los archivos deben empezar con `test_` o terminar con `_test.py`.
- Las funciones de prueba deben empezar con `test_`.

### Estructura de un archivo de prueba

```python
# tests/test_calculadora.py

from calculadora import Calculadora

def test_suma_correcta():
    calc = Calculadora()
    resultado = calc.sumar(2, 3)
    assert resultado == 5
```

---

## ▶️ 4. Ejecutar pruebas con PyTest

Desde terminal:

```bash
pytest tests/test_calculadora.py
```

### Resultado en consola:

```bash
================== test session starts ==================
collected 1 item

tests/test_calculadora.py .                          [100%]

=================== 1 passed in 0.01s ===================
```

---

## ✅ Buenas prácticas

- Nombrar bien funciones de prueba: `test_suma_correcta`, `test_error_division`, etc.
- Usar `assert` para verificar resultados.
- Organizar código fuente y pruebas en carpetas separadas (`src/` y `tests/`).

---

¡Ahora ya puedes crear tus primeras pruebas automatizadas en Python!
