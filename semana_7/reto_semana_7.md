# 🚀 Reto Semana 7 - Flujo de Compra BDD y Hooks

## 🎯 Objetivo
Ampliar el conjunto de pruebas BDD para describir todo un flujo de negocio (compra en Demoblaze) en Gherkin. Implementar pasos clave de al menos un escenario complejo, usar hooks para setup/teardown y documentar su integración al proyecto final.

---

## 🔧 Instrucciones

1. **Crea** el archivo de característica `features/purchase_flow.feature`:

```gherkin
Feature: Flujo de compra en Demoblaze

  Como usuario registrado
  Quiero agregar un producto al carrito y verificar mi orden
  Para simular un flujo de compra

  Background:
    Given el usuario está en la página de inicio de Demoblaze

  Scenario: Agregar producto al carrito y verificar
    When selecciona la categoría "Laptops"
    And elige el producto "Sony vaio i5"
    And agrega el producto al carrito
    Then debería ver "Sony vaio i5" en el carrito

  Scenario: Intentar agregar producto inexistente
    When selecciona la categoría "Laptops"
    Then debería ver mensaje "Product not found"
```

2. **Crear** POM para Demoblaze (archivos en `pages/`).

3. **Implementar** los step definitions en `features/steps/purchase_steps.py`.

4. **Hooks** en `environment.py` para setup/teardown de cada escenario.

5. **Ejecuta**:
   ```bash
   behave
   ```

---

## 📄 Integración y valor para stakeholders

- Los `.feature` funcionan como documentación viva legible para negocio.  
- Facilita la colaboración QA-negocio.

---

**¡Felicidades!** Has implementado un flujo de compra BDD con POM y Behave.