# 🧪 Práctica Semana 8 - Integración Continua en el Proyecto

## 🎯 Objetivo
Implementar la integración continua en el repositorio del bootcamp: agregar un pipeline que, al hacer **push**, ejecute los tests de API y de UI, y verificar en la plataforma CI que las pruebas corren correctamente (verde/rojo).

---

## 🔧 Instrucciones

1. **Crear el workflow**  
   - Dentro de tu repositorio, crea el archivo `.github/workflows/ci.yml` con el siguiente contenido:

   ```yaml
   name: CI Pipeline

   on:
     push:
       branches: [ main ]
     pull_request:
       branches: [ main ]

   jobs:
     test:
       runs-on: ubuntu-latest
       strategy:
         matrix:
           python-version: [3.9, 3.10]

       steps:
         - name: Checkout code
           uses: actions/checkout@v3

         - name: Set up Python ${{ matrix.python-version }}
           uses: actions/setup-python@v4
           with:
             python-version: ${{ matrix.python-version }}

         - name: Install Dependencies
           run: |
             python -m pip install --upgrade pip
             pip install -r requirements.txt
             pip install pytest selenium requests pytest-html

         - name: Run tests (API + UI headless)
           env:
             DISPLAY: :99
           run: |
             sudo apt-get update && sudo apt-get install -y xvfb
             Xvfb :99 -screen 0 1920x1080x24 &
             pytest --html=report.html --self-contained-html
   ```

2. **Commit y Push**  
   ```bash
   git add .github/workflows/ci.yml
   git commit -m "Add CI pipeline to run API and UI tests"
   git push origin main
   ```

3. **Verificar en GitHub Actions**  
   - Abre la pestaña **Actions** en tu repositorio de GitHub.  
   - Observa el workflow **CI Pipeline** ejecutándose.  
   - Confirma que los pasos de instalación y ejecución de tests finalizan en **✔️ Success** o **❌ Failure**.

4. **Ajustes de entorno si falla**  
   - Revisa los logs de la ejecución para identificar errores (dependencias faltantes, path de WebDriver, permisos).  
   - Modifica `ci.yml` o tu código de tests (por ejemplo, agregar opciones `--headless` en WebDriver) y repite el commit hasta que el pipeline pase exitosamente.

5. **Documentar**  
   - Agrega un apartado en el `README.md` de tu proyecto indicando:
     - Ubicación y nombre del workflow (`.github/workflows/ci.yml`).  
     - Comando para ejecutar localmente (`pytest`).  
     - Observaciones de ajustes realizados en CI (instalación de Xvfb, drivers, etc.).