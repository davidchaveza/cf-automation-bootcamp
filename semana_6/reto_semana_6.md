
# 🚀 Reto Semana 6 - Multi-navegador y Headless

## 🎯 Objetivo
Adaptar la suite de pruebas web para ejecutarse en **Chrome** y **Firefox**, incluyendo modo **headless**, y documentar diferencias y ajustes.

---

## 🔧 Instrucciones

1. **Modifica** `conftest.py` para parametrizar el driver:
   ```python
   import pytest
   from selenium import webdriver

   @pytest.fixture(params=["chrome", "firefox"], scope="function")
   def driver(request):
       if request.param == "chrome":
           opts = webdriver.ChromeOptions()
           opts.add_argument("--headless")
           drv = webdriver.Chrome(options=opts)
       else:
           opts = webdriver.FirefoxOptions()
           opts.add_argument("-headless")
           drv = webdriver.Firefox(options=opts)
       yield drv
       drv.quit()
   ```

2. **Ejecuta** la suite completa en ambos navegadores:
   ```bash
   pytest -v tests/ui
   ```

3. **Documenta** en tu `README.md`:
   - Pasos seguidos para parametrizar.
   - Comando usado y resultados (logs o capturas).
   - Cualquier diferencia observada entre navegadores o en headless.

---

## 📄 Propuesta de solución

- Se usa `@pytest.fixture(params=[...])` para Chrome/Firefox.
- Se añade `--headless` en opciones de ambos.
- No se comparte estado entre iteraciones ya que fixture es `function`.

---

```bash
# Ejecución de prueba:
pytest -v tests/ui
```
