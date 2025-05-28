
# Clase 1 - Semana 4: Pruebas Avanzadas de API y Manejo de Datos Dinámicos (con múltiples APIs públicas)

En esta clase exploraremos el uso avanzado de pruebas de API REST usando librerías como `requests` y `jsonschema`. Para mejorar la práctica, usaremos **cuatro APIs públicas**: GoRest, ReqRes, Fake Store API y Swagger Petstore.

---

## 🔐 1. Autenticación en APIs

### a) GoRest (https://gorest.co.in)

```python
headers = {
    "Authorization": "Bearer <tu_token_gorest>",
    "Content-Type": "application/json"
}
response = requests.get("https://gorest.co.in/public/v2/users", headers=headers)
```

### b) ReqRes (https://reqres.in)

```python
headers = {
    "x-api-key": "reqres-free-v1",
    "Content-Type": "application/json"
}
response = requests.get("https://reqres.in/api/users/2", headers=headers)
```

### c) Fake Store API (https://fakestoreapi.com)

```python
# Login con credenciales de prueba
login = requests.post("https://fakestoreapi.com/auth/login", json={"username": "mor_2314", "password": "83r5^_"})
token = login.json()["token"]

# Uso del token
headers = {"Authorization": f"Bearer {token}"}
response = requests.get("https://fakestoreapi.com/carts", headers=headers)
```

---

## 🔄 2. Flujo de prueba dependiente

### a) GoRest

```python
def test_crear_y_actualizar_usuario_gorest():
    token = "<tu_token>"
    headers = {"Authorization": f"Bearer {token}", "Content-Type": "application/json"}

    # Crear usuario
    payload = {"name": "QA Bot", "gender": "male", "email": "qa.test@example.com", "status": "active"}
    crear = requests.post("https://gorest.co.in/public/v2/users", json=payload, headers=headers)
    user_id = crear.json()["id"]

    # Actualizar
    update = {"name": "QA Bot Senior"}
    actualizar = requests.put(f"https://gorest.co.in/public/v2/users/{user_id}", json=update, headers=headers)
    assert actualizar.status_code == 200
```

### b) Fake Store API

```python
def test_crear_y_borrar_producto():
    producto = {"title": "QA Producto", "price": 9.99, "description": "Producto de prueba", "category": "test"}
    crear = requests.post("https://fakestoreapi.com/products", json=producto)
    producto_creado = crear.json()

    eliminar = requests.delete(f"https://fakestoreapi.com/products/{producto_creado['id']}")
    assert eliminar.status_code == 200
```

---

## 📐 3. Validación de esquema de respuesta

### Usando JSON Schema con Petstore

```python
from jsonschema import validate

esquema = {
    "type": "object",
    "properties": {
        "id": {"type": "integer"},
        "name": {"type": "string"},
        "status": {"type": "string"}
    },
    "required": ["id", "name", "status"]
}

respuesta = requests.get("https://petstore.swagger.io/v2/pet/1")
validate(instance=respuesta.json(), schema=esquema)
```

---

## 📄 4. Manejo de paginación

### a) GoRest

```python
def test_paginacion_gorest():
    token = "<tu_token>"
    headers = {"Authorization": f"Bearer {token}"}
    page = 1
    while True:
        r = requests.get(f"https://gorest.co.in/public/v2/users?page={page}", headers=headers)
        datos = r.json()
        if not datos:
            break
        assert isinstance(datos, list)
        page += 1
```

### b) ReqRes

```python
def test_paginacion_reqres():
    page = 1
    while True:
        r = requests.get(f"https://reqres.in/api/users?page={page}", headers={"x-api-key": "reqres-free-v1"})
        data = r.json().get("data", [])
        if not data:
            break
        page += 1
```

---

## ⚠️ 5. Evitar falsos negativos o positivos

### Ejemplo genérico con reintento

```python
import time

def test_get_con_reintento():
    for _ in range(3):
        r = requests.get("https://reqres.in/api/users/2", headers={"x-api-key": "reqres-free-v1"})
        if r.status_code == 200:
            break
        time.sleep(2)
    assert r.status_code == 200
```

---

## 📚 Conclusión

Hoy aprendiste a:

- Aplicar autenticación con tokens (Bearer, API Key).
- Encadenar llamadas (crear → leer → actualizar → eliminar).
- Validar esquemas de JSON con `jsonschema`.
- Navegar APIs paginadas de forma robusta.
- Proteger tus pruebas ante errores intermitentes.

---

¡Explora cada API y adapta estos ejemplos a tu propio framework de automatización!
