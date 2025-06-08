# Clase 1 - Semana 8: Conceptos de CI/CD y Pipelines de Testing

En esta clase nos enfocaremos en los fundamentos de Integración Continua (CI) y Entrega/Despliegue Continuo (CD), con un enfoque práctico en **GitHub Actions**.

---

## 1. Introducción a Integración Continua (CI)

- **Definición**: Práctica de integrar y verificar el código con frecuencia (múltiples veces al día) mediante procesos automáticos.  
- **Beneficios para QA**:
  - Detección temprana de fallos: errores se identifican tan pronto se suben cambios.  
  - Retroalimentación rápida: los equipos reciben notificaciones inmediatas.  
  - Consistencia: entornos reproducibles para ejecutar pruebas.  

---

## 2. Introducción a Entrega/Despliegue Continuo (CD)

- **Definición**: Práctica de liberar software de forma frecuente y controlada, automatizando despliegues a entornos de prueba o producción.  
- **Rol de pruebas automatizadas**: aseguran que cada versión liberada cumple con los criterios de calidad, reduciendo riesgos en producción.

---

## 3. Herramientas Populares de CI/CD

| Herramienta        | Configuración        | Hosting         | Gratis para Open Source | Integraciones        | Facilidad de uso (1–5) |
|--------------------|----------------------|-----------------|-------------------------|----------------------|------------------------|
| Jenkins            | Jenkinsfile (Groovy) | Self-hosted     | Sí                      | Muy amplia           | 3                      |
| Travis CI          | .travis.yml          | SaaS           | Sí                      | GitHub, Slack, Docker| 4                      |
| GitLab CI/CD       | .gitlab-ci.yml       | SaaS/Self-hosted| Sí                      | Integrado con GitLab | 4                      |
| GitHub Actions     | .github/workflows/*.yml | SaaS        | Sí (públicos)           | Marketplace amplio   | 5                      |

---

## 4. GitHub Actions: ¿Por qué elegirla?

**Pros**:
- Integración nativa con GitHub (repositorios, pull requests, issues).  
- Workflows como código en YAML dentro del repo.  
- Matrices de construcción para múltiples entornos (SO, versiones de Python, etc.).  
- Marketplace de acciones reutilizables.  
- Minutes gratuitos para repositorios públicos.  

**Contras**:
- Límite de minutos gratuitos para repositorios privados.  
- Lock-in a la plataforma GitHub.  
- Curva de aprendizaje inicial para escribir YAML complejos.

---

## 5. Creación de un Pipeline Básico con GitHub Actions

### Estructura de archivos
```
.yarn/
.github/
└── workflows/
    └── python-tests.yml
tests/
src/
```

### Ejemplo: `.github/workflows/python-tests.yml`

```yaml
name: Python Test Suite

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
        python-version: [3.8, 3.9, 3.10]

    steps:
      - name: Checkout code
        uses: actions/checkout@v3

      - name: Set up Python
        uses: actions/setup-python@v4
        with:
          python-version: \${{ matrix.python-version }}

      - name: Install dependencies
        run: |
          pip install --upgrade pip
          pip install -r requirements.txt

      - name: Run tests
        run: pytest --maxfail=1 --disable-warnings -q

      - name: Upload test report
        if: always()
        uses: actions/upload-artifact@v3
        with:
          name: pytest-report
          path: report.xml
```

---

## 6. Notificaciones de Resultados

- **GitHub Checks**: estatus (✔️ o ❌) aparece en cada PR.  
- **Slack**: usar acción como `8398a7/action-slack` para enviar notificaciones.  
- **Email**: integraciones con correo electrónico o webhooks.  

---

### Resumen

1. CI/CD acelera la detección de errores y la entrega de valor.  
2. GitHub Actions es una opción poderosa para proyectos en GitHub.  
3. Con archivos YAML puedes definir pipelines reproducibles.  
4. Integrar notificaciones y artefactos mejora la visibilidad del equipo.

---

**¡Con esto tienes lo esencial para implementar CI/CD con GitHub Actions en tu proyecto!**