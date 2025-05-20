
# Clase 2 - Semana 3: Automatización de Pruebas de API con PyTest

En esta clase aplicaremos lo aprendido sobre APIs REST y `requests` para crear pruebas automatizadas con PyTest. Usaremos la API pública [reqres.in](https://reqres.in/) con su respectiva API Key (`x-api-key`) para realizar pruebas reales con métodos GET, POST, PUT y DELETE.

---

## 🗂️ 1. Organización de pruebas de API

Recomendación de estructura:

```
proyecto/
├── src/
├── tests/
│   └── api/
│       └── test_users.py
├── conftest.py
├── requirements.txt
```

---

## ⚙️ 2. Fixture para configuración base

```python
# conftest.py
import pytest

@pytest.fixture(scope="session")
def api_config():
    return {
        "base_url": "https://reqres.in/api",
        "headers": {
            "x-api-key": "reqres-free-v1",
            "Content-Type": "application/json"
        }
    }
```

---

## 🧪 3. Prueba GET - Obtener un usuario

```python
# tests/api/test_users.py
import requests

def test_get_usuario(api_config):
    url = f"{api_config['base_url']}/users/2"
    response = requests.get(url, headers=api_config["headers"])

    assert response.status_code == 200
    data = response.json()
    assert "data" in data
    assert data["data"]["id"] == 2
    assert "email" in data["data"]
```

---

## ➕ 4. Prueba POST - Crear un usuario

```python
def test_crear_usuario(api_config):
    payload = {"name": "morpheus", "job": "leader"}
    response = requests.post(f"{api_config['base_url']}/users", json=payload, headers=api_config["headers"])

    assert response.status_code == 201
    data = response.json()
    assert data["name"] == "morpheus"
    assert "id" in data
```

---

## ✏️ 5. Prueba PUT - Actualizar un usuario

```python
def test_actualizar_usuario(api_config):
    payload = {"name": "neo", "job": "the one"}
    response = requests.put(f"{api_config['base_url']}/users/2", json=payload, headers=api_config["headers"])

    assert response.status_code == 200
    data = response.json()
    assert data["job"] == "the one"
```

---

## ❌ 6. Prueba DELETE - Eliminar un usuario

```python
def test_eliminar_usuario(api_config):
    response = requests.delete(f"{api_config['base_url']}/users/2", headers=api_config["headers"])
    assert response.status_code == 204
```

---

## 🔄 7. Buenas prácticas

- Usa `fixtures` para configuración compartida (base URL, headers).
- Cada prueba debe ser **independiente**.
- Nombres claros y descriptivos en funciones de test.
- Evita hardcodear datos que se repiten.
- Valida tipos y campos importantes en la respuesta (`isinstance`, claves JSON, etc.).

---

## ▶️ 8. Ejecutar las pruebas

```bash
pytest -v tests/api/test_users.py
```

---

## 📚 Conclusión

Hoy aprendiste:

- Cómo estructurar y automatizar pruebas para APIs REST.
- Cómo probar correctamente métodos GET, POST, PUT y DELETE usando `requests` y `pytest`.
- Cómo reutilizar configuración con `fixtures`.

---

¡Explora más endpoints de [https://reqres.in](https://reqres.in) y construye tu propia suite de pruebas automatizadas!
