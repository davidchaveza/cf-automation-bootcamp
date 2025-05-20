
# Clase 1 - Semana 2: Uso Intermedio de PyTest – Estructura, Aserciones y Fixtures

En esta clase aprenderás a estructurar mejor tus pruebas con PyTest, usar correctamente las aserciones, emplear fixtures para datos comunes y categorizar tus tests con marcas (`markers`).

---

## 🔍 1. Anatomía de un test en PyTest

### Reglas de descubrimiento de pruebas

- Los archivos deben llamarse `test_*.py` o `*_test.py`
- Las funciones deben llamarse `test_*`
- Se recomienda ubicar los tests en una carpeta `tests/`

```bash
pytest                 # Descubre todos los tests automáticamente
pytest tests/test_matematica.py  # Ejecuta un archivo específico
```

### Setup/Teardown implícito

PyTest ejecuta código **antes y después de cada test** con ayuda de `fixtures`. Aun así, se pueden hacer setups simples directamente en el cuerpo del test:

```python
def test_ejemplo():
    # Setup
    a = 5
    b = 3
    # Ejecución
    resultado = a + b
    # Validación
    assert resultado == 8
```

---

## ✅ 2. Uso de aserciones en Python/PyTest

La validación en PyTest se realiza con `assert`. Algunos ejemplos:

```python
assert 1 == 1
assert "QA" in "Bootcamp de QA"
assert len([1, 2, 3]) == 3
assert {"a": 1} == {"a": 1}
```

Aserciones que fallan generan errores legibles:

```python
assert 3 == 5
# AssertionError: assert 3 == 5
```

Puedes incluir mensajes personalizados:

```python
assert 3 == 5, "El resultado esperado era 5, pero se obtuvo 3"
```

---

## 🧪 3. Introducción a fixtures en PyTest

Los **fixtures** son funciones especiales que PyTest ejecuta antes de las pruebas. Se usan para preparar datos o configuración.

```python
import pytest

@pytest.fixture
def datos_usuario():
    return {"nombre": "Ana", "edad": 28}

def test_nombre(datos_usuario):
    assert datos_usuario["nombre"] == "Ana"
```

### Alcance del fixture

Puedes definir cuándo se ejecuta un fixture:

```python
@pytest.fixture(scope="function")  # por defecto
@pytest.fixture(scope="module")   # una vez por archivo
```

---

## 🏷️ 4. Marcas (Markers) en PyTest

Sirven para **categorizar** pruebas:

```python
import pytest

@pytest.mark.smoke
def test_carga_rapida():
    assert True

@pytest.mark.regression
def test_regresion_total():
    assert True
```

### Ejecutar por marca desde la terminal:

```bash
pytest -m "smoke"
pytest -m "regression"
```

### Registrar marcas personalizadas (opcional en pytest.ini):

```ini
# pytest.ini
[pytest]
markers =
    smoke: pruebas básicas de fumador
    regression: pruebas completas de regresión
```

---

## 📚 Conclusión

Con estas herramientas puedes:
- Organizar tus tests de forma más clara.
- Compartir datos entre tests sin repetir código.
- Ejecutar subconjuntos de pruebas según su tipo o prioridad.

---

¿Listo para probarlo en código? A continuación, crea tus propios fixtures y usa `@pytest.mark` para categorizar tus pruebas.
