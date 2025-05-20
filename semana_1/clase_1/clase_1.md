
# Clase 1 - Conceptos Básicos de QA Automation y Preparación del Entorno (con consideraciones para Windows)

---

## 🧪 1. Introducción a la automatización de pruebas

**Objetivos:**
- Reducir el esfuerzo repetitivo del testing manual.
- Aumentar la cobertura de pruebas.
- Identificar errores temprano en el ciclo de desarrollo.
- Ejecutar pruebas rápidas y precisas.

**Beneficios:**
- Velocidad: Ejecución más rápida en comparación con pruebas manuales.
- Precisión: Menor margen de errores humanos.
- Reusabilidad: Reutilización y mantenimiento más fácil.
- Cobertura extendida: Pruebas más completas y diversas.

**Alcance dentro del ciclo de QA:**
- Desde pruebas unitarias hasta pruebas end-to-end automatizadas.
- Integración continua con pipelines CI/CD.

---

## 🔍 2. QA manual vs. QA automatizado

| Aspecto         | QA Manual                        | QA Automatizado                    |
|-----------------|----------------------------------|------------------------------------|
| Ejecución       | Manual, por personas             | Mediante scripts                   |
| Escalabilidad   | Limitada                         | Alta                               |
| Costos iniciales| Bajos                            | Altos, pero se compensan con uso  |
| Ideal para      | Exploración, usabilidad          | Repetitivas, regresión, smoke test|

**Tipos de pruebas:**
- **Unitarias**: A nivel del código.
- **Integración**: Entre componentes.
- **End-to-End**: Flujo completo desde el punto de vista del usuario.

**Pirámide de pruebas:**
- Base: pruebas unitarias.
- Medio: integración.
- Cima: pruebas UI (más costosas y lentas).

---

## 🛠️ 3. Configuración del entorno de desarrollo en PyCharm

### 1. Instalación de Python

- Descargar desde: https://www.python.org/downloads/
- Verificar instalación:
```bash
python --version
```

### 2. Crear entorno virtual (PyCharm o terminal)

#### En terminal:

```bash
python -m venv venv
# Activación
# Windows (cmd/PowerShell):
venv\Scripts\activate

# Git Bash/Cmder:
source venv/Scripts/activate
```

#### En PyCharm:
- Crear nuevo proyecto → New Environment → Virtualenv

### 3. Instalar librerías

```bash
pip install pytest
```

---

## 🔧 4. Control de versiones con Git y GitHub

### Verificar instalación:
```bash
git --version
```

### Configurar usuario:
```bash
git config --global user.name "Tu Nombre"
git config --global user.email "tu_email@example.com"
```

### Comandos básicos:

```bash
git init
git add .
git commit -m "Primer commit"
git branch -M main
git remote add origin https://github.com/usuario/repositorio.git
git push -u origin main
```

---

## 🗂️ 5. Organización inicial del proyecto

```plaintext
qa-automation-bootcamp/
├── tests/
│   └── test_ejemplo.py
├── venv/
├── requirements.txt
├── README.md
└── .gitignore
```

`.gitignore` recomendado:

```gitignore
venv/
__pycache__/
.idea/
*.pyc
```

---

## 🧪 6. Primer test con PyTest

### Código de ejemplo: `tests/test_ejemplo.py`

```python
def sumar(a, b):
    return a + b

def test_sumar():
    resultado = sumar(3, 5)
    assert resultado == 8
```

### Ejecutar el test:
```bash
pytest tests/test_ejemplo.py
```

---

# 💡 Consideraciones especiales para usuarios de Windows

---

## ✅ Terminal recomendada

| Opción       | Recomendación | Ventajas principales                                         |
|--------------|---------------|--------------------------------------------------------------|
| **cmd.exe**  | ❌ Evitar      | Limitada, sin soporte completo para scripts o colores.       |
| **PowerShell** | ⚠️ Válida     | Mejor que cmd, pero algunos comandos Unix no funcionan igual. |
| **Git Bash** | ✅ Recomendada | Compatible con la mayoría de comandos de Git y Unix.         |
| **Cmder**    | ✅ Recomendada | Más completo, con soporte para múltiples shells.              |

---

## 🐍 Activación de entornos virtuales

```bash
# cmd/PowerShell:
venv\Scripts\activate

# Git Bash o Cmder:
source venv/Scripts/activate
```

---

## 🧰 Instalación de herramientas clave

- **Python**: https://www.python.org/downloads/windows/
  - Marcar **"Add Python to PATH"**
- **Git**: https://git-scm.com/download/win
  - Terminal recomendada: **Git Bash**

---

## 📁 Rutas en Windows

```bash
# cmd:
C:\Users\usuario\proyecto

# Git Bash:
/c/Users/usuario/proyecto
```

---

## 🛠 Buenas prácticas en PyCharm

- Usar terminal integrada con Git Bash o Cmder.
- Configurar entorno virtual desde:
  `File > Settings > Project > Python Interpreter`
- Encoding: UTF-8 si hay errores.

---

## 🧪 Verificaciones rápidas

```bash
python --version
pip --version
git --version
pytest --version
```

Y:

```bash
python -m venv venv
venv\Scripts\activate
```

---

¡Listos para comenzar con la automatización de pruebas!