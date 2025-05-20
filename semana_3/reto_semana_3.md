
# 🚀 Reto Semana 3 - Casos de Error, Bordes y Headers en Pruebas API

## 🎯 Objetivo
Escribir pruebas automatizadas que cubran **escenarios negativos** y **manejo de headers**, simulando autenticación en endpoints ficticios o públicos.

---

## 🧠 Instrucciones

### 1. Agrega pruebas para los siguientes casos:

#### a) Recurso inexistente: GET `/posts/9999`

```python
def test_get_post_inexistente():
    response = requests.get("https://jsonplaceholder.typicode.com/posts/9999")
    assert response.status_code in [200, 404]
    # JSONPlaceholder devuelve 200 con objeto vacío
    assert response.json() == {}
```

#### b) POST con datos faltantes (sin title/body)

```python
def test_crear_post_incompleto():
    payload = {"userId": 1}
    response = requests.post("https://jsonplaceholder.typicode.com/posts", json=payload)
    assert response.status_code == 201  # JSONPlaceholder responde 201 incluso si faltan campos
```

> ⚠️ Nota: JSONPlaceholder es una API simulada y no valida campos; en una API real esto retornaría 400.

---

### 2. Simular autenticación con headers (API ficticia)

```python
def test_post_con_header_auth():
    url = "https://jsonplaceholder.typicode.com/posts"
    headers = {"Authorization": "Bearer token-ficticio"}
    payload = {"title": "secure", "body": "data", "userId": 1}
    response = requests.post(url, json=payload, headers=headers)

    assert response.status_code == 201
    assert response.json()["title"] == "secure"
```

---

## 📝 Documentación

En tu archivo `README.md`, incluye:

- Cómo ejecutar los tests
- Qué cubre cada uno (positivos, negativos, headers, etc.)
- Limitaciones de JSONPlaceholder como API simulada

---

## ✅ Entrega

- Sube tus archivos y pruebas al repositorio.
- Verifica que todos los tests se ejecutan con:

```bash
pytest -v
```

- Adjunta evidencia o resumen de los resultados en el README.
