
# Clase 1 - Semana 3: Fundamentos de Pruebas de API REST

En esta clase aprenderás los conceptos clave de las APIs REST, cómo interactuar con ellas usando Python y cómo validar sus respuestas de forma básica. Usaremos la API pública [reqres.in](https://reqres.in/) para nuestras pruebas. Actualmente, esta API requiere el uso de una clave gratuita (`x-api-key`) en cada petición.

---

## 🌐 1. ¿Qué es una API REST?

Una API (Application Programming Interface) permite que dos sistemas se comuniquen entre sí.  
Las **APIs REST** usan el protocolo HTTP para enviar y recibir datos en formato JSON.

### Métodos HTTP más comunes:

| Método | Descripción             | Ejemplo                     |
|--------|-------------------------|-----------------------------|
| GET    | Obtener información     | /api/users/2                |
| POST   | Crear un nuevo recurso  | /api/users                  |
| PUT    | Actualizar un recurso   | /api/users/2                |
| DELETE | Eliminar un recurso     | /api/users/2                |

### Códigos de estado HTTP:

| Código | Significado                    |
|--------|--------------------------------|
| 200    | OK                             |
| 201    | Creado                         |
| 400    | Solicitud incorrecta           |
| 401    | No autorizado                  |
| 404    | No encontrado                  |
| 500    | Error interno del servidor     |

### Cabeceras HTTP:

- `Content-Type: application/json`
- `x-api-key: reqres-free-v1` ← Obligatoria para usar la API de reqres.in

---

## 📡 2. Introducción a la librería Requests

Instalación:

```bash
pip install requests
```

### Definición de headers

```python
headers = {
    "x-api-key": "reqres-free-v1",
    "Content-Type": "application/json"
}
```

### Ejemplo: Hacer un GET

```python
import requests

response = requests.get("https://reqres.in/api/users/2", headers=headers)

print(response.status_code)
print(response.json())
```

### Ejemplo: Hacer un POST

```python
payload = {"name": "morpheus", "job": "leader"}
response = requests.post("https://reqres.in/api/users", json=payload, headers=headers)

print(response.status_code)
print(response.json())
```

### Ejemplo: Hacer un PUT

```python
update = {"name": "morpheus", "job": "zion resident"}
response = requests.put("https://reqres.in/api/users/2", json=update, headers=headers)

print(response.status_code)
print(response.json())
```

### Ejemplo: Hacer un DELETE

```python
response = requests.delete("https://reqres.in/api/users/2", headers=headers)
print(response.status_code)  # Esperado: 204
```

---

## 🧾 3. Análisis de respuestas JSON

### Parsear JSON en Python:

```python
data = response.json()
print(data.get("name"))
```

---

## ✅ 4. Verificaciones básicas en APIs

Verificar que una API responde correctamente:

```python
def test_get_usuario():
    headers = {
        "x-api-key": "reqres-free-v1"
    }

    response = requests.get("https://reqres.in/api/users/2", headers=headers)

    assert response.status_code == 200

    data = response.json()
    assert "data" in data
    assert data["data"]["id"] == 2
```

---

## 📚 Conclusión

Hoy aprendiste:

- Qué es una API REST y cómo se estructura.
- Cómo usar `requests` para consumir una API REST pública.
- Cómo incluir headers como `x-api-key` en tus peticiones.
- Cómo validar datos JSON y códigos de estado.

---

¡Explora otros endpoints de `https://reqres.in/api` y practica con diferentes métodos y cabeceras!
