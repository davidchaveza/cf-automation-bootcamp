
# 🧪 Práctica Semana 6 - Page Object Model y Pruebas E2E con Swag Labs

## 🎯 Objetivo
Implementar el **Page Object Model (POM)** para la página de login de Swag Labs y refactorizar las pruebas existentes, luego crear una nueva prueba end-to-end que navegue por varias páginas (login, inventario, detalle de producto) usando POM.

---

## 🔧 Instrucciones

1. **Instala** dependencias:
   ```bash
   pip install selenium pytest
   ```

2. **Crea** la clase `LoginPage` en `pages/login_page.py`:
   ```python
   from selenium.webdriver.common.by import By

   class LoginPage:
       URL = "https://www.saucedemo.com/"

       def __init__(self, driver):
           self.driver = driver
           self.username = (By.ID, "user-name")
           self.password = (By.ID, "password")
           self.login_btn = (By.ID, "login-button")

       def load(self):
           self.driver.get(self.URL)

       def login(self, user, pwd):
           self.driver.find_element(*self.username).send_keys(user)
           self.driver.find_element(*self.password).send_keys(pwd)
           self.driver.find_element(*self.login_btn).click()
   ```

3. **Refactoriza** tu prueba de login en `tests/ui/test_login_pom.py` para usar `LoginPage`:
   ```python
   import pytest
   from pages.login_page import LoginPage

   def test_login_pom(driver):
       page = LoginPage(driver)
       page.load()
       page.login("standard_user", "secret_sauce")
       assert "inventory.html" in driver.current_url
   ```

4. **Crea** la clase `InventoryPage` en `pages/inventory_page.py`:
   ```python
   from selenium.webdriver.common.by import By

   class InventoryPage:
       def __init__(self, driver):
           self.driver = driver
           self.items = (By.CLASS_NAME, "inventory_item_name")

       def get_item_names(self):
           return [el.text for el in self.driver.find_elements(*self.items)]
   ```

5. **Crea** la clase `ProductPage` en `pages/product_page.py`:
   ```python
   from selenium.webdriver.common.by import By

   class ProductPage:
       def __init__(self, driver):
           self.driver = driver
           self.add_btn = (By.ID, "add-to-cart-sauce-labs-backpack")
           self.cart_icon = (By.CLASS_NAME, "shopping_cart_link")

       def add_to_cart_and_open(self):
           self.driver.find_element(*self.add_btn).click()
           self.driver.find_element(*self.cart_icon).click()
   ```

6. **Escribe** un test E2E en `tests/ui/test_inventory_flow.py`:
   ```python
   import pytest
   from pages.login_page import LoginPage
   from pages.inventory_page import InventoryPage
   from pages.product_page import ProductPage

   @pytest.mark.ui
   def test_inventory_flow(driver):
       login = LoginPage(driver)
       inv = InventoryPage(driver)
       prod = ProductPage(driver)

       login.load()
       login.login("standard_user", "secret_sauce")

       names = inv.get_item_names()
       assert "Sauce Labs Backpack" in names

       prod.add_to_cart_and_open()
       # Verificar que el carrito contiene el producto
       assert "Your Cart" in driver.title
   ```

7. **Ejecuta** todas las pruebas:
   ```bash
   pytest -v tests/ui
   ```

---

## 📸 Entrega

- Tus clases POM (`login_page.py`, `inventory_page.py`, `product_page.py`).
- Tests refactorizados (`test_login_pom.py`, `test_inventory_flow.py`).
- Captura de pantalla o log mostrando el paso de todas las pruebas.
