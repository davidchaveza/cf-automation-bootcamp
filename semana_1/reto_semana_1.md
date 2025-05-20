
# 🚀 Reto Semana 1 - Automatización vs Manual + Lectura de Archivos

## 🎯 Objetivo
Aplicar tus conocimientos básicos de Python y reflexionar sobre el valor de la automatización de pruebas en situaciones reales.

---

## 📘 Parte 1: Reflexión escrita

Redacta un breve texto (máximo 200 palabras) donde expliques un caso real o hipotético en el que:

- La automatización de pruebas **agrega valor significativo** frente a realizar pruebas manuales.
- Puedes referirte a pruebas regresivas, ejecución repetitiva o precisión.

Guárdalo como un archivo `reflexion.md` en tu repositorio del bootcamp.

---

## 🐍 Parte 2: Script de lectura de archivos

1. Crea un archivo llamado `datos.json` o `datos.csv` con información de ejemplo. Por ejemplo:

```json
[
  {"nombre": "Carlos", "edad": 30},
  {"nombre": "Laura", "edad": 25}
]
```

2. Escribe un script llamado `leer_datos.py` que:

- Lea el archivo.
- Recorra el contenido.
- Imprima el nombre de cada persona.

Usa el módulo `json` si usas JSON o `csv` si usas CSV.

---

## 📸 Entrega

1. Subir ambos archivos (`reflexion.md` y `leer_datos.py`) a tu repositorio.
2. Verifica que puedan ejecutarse correctamente desde la terminal con:

```bash
python leer_datos.py
```

---

## ✅ Propuesta de solución

### Ejemplo de archivo `leer_datos.py` para JSON:

```python
import json

with open("datos.json", "r") as f:
    personas = json.load(f)

for persona in personas:
    print(f"Nombre: {persona['nombre']} - Edad: {persona['edad']}")
```

### Ejemplo de archivo `datos.json`:

```json
[
  {"nombre": "Carlos", "edad": 30},
  {"nombre": "Laura", "edad": 25}
]
```

---

¡Reto completado! Comparte tu repositorio cuando hayas terminado.
