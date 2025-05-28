
# 🚀 Reto Semana 4 - Diseño de Plan de Pruebas y Demostración

## 🎯 Objetivo
Documentar y diseñar un plan de pruebas para un servicio web simulado, y demostrar la implementación de 1-2 casos de prueba automatizados.

---

## 📘 Parte 1: Diseño del plan de pruebas

1. Selecciona una API pública de tu elección (por ejemplo, JSONPlaceholder `/todos`).
2. Define las funcionalidades y casos de prueba clave:
   - CRUD de recursos (GET, POST, PUT, DELETE).
   - Validación de respuestas y esquemas.
   - Manejo de datos inválidos y códigos de error.
   - Paginación si aplica.
3. Para cada caso, detalla:
   - **Descripción** del caso.
   - **Método HTTP** y **endpoint**.
   - **Datos de entrada** (si aplica).
   - **Resultado esperado**.
4. Guarda este plan en un archivo `plan_de_pruebas.md`.

---

## 📂 Parte 2: Implementación de casos de prueba

1. Crea `tests/api/test_todos.py`.
2. Implementa al menos **dos** casos de prueba:
   - GET `/todos/1` → verificar status 200 y esquema.
   - POST `/todos` → crear un recurso y validar status 201.
3. Ejecuta tus pruebas con:
   ```bash
   pytest -v tests/api/test_todos.py
   ```

---

## 📄 Propuesta de solución

### Test GET `/todos/1`

```python
import requests
import pytest

@pytest.mark.api
def test_get_todo():
    url = "https://jsonplaceholder.typicode.com/todos/1"
    response = requests.get(url)
    assert response.status_code == 200
    data = response.json()
    assert "userId" in data and "title" in data and "completed" in data
```

### Test POST `/todos`

```python
import requests

def test_create_todo():
    url = "https://jsonplaceholder.typicode.com/todos"
    payload = {"userId": 1, "title": "Nueva tarea", "completed": False}
    response = requests.post(url, json=payload)
    assert response.status_code == 201
    data = response.json()
    assert data["title"] == payload["title"]
```
