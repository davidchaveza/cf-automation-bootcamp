
# 🧪 Práctica Semana 2 - PyTest: Parametrización y Fixtures

## 🎯 Objetivo
Aplicar el uso de PyTest para validar múltiples escenarios utilizando **fixtures** y **parametrización**, mejorando la cobertura de pruebas y evitando código repetido.

---

## 🧩 Instrucciones

### 1. Clona o continúa tu repositorio del bootcamp.

### 2. Crea una carpeta `src/` con un archivo `utilidades.py` que contenga las siguientes funciones:

```python
def es_par(numero):
    return numero % 2 == 0

def mayusculas(texto):
    return texto.upper()
```

### 3. Crea un archivo `tests/test_utilidades.py` y:

#### a) Escribe un **fixture** para una lista de números:

```python
import pytest

@pytest.fixture
def lista_numeros():
    return [2, 3, 4, 5, 6]
```

#### b) Usa la fixture para probar que los elementos pares se identifican correctamente con `es_par`.

#### c) Parametriza una prueba para `mayusculas` con distintos textos:

```python
@pytest.mark.parametrize("entrada, esperado", [
    ("hola", "HOLA"),
    ("Qa", "QA"),
    ("bootcamp", "BOOTCAMP")
])
def test_mayusculas(entrada, esperado):
    assert mayusculas(entrada) == esperado
```

### 4. Ejecuta las pruebas con PyTest:

```bash
pytest -v
```

---

## 📸 Entrega

- Asegúrate de tener los archivos `src/utilidades.py`, `tests/test_utilidades.py` y `conftest.py` (si lo usas).
- Haz `push` de tu código a tu repositorio de GitHub.
- Sube un **screenshot o log de consola** con el resultado de la ejecución exitosa.
