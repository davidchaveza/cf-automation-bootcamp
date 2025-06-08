# Clase 2 - Semana 8: Implementación Práctica de CI para la Suite de Pruebas

En esta clase configuraremos un pipeline real de Integración Continua (CI) usando **GitHub Actions** para ejecutar la suite completa de PyTest, incluyendo pruebas UI en modo headless, y exploraremos cómo publicar reportes y artefactos.

---

## 1. Configuración del Pipeline CI

### Estructura de directorios

```
.github/
└── workflows/
    └── ci.yml
requirements.txt
tests/
src/
```

### Ejemplo de archivo: `.github/workflows/ci.yml`

```yaml
name: CI Pipeline

on:
  push:
    branches: [ main ]
  pull_request:
    branches: [ main ]

jobs:
  build-and-test:
    runs-on: ubuntu-latest
    strategy:
      matrix:
        python-version: [3.9, 3.10]

    steps:
      - name: Checkout code
        uses: actions/checkout@v3

      - name: Setup Python ${{ matrix.python-version }}
        uses: actions/setup-python@v4
        with:
          python-version: ${{ matrix.python-version }}

      - name: Install Dependencies
        run: |
          python -m pip install --upgrade pip
          pip install -r requirements.txt
          pip install pytest pytest-html selenium requests

      - name: Run PyTest (Headless UI + API)
        env:
          DISPLAY: :99
        run: |
          # Iniciar Xvfb para pruebas UI headless (opcional)
          sudo apt-get update && sudo apt-get install -y xvfb
          Xvfb :99 -screen 0 1920x1080x24 &
          pytest --html=report.html --self-contained-html

      - name: Upload Test Report
        if: always()
        uses: actions/upload-artifact@v3
        with:
          name: pytest-report
          path: report.html

      - name: Upload Screenshots
        if: failure()
        uses: actions/upload-artifact@v3
        with:
          name: ui-screenshots
          path: screenshots/*.png
```

---

## 2. Particularidades para Pruebas UI en CI

- **Headless Mode**: usar opciones de Chrome/Firefox headless:
  ```python
  options = webdriver.ChromeOptions()
  options.add_argument("--headless")
  options.add_argument("--no-sandbox")
  options.add_argument("--disable-dev-shm-usage")
  driver = webdriver.Chrome(options=options)
  ```
- **Xvfb**: emular un servidor X virtual en Linux para navegadores.
- **Drivers**: Selenium Manager gestiona drivers automáticamente con Selenium 4.6+.

---

## 3. Generación y Publicación de Reportes

- **pytest-html**: generar reporte HTML con `--html=report.html`.
- **Artifacts**: usar `actions/upload-artifact` para que el reporte y capturas estén disponibles tras la ejecución.
- **Captura de Pantallas**: en hooks de PyTest o fixtures, salvar screenshots de fallos en `screenshots/`.

---

## 4. Introducción Conceptual a CD

- **Despliegue Automático**: tras CI exitoso, un job adicional puede:
  - Construir y publicar una imagen Docker.
  - Desplegar en entorno de staging (por ejemplo, usando `appleboy/ssh-action`).
- **Ejemplo de paso CD** (en `ci.yml`):
  ```yaml
  deploy:
    needs: build-and-test
    runs-on: ubuntu-latest
    steps:
      - name: Deploy to Staging
        uses: appleboy/ssh-action@v0.1.6
        with:
          host: ${{ secrets.STAGING_HOST }}
          username: ${{ secrets.STAGING_USER }}
          key: ${{ secrets.STAGING_KEY }}
          script: |
            docker pull myapp:latest
            docker run -d -p 80:80 myapp:latest
  ```

---

## Resumen

1. Definimos un workflow en GitHub Actions para ejecutar pruebas unitarias, de API y UI en modo headless.  
2. Instalamos dependencias y configuramos Xvfb para CI en Linux.  
3. Publicamos reportes y capturas como artefactos descargables.  
4. Concepto de CD: agregar pasos de despliegue para un pipeline completo CI/CD.
