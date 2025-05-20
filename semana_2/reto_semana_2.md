
# 🚀 Reto Semana 2 - Diseño de suite de pruebas con PyTest

## 🎯 Objetivo
Diseñar y desarrollar un conjunto de pruebas automatizadas para una mini biblioteca de funciones, aplicando **parametrización**, **fixtures** y buenas prácticas.

---

## 📘 Instrucciones

### 1. Crea un archivo `src/cadena.py` con las siguientes funciones:

```python
def invertir_cadena(texto):
    return texto[::-1]

def es_palindromo(texto):
    texto = texto.lower().replace(" ", "")
    return texto == texto[::-1]
```

### 2. En `tests/test_cadena.py`, implementa las siguientes pruebas:

#### a) Crea un fixture `cadenas_de_prueba` con una lista de frases:

```python
import pytest

@pytest.fixture
def cadenas_de_prueba():
    return ["radar", "hola", "oso", "python", "anita lava la tina"]
```

#### b) Parametriza pruebas para validar:

- Que `invertir_cadena("hola")` devuelve `"aloh"`
- Que `es_palindromo` detecta correctamente palíndromos

### 3. Verifica también que se lanza un error si se intenta invertir una cadena vacía (debes modificar la función para lanzar `ValueError` si `texto` es vacío).

---

## 📊 Entrega

- Incluye en tu repositorio:
  - `src/cadena.py`
  - `tests/test_cadena.py`
  - `README.md` explicando qué pruebas automatizaste.
- Ejecuta con:

```bash
pytest -v -m "not skip"
```

---

## ✅ Propuesta de solución

### `invertir_cadena` modificada:

```python
def invertir_cadena(texto):
    if not texto:
        raise ValueError("La cadena no puede estar vacía.")
    return texto[::-1]
```

### Test de excepción:

```python
def test_invertir_cadena_error():
    with pytest.raises(ValueError, match="no puede estar vacía"):
        invertir_cadena("")
```

---

¡Sube tu repositorio y demuestra tu dominio de PyTest con buenas prácticas!
