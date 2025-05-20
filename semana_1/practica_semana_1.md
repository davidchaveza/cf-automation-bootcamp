
# 🧪 Práctica Semana 1 - Configuración y Primer Test con PyTest

## 🎯 Objetivo
Configurar correctamente el entorno de desarrollo local, inicializar un repositorio Git y escribir tu primer test automatizado utilizando PyTest.

---

## 🔧 Parte 1: Configuración del entorno

1. **Instala Python** desde https://www.python.org/downloads/
   - Asegúrate de marcar "Add Python to PATH" durante la instalación.
2. **Instala Git** desde https://git-scm.com/downloads
3. **Crea un nuevo proyecto** en PyCharm.
4. **Crea un entorno virtual**:
   - Desde PyCharm: selecciona la opción "New environment using Virtualenv".
   - O desde terminal:
     ```bash
     python -m venv venv
     ```
5. **Activa el entorno virtual**:
   - Windows (cmd/PowerShell): `venv\Scripts\activate`
   - Git Bash: `source venv/Scripts/activate`

---

## 🗃️ Parte 2: Repositorio Git

1. Abre la terminal en la raíz del proyecto.
2. Ejecuta los siguientes comandos:

```bash
git init
git add .
git commit -m "Configuración inicial del proyecto"
```

3. Crea un repositorio en GitHub y conéctalo:

```bash
git remote add origin https://github.com/tu_usuario/nombre_repo.git
git branch -M main
git push -u origin main
```

---

## ✅ Parte 3: Primer test con PyTest

1. Instala la librería PyTest:
```bash
pip install pytest
```

2. Crea un archivo en la carpeta `tests/` llamado `test_funciones.py`.

3. Escribe una función simple y su test:

```python
# funciones.py
def suma(a, b):
    return a + b
```

```python
# tests/test_funciones.py
from funciones import suma

def test_suma():
    assert suma(3, 4) == 7
```

4. Ejecuta el test desde la terminal:

```bash
pytest tests/test_funciones.py
```

Deberías ver un mensaje que indique que la prueba pasó correctamente.

---

## 📸 Entrega

1. Asegúrate de que tu repositorio tenga:
   - Código fuente y tests separados.
   - Archivo `README.md`.
2. Sube una **captura de pantalla** o un **log de consola** que muestre el resultado del test ejecutado correctamente.
3. Comparte el enlace de tu repositorio en la plataforma o espacio designado.
