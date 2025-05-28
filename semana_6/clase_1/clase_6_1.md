
# Clase 1 - Semana 6: Page Object Model (POM) y Mantenibilidad de Pruebas UI

En esta sesión aprenderás a aplicar el patrón **Page Object Model (POM)** para desacoplar la lógica de localización de elementos de las pruebas, mejorando la legibilidad y el mantenimiento de tu suite de pruebas UI. Usaremos como sitio de práctica el demo **The Internet** (http://the-internet.herokuapp.com) y **ToolsQA**.

---

## 1. Introducción al patrón Page Object Model

El **Page Object Model** es un patrón de diseño que sugiere representar cada página (o componente) de la aplicación como una **clase** en Python.  
- **Ventajas**:
  - Centraliza selectores y acciones.
  - Facilita el mantenimiento cuando cambia la UI.
  - Mejora la legibilidad de los tests.

---

## 2. Implementación de un POM básico

### Archivo `pages/login_page.py`

```python
from selenium.webdriver.common.by import By

class LoginPage:
    def __init__(self, driver):
        self.driver = driver
        self.url = "http://the-internet.herokuapp.com/login"
        self.username_input = (By.ID, "username")
        self.password_input = (By.ID, "password")
        self.login_button = (By.CSS_SELECTOR, "button.radius")

    def load(self):
        self.driver.get(self.url)

    def login(self, username, password):
        self.driver.find_element(*self.username_input).clear()
        self.driver.find_element(*self.username_input).send_keys(username)
        self.driver.find_element(*self.password_input).clear()
        self.driver.find_element(*self.password_input).send_keys(password)
        self.driver.find_element(*self.login_button).click()
```

---

## 3. Refactorización de tests para usar POM

### Antes (sin POM)

```python
def test_login_sin_pom(driver):
    driver.get("http://the-internet.herokuapp.com/login")
    driver.find_element(By.ID, "username").send_keys("tomsmith")
    driver.find_element(By.ID, "password").send_keys("SuperSecretPassword!")
    driver.find_element(By.CSS_SELECTOR, "button.radius").click()
    assert "/secure" in driver.current_url
```

### Después (con POM)

```python
# tests/ui/test_login_pom.py
import pytest
from selenium.webdriver.common.by import By
from selenium.webdriver.support.ui import WebDriverWait
from selenium.webdriver.support import expected_conditions as EC
from pages.login_page import LoginPage

def test_login_pom(driver):
    page = LoginPage(driver)
    page.load()
    page.login("tomsmith", "SuperSecretPassword!")
    WebDriverWait(driver, 10).until(EC.url_contains("/secure"))
    assert "Secure Area" in driver.title
```

---

## 4. Interacciones avanzadas con Selenium

### a) ActionChains

Para acciones complejas como **arrastrar y soltar**, **hover** o **doble click**:

```python
from selenium.webdriver import ActionChains
from selenium.webdriver.common.by import By

def test_drag_and_drop(driver):
    driver.get("http://the-internet.herokuapp.com/drag_and_drop")
    source = driver.find_element(By.ID, "column-a")
    target = driver.find_element(By.ID, "column-b")
    actions = ActionChains(driver)
    actions.drag_and_drop(source, target).perform()
    assert driver.find_element(By.ID, "column-a").text == "B"
```

---

## 5. Captura de evidencias (screenshots)

Puedes tomar capturas en puntos clave o al detectarse un fallo.  
Por ejemplo, en un **hook** de PyTest:

```python
# conftest.py
import pytest

@pytest.hookimpl(hookwrapper=True)
def pytest_runtest_makereport(item, call):
    outcome = yield
    rep = outcome.get_result()
    if rep.when == "call" and rep.failed:
        driver = item.funcargs['driver']
        driver.save_screenshot(f"screenshots/{item.name}.png")
```

---

## 6. Sitios de práctica

- **The Internet**: http://the-internet.herokuapp.com  
  - Login, Drag and Drop, JavaScript Alerts, ventanas, etc.
- **ToolsQA Practice**: https://demoqa.com/automation-practice-form  
  - Formularios, widgets, interacciones avanzadas.

---

¡Ahora tu suite UI será más mantenible y escalable gracias al Page Object Model!

