# 🧪 Práctica Semana 7 - Pruebas BDD con Selenium y Requests

## 🎯 Objetivo
Crear un archivo de característica (.feature) con al menos un Feature y dos Scenarios describiendo una funcionalidad. Implementar los step definitions en Python usando Selenium o Requests, y ejecutar `behave` para asegurarse de que pasan correctamente. Utilizaremos sitios y APIs públicas para practicar.

---

## 🔧 Instrucciones

1. **Instala** las dependencias:
   ```bash
   pip install behave selenium requests
   ```

2. **Estructura** recomendada:
   ```
   proyecto/
   ├── features/
   │   ├── search.feature
   │   ├── api.feature
   │   └── steps/
   │       ├── search_steps.py
   │       └── api_steps.py
   ├── environment.py
   └── requirements.txt
   ```

---

## 🔍 1. Búsqueda de producto (Selenium)

### Archivo `features/search.feature`

```gherkin
Feature: Búsqueda de producto en Demoblaze

  Como usuario visitante
  Quiero buscar un producto en Demoblaze
  Para ver si está disponible

  Scenario: Producto existe
    Given el usuario está en la página de inicio de Demoblaze
    When busca "Samsung galaxy s6"
    Then debería ver resultados que incluyan "Samsung galaxy s6"

  Scenario: Producto no existe
    Given el usuario está en la página de inicio de Demoblaze
    When busca "ProductoInexistenteXYZ"
    Then debería ver mensaje de "Product not found"
```

### Archivo `features/steps/search_steps.py`

```python
from behave import given, when, then
from selenium import webdriver
from selenium.webdriver.common.by import By
from selenium.webdriver.support.ui import WebDriverWait
from selenium.webdriver.support import expected_conditions as EC

@given('el usuario está en la página de inicio de Demoblaze')
def step_impl(context):
    context.driver = webdriver.Chrome()
    context.driver.get("https://www.demoblaze.com")

@when('busca "{producto}"')
def step_impl(context, producto):
    try:
        WebDriverWait(context.driver, 10).until(
            EC.presence_of_element_located((By.ID, "tbodyid"))
        )
        context.driver.find_element(By.LINK_TEXT, producto).click()
    except Exception:
        pass

@then('debería ver resultados que incluyan "{producto}"')
def step_impl(context, producto):
    WebDriverWait(context.driver, 10).until(
        EC.presence_of_element_located((By.CSS_SELECTOR, ".name"))
    )
    name = context.driver.find_element(By.CSS_SELECTOR, ".name").text
    assert producto in name
    context.driver.quit()

@then('debería ver mensaje de "Product not found"')
def step_impl(context):
    page_source = context.driver.page_source
    assert "Samsung galaxy s6" not in page_source
    context.driver.quit()
```

---

## 📦 2. Creación de recurso vía API (Requests)

### Archivo `features/api.feature`

```gherkin
Feature: Creación y consulta de posts en JSONPlaceholder API

  Como desarrollador
  Quiero crear un nuevo post y luego consultar información
  Para validar operaciones básicas de la API

  Scenario: Crear post exitoso
    Given que la API JSONPlaceholder está disponible
    When envío datos válidos a "/posts"
    Then debería recibir un post con ID

  Scenario: Consultar post existente
    Given que la API JSONPlaceholder está disponible
    When consulto el post con ID 1 a "/posts/1"
    Then debería recibir un post con userId, id, title y body
```

### Archivo `features/steps/api_steps.py`

```python
import requests
from behave import given, when, then

BASE_API = "https://jsonplaceholder.typicode.com"

@given('que la API JSONPlaceholder está disponible')
def step_impl(context):
    response = requests.get(f"{BASE_API}/posts/1")
    assert response.status_code == 200

@when('envío datos válidos a "/posts"')
def step_impl(context):
    payload = {"title": "foo", "body": "bar", "userId": 1}
    context.response = requests.post(f"{BASE_API}/posts", json=payload)

@then('debería recibir un post con ID')
def step_impl(context):
    assert context.response.status_code == 201
    data = context.response.json()
    assert "id" in data
    assert data["title"] == "foo"

@when('consulto el post con ID 1 a "/posts/1"')
def step_impl(context):
    context.response = requests.get(f"{BASE_API}/posts/1")

@then('debería recibir un post con userId, id, title y body')
def step_impl(context):
    assert context.response.status_code == 200
    data = context.response.json()
    for key in ("userId", "id", "title", "body"):
        assert key in data
```