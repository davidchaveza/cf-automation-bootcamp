
# 🧪 Práctica Semana 3 - Pruebas Básicas con Requests y PyTest

## 🎯 Objetivo
Escribir pruebas automatizadas utilizando `requests` y `pytest` sobre la API pública [JSONPlaceholder](https://jsonplaceholder.typicode.com), aplicando lo aprendido sobre validación de respuestas.

---

## 🔧 Instrucciones

### 1. Estructura sugerida

```
proyecto/
├── tests/
│   └── api/
│       └── test_jsonplaceholder.py
├── requirements.txt
├── README.md
```

### 2. Instala dependencias necesarias

```bash
pip install requests pytest
```

---

## ✅ Actividades

### 1. Verificar que un GET a `/posts` retorna una lista de 100 posts

```python
import requests

def test_get_todos_los_posts():
    response = requests.get("https://jsonplaceholder.typicode.com/posts")
    assert response.status_code == 200
    data = response.json()
    assert isinstance(data, list)
    assert len(data) == 100
```

---

### 2. Verificar que un GET a `/posts/1` contiene los campos esperados

```python
def test_get_post_especifico():
    response = requests.get("https://jsonplaceholder.typicode.com/posts/1")
    data = response.json()
    assert response.status_code == 200
    assert "userId" in data
    assert "id" in data
    assert "title" in data
    assert "body" in data
```

---

### 3. Verificar que un POST crea un recurso y retorna código 201

```python
def test_crear_post():
    payload = {"title": "foo", "body": "bar", "userId": 1}
    response = requests.post("https://jsonplaceholder.typicode.com/posts", json=payload)
    assert response.status_code == 201
    data = response.json()
    assert data["title"] == "foo"
```

---

## 📸 Entrega

- Incluye los archivos de prueba en tu repositorio.
- Ejecuta las pruebas localmente con:

```bash
pytest -v tests/api/test_jsonplaceholder.py
```

- Sube una captura o log de la ejecución al README.
