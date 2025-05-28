
# 🚀 Reto Semana 5 - Parametrización de Pruebas y Manejo de Casos Inválidos

## 🎯 Objetivo
Parametrizar la prueba del formulario de registro para ejecutarla con diferentes datos, incluyendo conjuntos válidos e inválidos, y documentar los desafíos encontrados.

---

## 🔧 Instrucciones

1. **Amplía** el test de registro en `tests/ui/test_registration.py` usando `@pytest.mark.parametrize`:
   ```python
   import pytest
   from selenium.webdriver.common.by import By
   from selenium.webdriver.common.keys import Keys
   from selenium.webdriver.support.ui import WebDriverWait
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
   ```
2. **Asegura** que cada iteración parte de un estado limpio (el fixture `driver` crea un navegador nuevo).  
3. **Ejecuta** con:
   ```bash
   pytest -v tests/ui/test_registration.py
   ```
4. **Documenta** en tu `README.md`:
   - Los datos usados en la parametrización.
   - Qué casos consideraste válidos e inválidos.
   - Desafíos en la identificación de elementos o sincronización.

---

## 📄 Propuesta de solución

- Se usa `pytest.mark.parametrize` para dos conjuntos de datos.  
- Se detecta la aparición del modal con `WebDriverWait`.  
- Para datos inválidos, se captura `TimeoutException` y se asume no aparición del modal.
