# Clase 1 - Semana 7: Introducción a BDD y Escritura de Features

En esta clase exploraremos los fundamentos de **Behavior Driven Development (BDD)** y aprenderemos a escribir especificaciones en lenguaje **Gherkin**. Usaremos la librería **Behave** para Python y ejemplo de sitio de práctica **The Internet** (http://the-internet.herokuapp.com/login).

---

## 1. ¿Qué es Behavior Driven Development (BDD)?

- **BDD** es una metodología que promueve la colaboración entre **QA**, **Desarrollo** y **Negocio**.  
- El objetivo es definir **comportamientos esperados** antes de comenzar a implementar el código.  
- Permite que todas las partes involucradas compartan un lenguaje común, enfocándose en el **valor de negocio**.

---

## 2. Lenguaje Gherkin

- Sintaxis sencilla en texto plano para describir escenarios en lenguaje natural.  
- Palabras clave:
  - **Feature** / **Característica**: descripción general de la funcionalidad.
  - **Scenario** / **Escenario**: caso de uso específico.
  - **Given** / **Dado**: contexto inicial o precondiciones.
  - **When** / **Cuando**: acción o evento que dispara el comportamiento.
  - **Then** / **Entonces**: resultado esperado o verificación.
  - **And/But** / **Y/Pero**: complementos para Given, When o Then.

---

## 3. Ejemplo Práctico en Gherkin

### Archivo `features/login.feature`

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

## 4. Herramienta Behave en Python

- **Behave** es un framework BDD para Python, equivalente a Cucumber.  
- **Estructura de proyecto**:
  ```
  proyecto/
  ├── features/
  │   ├── environment.py  # hooks opcionales
  │   ├── login.feature
  │   └── steps/
  │       └── login_steps.py
  └── requirements.txt
  ```
- Instalar Behave:
  ```bash
  pip install behave selenium
  ```

---

## 5. Definir Step Definitions en Behave

### Archivo `features/steps/login_steps.py`

```python
from behave import given, when, then
from selenium import webdriver
from selenium.webdriver.common.by import By
from selenium.webdriver.support.ui import WebDriverWait
from selenium.webdriver.support import expected_conditions as EC

@given('el usuario ingresa a la página de login')
def step_impl(context):
    context.driver = webdriver.Chrome()
    context.driver.get("http://the-internet.herokuapp.com/login")

@when('el usuario introduce credenciales válidas')
def step_impl(context):
    context.driver.find_element(By.ID, "username").send_keys("tomsmith")
    context.driver.find_element(By.ID, "password").send_keys("SuperSecretPassword!")
    context.driver.find_element(By.CSS_SELECTOR, "button.radius").click()

@when('el usuario introduce credenciales inválidas')
def step_impl(context):
    context.driver.find_element(By.ID, "username").send_keys("invalid")
    context.driver.find_element(By.ID, "password").send_keys("invalid")
    context.driver.find_element(By.CSS_SELECTOR, "button.radius").click()

@then('debería ver la página segura')
def step_impl(context):
    WebDriverWait(context.driver, 10).until(EC.url_contains("/secure"))
    assert "Secure Area" in context.driver.title
    context.driver.quit()

@then('debería ver un mensaje de error')
def step_impl(context):
    error = context.driver.find_element(By.ID, "flash")
    assert "Your username is invalid!" in error.text
    context.driver.quit()
```

---

## 6. Ejecutar las pruebas BDD

- Navega al directorio del proyecto y ejecuta:
  ```bash
  behave
  ```
- Verás la salida con cada **Scenario** y su resultado.

---

## 7. Recursos y Buenas Prácticas

- Usa **Before** y **After** hooks en `environment.py` para setup/teardown global:
  ```python
  # environment.py
  from selenium import webdriver

  def before_all(context):
      context.driver = webdriver.Chrome()

  def after_all(context):
      context.driver.quit()
  ```
- Mantén los **selectors** y lógica de interacción en Page Objects si es necesario.
- Escribe **Features** claros, orientados a valor de negocio y legibles por todos.

---

## 8. Sitios de práctica recomendados

- **The Internet**: http://the-internet.herokuapp.com/login  
- **ToolsQA**: https://demoqa.com/automation-practice-form