# Proyecto Final (Desarrollo del Framework de Pruebas)

## Planificación y Diseño del Proyecto Final

---

### 1. Presentación del Proyecto Final

**Sistema a probar**:  
Se propone un **e-commerce simplificado** con:
- **Interfaz web** para el usuario (UI)  
- **API REST** para operaciones de backend  
- Funcionalidades clave: registro/login, catálogo de productos, carrito de compras, checkout (simulado), gestión de ordenes.

**Alcances y requisitos de pruebas**:  
- Automatizar pruebas de UI (flujo de compra, validaciones de formulario).  
- Automatizar pruebas de API (endpoints CRUD: usuarios, productos, órdenes).  
- Integración continua de toda la suite de pruebas.

---

### 2. Definición del Alcance de las Pruebas

Identificar funcionalidades críticas a automatizar:

| Capa    | Funcionalidad                                      | Tipo de prueba               |
|---------|-----------------------------------------------------|------------------------------|
| UI      | Registro y Login de usuario                         | E2E (login/logout)           |
| UI      | Navegación de catálogo y búsqueda de productos      | E2E                          |
| UI      | Agregar al carrito y flujo de compra                | End-to-end                   |
| API     | Crear, leer, actualizar y borrar usuarios           | Tests de API (CRUD)          |
| API     | Listar productos                                    | Tests de API (GET)           |
| API     | Crear y consultar órdenes                           | Tests de API (POST/GET)      |

---

### 3. Arquitectura del Framework de Pruebas

**Estructura de carpetas propuesta**:

```
project/
├── .github/workflows/           # CI/CD pipelines
├── pages/                       # Page Objects (UI)
│   ├── login_page.py
│   ├── product_page.py
│   └── cart_page.py
├── tests/
│   ├── api/
│   │   ├── test_users.py
│   │   └── test_orders.py
│   └── ui/
│       ├── test_login.py
│       └── test_purchase_flow.py
├── fixtures/                    # PyTest fixtures (env, data)
│   └── conftest.py
├── data/                        # Datos de prueba (JSON, CSV)
│   └── users.json
├── configs/                     # Configuraciones (URLs, credenciales)
│   └── config.yml
└── requirements.txt
```

**Convenciones de nombres**:
- **Tests**: `test_<module>_<feature>.py`  
- **Page Objects**: `<Name>Page` en `pages/<name>_page.py`  
- **Fixtures**: funciones en `tests/conftest.py`, scope adecuado (function, module).

**Herramientas**:
- **PyTest** para ejecución de tests  
- **Selenium** para UI  
- **Requests** para API  
- **Behave** (opcional) para casos BDD  
- Plugins: `pytest-html`, `pytest-xdist`, etc.

---

### 4. Estrategia de Trabajo

- **Individual**: cada alumno desarrolla su propio framework.  
- **Grupal**: división en roles:
  - **Equipo API**: diseñar y automatizar endpoints.  
  - **Equipo UI**: implementar tests E2E y POM.  
  - **Equipo CI/CD**: configurar pipelines y reportes.

**Flujo de Git**:
1. Crear rama feature: `feature/login-ui`  
2. Desarrollar tests y Page Objects  
3. Pull Request a `main`  
4. Revisión de código y merge si pasa CI  

---

### 5. Checklist Inicial

Antes de comenzar:
- [ ] Python instalado y entorno virtual configurado  
- [ ] IDE configurado (PyCharm)  
- [ ] Repositorio clonado y acceso a remoto  
- [ ] URL de la aplicación y credenciales disponibles  
- [ ] Drivers de Selenium accesibles (Selenium Manager o manual)  
- [ ] Configuración de CI habilitada en GitHub Actions  

**Criterios de evaluación**:
- Cobertura de tests (UI y API)  
- Calidad del código (estructura, POM, fixtures)  
- Documentación (README, comentarios)  
- Integración Continua (paso de tests en CI)