
# Clase 2 - Semana 4: Construcción de un Mini Framework de Pruebas de API

En esta clase veremos cómo **refactorizar**, **estructurar** y **potenciar** tu suite de pruebas de API usando PyTest, creando un mini framework que facilite la mantenibilidad, generación de reportes y ejecución en paralelo e integración continua.

---

## 1. Refactorización y Helpers

Crear un módulo de helpers reutilizable:

```python
# tests/api/helpers.py
import requests

def api_request(method: str, endpoint: str, api_config: dict, **kwargs):
    url = f"{api_config['base_url']}{endpoint}"
    return requests.request(method, url, headers=api_config['headers'], **kwargs)

def get(endpoint: str, api_config: dict, **kwargs):
    return api_request('GET', endpoint, api_config, **kwargs)

def post(endpoint: str, api_config: dict, **kwargs):
    return api_request('POST', endpoint, api_config, **kwargs)

def put(endpoint: str, api_config: dict, **kwargs):
    return api_request('PUT', endpoint, api_config, **kwargs)

def delete(endpoint: str, api_config: dict, **kwargs):
    return api_request('DELETE', endpoint, api_config, **kwargs)
```

Ejemplo en un test:

```python
# tests/api/test_users.py
from tests.api.helpers import get, post
import pytest

def test_get_user(api_config):
    r = get("/users/2", api_config)
    assert r.status_code == 200

def test_create_user(api_config):
    payload = {"name": "CI Bot", "job": "tester"}
    r = post("/users", api_config, json=payload)
    assert r.status_code == 201
```

---

## 2. Estructura del proyecto

Mantén la configuración en un solo lugar (`config.yaml` o variables de entorno):

```yaml
# config.yaml
base_url: "https://reqres.in/api"
headers:
  x-api-key: "reqres-free-v1"
```

```python
# conftest.py
import pytest, yaml, os

@pytest.fixture(scope="session")
def api_config():
    cfg = yaml.safe_load(open("config.yaml"))
    # Sobrescribir con env var si existen
    cfg['headers']['x-api-key'] = os.getenv("API_KEY", cfg['headers']['x-api-key'])
    return cfg
```

Estructura recomendada:

```
proyecto/
├── config.yaml
├── conftest.py
├── pytest.ini
├── requirements.txt
└── tests/
    └── api/
        ├── helpers.py
        ├── test_users.py
        └── test_products.py
```

---

## 3. Generación de reportes HTML

Instala el plugin:

```bash
pip install pytest-html
```

Ejecuta:

```bash
pytest -m api --html=report.html
```

Abre `report.html` en tu navegador para ver el reporte de resultados.

---

## 4. Ejecución en paralelo

Instala `pytest-xdist`:

```bash
pip install pytest-xdist
```

Ejecuta tests en paralelo:

```bash
pytest -m api -n auto
```

**Consideraciones**: asegúrate de que los tests no compartan estado (datos únicos, limpieza entre pruebas).

---

## 5. Preparación para CI

Define un marker en `pytest.ini`:

```ini
# pytest.ini
[pytest]
markers =
    api: Pruebas de API
```

Ejemplo de workflow en GitHub Actions:

```yaml
name: API Tests

on: [push]

jobs:
  test-api:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      - uses: actions/setup-python@v4
        with:
          python-version: '3.x'
      - run: pip install -r requirements.txt
      - run: pytest -m api --html=report.html --self-contained-html
      - uses: actions/upload-artifact@v3
        with:
          name: api-test-report
          path: report.html
```

Con esto, tu suite de pruebas API es:

- **Modular** y fácil de mantener
- **Configurada** de manera centralizada
- **Capaz** de generar reportes HTML
- **Escalable** con ejecución en paralelo
- **Lista** para integrarse en pipelines de CI

---

¡Listo! Has construido un mini framework de pruebas de API profesional.
