
# Clase 2 - Semana 2: Buenas Prácticas en Diseño de Tests y Parametrización

En esta clase conocerás buenas prácticas para escribir pruebas limpias, reutilizables y fáciles de mantener usando PyTest. También aprenderás a parametrizar pruebas y verificar excepciones correctamente.

---

## 🧱 1. Principio AAA: Arrange - Act - Assert

Organiza tu código de prueba en tres bloques bien diferenciados:

```python
def test_suma():
    # Arrange (preparación)
    a = 4
    b = 6

    # Act (ejecución)
    resultado = a + b

    # Assert (verificación)
    assert resultado == 10
```

Este patrón mejora la legibilidad y mantenimiento de tus pruebas.

---

## 🧪 2. Parametrización de pruebas con PyTest

Permite correr un mismo test con diferentes entradas y resultados esperados:

```python
import pytest

@pytest.mark.parametrize("a, b, esperado", [
    (2, 3, 5),
    (0, 0, 0),
    (-1, -1, -2)
])
def test_suma_parametrizada(a, b, esperado):
    assert a + b == esperado
```

Esto evita duplicar código y mejora la cobertura de escenarios.

---

## ❗ 3. Manejo de excepciones en tests

Puedes verificar si una función lanza una excepción usando `pytest.raises`:

```python
import pytest

def dividir(a, b):
    return a / b

def test_division_por_cero():
    with pytest.raises(ZeroDivisionError):
        dividir(10, 0)
```

También puedes comprobar el mensaje de error:

```python
def test_error_mensaje():
    with pytest.raises(ZeroDivisionError, match="division by zero"):
        dividir(1, 0)
```

---

## 🧩 4. Organización del código de pruebas

Separa el código de aplicación del código de prueba:

```
proyecto/
├── src/
│   └── calculadora.py
├── tests/
│   ├── test_calculadora.py
│   └── conftest.py
├── data/
│   └── datos.csv
├── requirements.txt
└── pytest.ini
```

- `src/`: código de la aplicación.
- `tests/`: pruebas automatizadas.
- `conftest.py`: fixtures compartidos.
- `data/`: archivos de prueba o datos de entrada.

---

## 📊 5. Interpretación de resultados y reportes básicos

Usa `-v` (verbose) para ver el detalle de cada prueba:

```bash
pytest -v
pytest -vv  # aún más detalle
```

Ejemplo de salida:

```bash
test_calculadora.py::test_suma_parametrizada[2-3-5] PASSED
test_calculadora.py::test_suma_parametrizada[0-0-0] PASSED
```

### Generación de reporte (introducción)

Más adelante usaremos plugins como `pytest-html`, pero por ahora puedes redirigir la salida:

```bash
pytest -v > resultado.txt
```

---

## 📚 Conclusión

Estas prácticas te ayudarán a:

- Escribir pruebas limpias y organizadas.
- Probar múltiples escenarios de forma sencilla.
- Detectar excepciones esperadas sin romper el flujo de pruebas.
- Prepararte para reportes y frameworks más robustos.

---

¡Ahora practica creando tus propias pruebas parametrizadas y con manejo de errores!
