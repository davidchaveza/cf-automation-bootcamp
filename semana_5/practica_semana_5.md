
# 🧪 Práctica Semana 5 - Prueba de Registro Web con Selenium y PyTest

## 🎯 Objetivo
Desarrollar una prueba automatizada con Selenium que verifique una funcionalidad básica en una página de prueba. Usaremos el formulario de registro de **ToolsQA** en https://demoqa.com/automation-practice-form.

---

## 🔧 Instrucciones

1. **Instala** las dependencias:
   ```bash
   pip install selenium pytest
   ```
2. **Crea** un fixture en `conftest.py` para iniciar y cerrar el navegador:
   ```python
   # conftest.py
   import pytest
   from selenium import webdriver

   @pytest.fixture(scope="function")
   def driver():
       driver = webdriver.Chrome()
       yield driver
       driver.quit()
   ```
3. **Crea** el archivo de prueba `tests/ui/test_registration.py`:
   ```python
   import pytest
   from selenium.webdriver.common.by import By
   from selenium.webdriver.common.keys import Keys
   from selenium.webdriver.support.ui import WebDriverWait
   from selenium.webdriver.support import expected_conditions as EC

   @pytest.mark.ui
   def test_registration_form(driver):
       driver.get("https://demoqa.com/automation-practice-form")

       # Llenar formulario
       driver.find_element(By.ID, "firstName").send_keys("John")
       driver.find_element(By.ID, "lastName").send_keys("Doe")
       driver.find_element(By.ID, "userEmail").send_keys("john.doe@example.com")
       driver.find_element(By.ID, "userNumber").send_keys("1234567890")

       # Enviar formulario
       submit = driver.find_element(By.ID, "submit")
       driver.execute_script("arguments[0].scrollIntoView();", submit)
       submit.click()

       # Verificar modal de éxito
       wait = WebDriverWait(driver, 10)
       modal_title = wait.until(
           EC.visibility_of_element_located((By.ID, "example-modal-sizes-title-lg"))
       )
       assert modal_title.text == "Thanks for submitting the form"
   ```
4. **Ejecuta** la prueba:
   ```bash
   pytest -v tests/ui/test_registration.py
   ```
5. **Entrega**:
   - El archivo de prueba `test_registration.py`.
   - Fixture en `conftest.py`.
   - Captura de pantalla o log de la ejecución mostrando que la prueba pasó.
