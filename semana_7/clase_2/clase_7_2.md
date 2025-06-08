# Clase 2 - Semana 7: Implementación de Pruebas BDD con Selenium, Requests y POM

En esta clase aplicaremos los conceptos de BDD usando **Behave**, integrando pruebas UI con Selenium, API con `requests` y aplicando **Page Object Model (POM)** para mejorar la mantenibilidad.

---

## 1. Implementar escenarios BDD con código

### 1.1 Reutilizar `features/login.feature`

```gherkin
Feature: Autenticación de usuario

  Como usuario registrado
  Quiero poder iniciar sesión
  Para acceder a mis funcionalidades restringidas

  Scenario: Login exitoso
    Given el usuario ingresa a la página de login
    When el usuario introduce credenciales válidas
    Then debería ver la página segura

  Scenario: Login fallido
    Given el usuario ingresa a la página de login
    When el usuario introduce credenciales inválidas
    Then debería ver un mensaje de error
```

---

## 2. Definir Page Object Model para Login

### Archivo `pages/login_page.py`

```python
from selenium.webdriver.common.by import By
from selenium.webdriver.support.ui import WebDriverWait
from selenium.webdriver.support import expected_conditions as EC

class LoginPage:
    URL = "http://the-internet.herokuapp.com/login"
    def __init__(self, driver):
        self.driver = driver
        self.username_input = (By.ID, "username")
        self.password_input = (By.ID, "password")
        self.login_button = (By.CSS_SELECTOR, "button.radius")
        self.flash_message = (By.ID, "flash")

    def load(self):
        self.driver.get(self.URL)

    def login(self, username, password):
        WebDriverWait(self.driver, 10).until(EC.presence_of_element_located(self.username_input))
        self.driver.find_element(*self.username_input).clear()
        self.driver.find_element(*self.username_input).send_keys(username)
        self.driver.find_element(*self.password_input).clear()
        self.driver.find_element(*self.password_input).send_keys(password)
        self.driver.find_element(*self.login_button).click()

    def get_flash_text(self):
        WebDriverWait(self.driver, 10).until(EC.visibility_of_element_located(self.flash_message))
        return self.driver.find_element(*self.flash_message).text
```

---

## 3. Step Definitions con POM y Selenium

### Archivo `features/steps/login_steps.py`

```python
from behave import given, when, then
from selenium import webdriver
from pages.login_page import LoginPage

@given('el usuario ingresa a la página de login')
def step_impl(context):
    context.driver = webdriver.Chrome()
    context.login_page = LoginPage(context.driver)
    context.login_page.load()

@when('el usuario introduce credenciales válidas')
def step_impl(context):
    context.login_page.login("tomsmith", "SuperSecretPassword!")

@when('el usuario introduce credenciales inválidas')
def step_impl(context):
    context.login_page.login("invalid", "invalid")

@then('debería ver la página segura')
def step_impl(context):
    assert "/secure" in context.driver.current_url
    context.driver.quit()

@then('debería ver un mensaje de error')
def step_impl(context):
    flash_text = context.login_page.get_flash_text()
    assert "Your username is invalid!" in flash_text
    context.driver.quit()
```

---

## 4. Definir Page Object Model para API

### Archivo `pages/api_helper.py`

```python
import requests

class APIHelper:
    BASE_API = "https://reqres.in/api"

    def login(self, email, password):
        return requests.post(f"{self.BASE_API}/login", json={"email": email, "password": password})

    def is_service_up(self):
        response = requests.get(f"{self.BASE_API}/users/2")
        return response.status_code == 200
```

---

## 5. Step Definitions con POM y Requests (API)

### Archivo `features/steps/api_login_steps.py`

```python
from behave import given, when, then
from pages.api_helper import APIHelper

@given('el servicio de autenticación está disponible')
def step_impl(context):
    api = APIHelper()
    assert api.is_service_up()

@when('envío credenciales válidas a "/api/login"')
def step_impl(context):
    api = APIHelper()
    context.response = api.login("eve.holt@reqres.in", "cityslicka")

@when('envío credenciales inválidas a "/api/login"')
def step_impl(context):
    api = APIHelper()
    context.response = api.login("invalid", "wrong")

@then('debería recibir un token en la respuesta')
def step_impl(context):
    assert context.response.status_code == 200
    data = context.response.json()
    assert "token" in data

@then('debería recibir un error 401')
def step_impl(context):
    assert context.response.status_code in (400, 401)
```

---

## 6. Ejecutar las pruebas BDD con Behave

1. Navega al directorio raíz de tu proyecto (donde está `features/`).
2. Ejecuta:
   ```bash
   behave
   ```
3. Verás la secuencia de pasos y sus resultados.

---

## 7. Beneficios de integrar POM en BDD

- Mayor **mantenibilidad** y **reutilización**.  
- Mantiene los pasos de Gherkin enfocados en el **qué**, no en el **cómo**.

---

## 8. Alternativas y Ejercicio adicional

- **pytest-bdd**: combinar Gherkin con PyTest.  
- Ejercicio: crear POM para demoblaze search y escribir steps.