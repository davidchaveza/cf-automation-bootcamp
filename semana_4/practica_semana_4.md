
# 🧪 Práctica Semana 4 - Refactorización y Reportes

## 🎯 Objetivo
Reestructurar las pruebas de API desarrolladas para aplicar refactorización: centralizar la URL base de la API en un fixture o archivo de configuración, y crear funciones auxiliares para eliminar código repetitivo. Ejecutar el suite completo y generar un reporte HTML utilizando `pytest-html`, verificando que incluya todos los casos.

---

## 🔧 Instrucciones

1. **Configura** tu proyecto con la API pública ReqRes (`https://reqres.in/api`) o JSONPlaceholder.
2. **Crea** un archivo `config.yaml` en la raíz con:
   ```yaml
   base_url: "https://reqres.in/api"
   headers:
     x-api-key: "reqres-free-v1"
   ```
3. **Actualiza** tu `conftest.py` para cargar `config.yaml`:
   ```python
   import pytest, yaml, os

   @pytest.fixture(scope="session")
   def api_config():
       cfg = yaml.safe_load(open("config.yaml"))
       cfg['headers']['x-api-key'] = os.getenv("API_KEY", cfg['headers']['x-api-key'])
       return cfg
   ```
4. **Crea** `tests/api/helpers.py` con funciones:
   ```python
   import requests

   def get(endpoint, api_config, **kwargs):
       return requests.get(f"{api_config['base_url']}{endpoint}", headers=api_config['headers'], **kwargs)

   def post(endpoint, api_config, **kwargs):
       return requests.post(f"{api_config['base_url']}{endpoint}", headers=api_config['headers'], **kwargs)

   # Similar para put y delete...
   ```
5. **Refactoriza** tus tests existentes para utilizar `api_config` y los helpers.
6. **Instala** el plugin de reportes:
   ```bash
   pip install pytest-html
   ```
7. **Ejecuta** el suite completo y genera el reporte:
   ```bash
   pytest -m api --html=report.html --self-contained-html
   ```
8. **Verifica** que `report.html` incluya todas las pruebas y casos.

---

## 📸 Entrega

- Tu `config.yaml`, `conftest.py`, `helpers.py` y tests refactorizados.
- El archivo `report.html` o una captura de pantalla mostrando el reporte.
