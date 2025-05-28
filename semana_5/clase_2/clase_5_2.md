
# Clase 2 - Semana 5: Selenium + PyTest – Estructurando Pruebas Web Automatizadas

En esta clase integraremos Selenium con PyTest para estructurar pruebas web robustas, usando fixtures, esperas, gestión de estados y manejo de errores comunes.

---

## 1. Integración de Selenium en PyTest

### Fixture de navegador en `conftest.py`

```python
import pytest
from selenium import webdriver

@pytest.fixture(scope="function")
def driver():
    # Configuración inicial: driver con Selenium Manager
    driver = webdriver.Chrome()
    driver.implicitly_wait(10)  # Espera implícita de 10s
    yield driver
    # Teardown: limpiar estado
    driver.quit()
```

- **scope="function"**: fixture ejecutada antes y después de cada test.
- `implicitly_wait(10)`: espera implícita para todos los find_element.

---

## 2. Manejo de esperas en la web

### a) Esperas implícitas

- Se configuran una vez con `driver.implicitly_wait(segundos)`.
- Esperan que los elementos estén presentes antes de lanzar excepción.

### b) Esperas explícitas

```python
from selenium.webdriver.common.by import By
from selenium.webdriver.support.ui import WebDriverWait
from selenium.webdriver.support import expected_conditions as EC

# Espera explícita
wait = WebDriverWait(driver, 15)
element = wait.until(
    EC.element_to_be_clickable((By.CSS_SELECTOR, "#login button"))
)
```

- Útiles para condiciones específicas (clicable, visibilidad, presencia).

### c) Fluent Waits

```python
from selenium.webdriver.common.by import By
from selenium.webdriver.support.ui import WebDriverWait
from selenium.webdriver.support import expected_conditions as EC
from selenium.common.exceptions import NoSuchElementException

# Fluent Wait: timeout de 20s, poll_frequency de 2s, ignorar NoSuchElementException
wait = WebDriverWait(
    driver,
    timeout=20,
    poll_frequency=2,
    ignored_exceptions=[NoSuchElementException]
)
dynamic_elem = wait.until(
    EC.visibility_of_element_located((By.ID, "dynamic-element"))
)
```

- **poll_frequency** controla cada cuántos segundos reintenta la condición.
- **ignored_exceptions** permite ignorar excepciones intermedias hasta el timeout.

---

## 3. Ejemplo de prueba completa: Login en The Internet

Sitio demo: [Heroku App Login](http://the-internet.herokuapp.com/login)

```python
# tests/ui/test_login.py
import pytest
from selenium.webdriver.common.by import By
from selenium.webdriver.support.ui import WebDriverWait
from selenium.webdriver.support import expected_conditions as EC

LOGIN_URL = "http://the-internet.herokuapp.com/login"
SECURE_URL = "http://the-internet.herokuapp.com/secure"

def test_login_exitoso(driver):
    driver.get(LOGIN_URL)

    # Arrange: espera para campos
    wait = WebDriverWait(driver, 10)
    username = wait.until(EC.presence_of_element_located((By.ID, "username")))
    password = driver.find_element(By.ID, "password")
    login_button = driver.find_element(By.CSS_SELECTOR, "button.radius")

    # Act: enviar credenciales
    username.clear()
    username.send_keys("tomsmith")
    password.clear()
    password.send_keys("SuperSecretPassword!")
    login_button.click()

    # Assert: URL y mensaje
    wait.until(EC.url_contains("/secure"))
    assert driver.current_url == SECURE_URL
    mensaje = driver.find_element(By.ID, "flash")
    assert "You logged into a secure area!" in mensaje.text
```

---

## 4. Gestión de estados

- **Antes de pruebas**: asegurar estado inicial (logout):
  ```python
  driver.get("http://the-internet.herokuapp.com/logout")
  ```
- **Después de pruebas**: volver a estado limpio en el teardown (`driver.quit()`).

---

## 5. Solución de problemas comunes

### a) Manejo de alertas

```python
driver.get("http://the-internet.herokuapp.com/javascript_alerts")
driver.find_element(By.XPATH, "//button[text()='Click for JS Alert']").click()
alert = driver.switch_to.alert
assert alert.text == "I am a JS Alert"
alert.accept()
```

### b) Múltiples ventanas/pestañas

```python
driver.get("http://the-internet.herokuapp.com/windows")
driver.find_element(By.LINK_TEXT, "Click Here").click()

main = driver.current_window_handle
for handle in driver.window_handles:
    if handle != main:
        driver.switch_to.window(handle)
        break
assert "New Window" in driver.title
driver.close()
driver.switch_to.window(main)
```

### c) Captura de pantalla en fallo

```python
# en el test
try:
    assert False
except AssertionError:
    driver.save_screenshot("error.png")
    raise
```

---

## 📚 Conclusión

- Integrar Selenium en PyTest mejora la limpieza y repetibilidad.
- Usa esperas implícitas, explícitas y fluent waits según necesidad.
- Administra estado del navegador entre tests.
- Maneja alertas, ventanas y captura pantallas para depuración.

Próxima clase: **Page Object Model** y pruebas end-to-end con múltiples páginas.


rom selenium.webdriver.support.ui import WebDriverWait
   from selenium.webdriver.support import expected_conditions as EC
   from selenium.common.exceptions import TimeoutException

   @pytest.mark.ui
   @pytest.mark.parametrize("first, last, email, mobile, valid", [
       ("John", "Doe", "john.doe@example.com", "1234567890", True),
       ("Jane", "Smith", "invalid-email", "abcdefg", False),
   ])
   def test_registration_param(driver, first, last, email, mobile, valid):
       driver.get("https://demoqa.com/automation-practice-form")

       driver.find_element(By.ID, "firstName").send_keys(first)
       driver.find_element(By.ID, "lastName").send_keys(last)
       driver.find_element(By.ID, "userEmail").send_keys(email)
       driver.find_element(By.ID, "userNumber").send_keys(mobile)

       submit = driver.find_element(By.ID, "submit")
       driver.execute_script("arguments[0].scrollIntoView();", submit)
       submit.click()

       wait = WebDriverWait(driver, 5)
       try:
           modal = wait.until(
               EC.visibility_of_element_located((By.ID, "example-modal-sizes-title-lg"))
           )
           appeared = True
       except TimeoutException:
           appeared = False

       if valid:
           assert appeared, "Expected success modal for valid data"
       else:
           assert not appeared, "Did not expect modal for invalid data"
Asegura que cada iteración parte de un estado limpio (el fixture driver crea un navegador nuevo).

Ejecuta con:

bash
Copy
Edit
pytest -v tests/ui/test_registration.py
Documenta en tu README.md:

Los datos usados en la parametrización.

Qué casos consideraste válidos e inválidos.

Desafíos en la identificación de elementos o sincronización.