
# Clase 1 - Semana 5: Fundamentos de Automatización Web con Selenium

En esta clase veremos los conceptos básicos de Selenium WebDriver y cómo empezar a automatizar interacciones en el navegador.

---

## 1. ¿Qué es Selenium WebDriver?

Selenium WebDriver es una herramienta de código abierto para automatizar navegadores web.  
**Arquitectura y componentes**:
- **Cliente WebDriver**: API que envía comandos al navegador.
- **Drivers específicos**:  
  - **ChromeDriver** para Google Chrome  
  - **GeckoDriver** para Mozilla Firefox  
  - **IEDriverServer** para Internet Explorer  
- **Navegadores soportados**: Chrome, Firefox, Edge, Safari, IE, Opera, entre otros.

---

## 2. Configuración inicial

### Instalación de la librería

```bash
pip install selenium
```

### Descarga y configuración del WebDriver

1. **ChromeDriver**:  
   - Descargar desde https://chromedriver.chromium.org/downloads  
   - Asegúrate de que la versión coincida con tu navegador Chrome.  
   - Descomprime y ubica el ejecutable en una carpeta incluida en tu **PATH** o define la ruta al inicializar:
     ```python
     driver = webdriver.Chrome(executable_path="ruta/a/chromedriver")
     ```
2. **GeckoDriver** (Firefox):  
   - Descargar desde https://github.com/mozilla/geckodriver/releases  
   - Agrega al **PATH** o especifica la ruta al instanciar:
     ```python
     driver = webdriver.Firefox(executable_path="ruta/a/geckodriver")
     ```

---

## 3. Navegación básica con Selenium

```python
from selenium import webdriver

# Instanciar Chrome (requiere chromedriver en PATH)
driver = webdriver.Chrome()

# Cargar URL de prueba
driver.get("https://example.com")

# Mostrar título de la página
print(driver.title)

# Cerrar el navegador
driver.quit()
```

---

## 4. Estrategias de localización de elementos

Importar la clase By:

```python
from selenium.webdriver.common.by import By
```

Métodos comunes:

| Estrategia   | Método                     | Uso                                    |
|--------------|----------------------------|----------------------------------------|
| ID           | `find_element(By.ID, id)`  | `driver.find_element(By.ID, "username")` |
| Name         | `find_element(By.NAME, name)` | `driver.find_element(By.NAME, "q")`  |
| Class Name   | `find_element(By.CLASS_NAME, class)` | `driver.find_element(By.CLASS_NAME, "btn")` |
| CSS Selector | `find_element(By.CSS_SELECTOR, selector)` | `driver.find_element(By.CSS_SELECTOR, "#main > a")` |
| XPath        | `find_element(By.XPATH, xpath)` | `driver.find_element(By.XPATH, "//div[@id='main']//a")` |

Para múltiples elementos:
```python
elements = driver.find_elements(By.TAG_NAME, "a")
```

---

## 5. Interacciones simples

Supongamos un formulario de login:

```python
from selenium import webdriver
from selenium.webdriver.common.by import By

driver = webdriver.Chrome()
driver.get("https://example.com/login")

# Localizar campos
username = driver.find_element(By.ID, "username")
password = driver.find_element(By.ID, "password")
login_button = driver.find_element(By.ID, "login")

# Ingresar texto
username.send_keys("mi_usuario")
password.send_keys("mi_contraseña")

# Click en botón
login_button.click()

# Esperar y obtener mensaje
mensaje = driver.find_element(By.ID, "welcome-message")
print(mensaje.text)  # Validar texto mostrado

# Limpiar un campo
username.clear()

driver.quit()
```

---

### Buenas prácticas iniciales

- Siempre **cerrar** el navegador con `driver.quit()` al finalizar.
- Usar **esperas explícitas** (WebDriverWait) para sincronización (veremos en próximas clases).
- Organizar selectores en un **Page Object Model** (POM) para mantenibilidad.

---

¡Con esto tienes los fundamentos para comenzar a automatizar pruebas web con Selenium!
