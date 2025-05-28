
# Clase 2 - Semana 6: Pruebas End-to-End y Multi-Navegador

En esta clase construiremos casos de prueba completos de extremo a extremo (E2E), gestionaremos múltiples páginas con POM, ejecutaremos en distintos navegadores y hablaremos de paralelismo y buenas prácticas. Como sitio de práctica usaremos **Demoblaze**: https://www.demoblaze.com

---

## 1. Casos de Prueba End-to-End

**Ejemplo: Flujo de compra en Demoblaze**  
Sitio demo: https://www.demoblaze.com  

### Flujo
1. **Cargar Home**  
2. **Seleccionar categoría** (ej: Laptops)  
3. **Elegir un producto** (ej: “Sony vaio i5”)  
4. **Agregar al carrito**  
5. **Ir al carrito** y verificar el producto agregado  

---

## 2. Manejo de Múltiples Páginas con POM

### Estructura de POM
- `pages/home_page.py`
- `pages/category_page.py`
- `pages/product_page.py`
- `pages/cart_page.py`

#### `pages/home_page.py`
```python
from selenium.webdriver.common.by import By

class HomePage:
    URL = "https://www.demoblaze.com"

    def __init__(self, driver):
        self.driver = driver
        self.category_laptops = (By.LINK_TEXT, "Laptops")

    def load(self):
        self.driver.get(self.URL)

    def go_to_laptops(self):
        self.driver.find_element(*self.category_laptops).click()
```

#### `pages/category_page.py`
```python
from selenium.webdriver.common.by import By

class CategoryPage:
    def __init__(self, driver):
        self.driver = driver

    def select_product(self, product_name):
        locator = (By.LINK_TEXT, product_name)
        self.driver.find_element(*locator).click()
```

#### `pages/product_page.py`
```python
from selenium.webdriver.common.by import By

class ProductPage:
    def __init__(self, driver):
        self.driver = driver
        self.add_to_cart = (By.LINK_TEXT, "Add to cart")
        self.cart_link = (By.ID, "cartur")

    def add_to_cart_and_open(self):
        self.driver.find_element(*self.add_to_cart).click()
        alert = self.driver.switch_to.alert
        alert.accept()
        self.driver.find_element(*self.cart_link).click()
```

#### `pages/cart_page.py`
```python
from selenium.webdriver.common.by import By

class CartPage:
    def __init__(self, driver):
        self.driver = driver
        self.cart_items = (By.CSS_SELECTOR, ".success td:nth-child(2)")

    def get_product_names(self):
        elems = self.driver.find_elements(*self.cart_items)
        return [e.text for e in elems]
```

#### Test E2E con POM
```python
# tests/ui/test_purchase_flow.py
def test_purchase_flow(driver):
    from pages.home_page import HomePage
    from pages.category_page import CategoryPage
    from pages.product_page import ProductPage
    from pages.cart_page import CartPage

    home = HomePage(driver)
    category = CategoryPage(driver)
    product = ProductPage(driver)
    cart = CartPage(driver)

    home.load()
    home.go_to_laptops()
    category.select_product("Sony vaio i5")
    product.add_to_cart_and_open()

    names = cart.get_product_names()
    assert "Sony vaio i5" in names
```

---

## 3. Ejecución en Múltiples Navegadores

### Fixture parametrizado en `conftest.py`
```python
import pytest
from selenium import webdriver

@pytest.fixture(params=["chrome", "firefox"], scope="function")
def driver(request):
    if request.param == "chrome":
        options = webdriver.ChromeOptions()
        options.add_argument("--headless")
        drv = webdriver.Chrome(options=options)
    else:
        options = webdriver.FirefoxOptions()
        options.add_argument("-headless")
        drv = webdriver.Firefox(options=options)
    yield drv
    drv.quit()
```

Ejecuta tests con:
```bash
pytest -v tests/ui/test_purchase_flow.py
```

---

## 4. Pruebas en Paralelo (Concepto)

- Instala `pytest-xdist`:  
  ```bash
  pip install pytest-xdist
  ```
- Ejecuta en paralelo:  
  ```bash
  pytest -m ui -n 2
  ```
- **Consideraciones**:  
  - Aislamiento de datos y estado.  
  - Servicios de CI pueden requerir WebDriver en contenedores.

---

## 5. Mejores Prácticas de Mantenimiento

- **Actualizar selectores** en POM cuando cambia la UI.  
- **Datos de prueba** fuera del código (YAML/JSON).  
- **Casos críticos** en E2E; mantener la suite rápida.  
- **Capturas** y **logs** para fallos graves.