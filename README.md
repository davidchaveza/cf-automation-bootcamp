# CF Automation Bootcamp

![CI Pipeline](https://github.com/terranigmark/cf-automation-bootcamp/actions/workflows/ci.yml/badge.svg)

Repositorio del Bootcamp de **QA Automation** con Python, donde encontrarás un framework de pruebas automatizadas para web y APIs, ejercicios prácticos, retos semanales y un proyecto final.

## 📋 Contenido

- **Semana 1-8**: Clases, prácticas y retos enfocados en automatización con PyTest, Selenium, Requests y Behave.
- **Proyecto Final**: Desarrollo de un framework completo de pruebas (UI y API) con CI/CD.
- **.github/workflows/**: Pipelines de GitHub Actions para CI.
- **pages/**: Page Objects para pruebas UI.
- **tests/**:
  - **api/**: Tests de API automatizados.
  - **ui/**: Tests UI (E2E, POM).
- **features/**: Scenarios BDD con Behave.
- **fixtures/**: `conftest.py` y configuración de PyTest.
- **data/**: Archivos de datos de prueba (JSON, CSV).
- **configs/**: Configuraciones de entorno (URLs, credenciales).
- **docs/**: Material de clase, rúbrica y planeación del proyecto.

## 🚀 Cómo empezar

### Requisitos previos

- Python 3.9+  
- Git  
- Navegador Chrome o Firefox  

### Instalación

```bash
git clone https://github.com/terranigmark/cf-automation-bootcamp.git
cd cf-automation-bootcamp
python -m venv venv
source venv/bin/activate      # Linux/macOS
# .\venv\Scripts\activate  # Windows
pip install --upgrade pip
pip install -r requirements.txt
```

### Ejecutar pruebas

- **Tests de API y UI**:
  ```bash
  pytest --maxfail=1 --disable-warnings -q
  ```
- **Generar reporte HTML**:
  ```bash
  pytest --html=report.html --self-contained-html
  ```

- **BDD (Behave)**:
  ```bash
  behave
  ```

## 🏗 Estructura del proyecto

```
├── .github/workflows/    # Pipelines CI/CD
├── pages/                # Page Objects (POM)
├── tests/
│   ├── api/              # Pruebas API con PyTest + Requests
│   └── ui/               # Pruebas UI con PyTest + Selenium
├── features/             # Pruebas BDD con Behave
├── fixtures/             # PyTest fixtures (conftest.py)
├── data/                 # Datos de prueba
├── configs/              # Configuraciones (config.yml)
├── docs/                 # Documentación, temarios, rúbrica
├── requirements.txt
└── README.md
```

## 📈 CI/CD

Este repositorio utiliza **GitHub Actions** para ejecutar automáticamente la suite de pruebas en cada push y pull request a la rama `main`. Los resultados y reportes (HTML y screenshots) se publican como artefactos en cada ejecución.

## 📑 Rúbrica de Evaluación

Consulta la [rúbrica del proyecto final](docs/Rubrica_Proyecto_Final.md) para conocer los criterios de evaluación.

## 🤝 Contribuciones

1. Haz un fork del repositorio.  
2. Crea una rama (`feature/nueva-prueba`).  
3. Realiza tus cambios y tests.  
4. Abre un Pull Request.

## 📄 Licencias

- Código bajo [MIT License](LICENSE)  
- Materiales de curso bajo [CC BY-NC 4.0](LICENSE-docs.md)

---

**¡Bienvenido al Bootcamp de QA Automation!**
