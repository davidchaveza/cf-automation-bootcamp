# 🚀 Reto Semana 8 - Reportes Detallados en el Pipeline de CI

## 🎯 Objetivo
Mejorar el pipeline de CI incorporando un paso de generación y publicación de reportes de resultados, ya sea con **pytest-html** o **Allure Report**, y documentar en el repositorio las configuraciones de CI realizadas.

---

## 🔧 Instrucciones

1. **Extender el workflow**  
   - Abre `.github/workflows/ci.yml` y añade después de ejecutar pytest un paso para subir el reporte HTML generado:

   ```yaml
   - name: Upload Test Report
     if: always()
     uses: actions/upload-artifact@v3
     with:
       name: pytest-html-report
       path: report.html
   ```

2. **(Opcional) Integrar Allure Report**  
   - Instala `allure-pytest` y agrega la generación de reporte:
     ```yaml
     - name: Install Allure
       run: |
         pip install allure-pytest

     - name: Run tests with Allure
       run: |
         pytest --alluredir=allure-results

     - name: Generate Allure report
       run: |
         pip install allure-commandline
         allure generate allure-results --clean -o allure-report

     - name: Upload Allure Report
       if: always()
       uses: actions/upload-artifact@v3
       with:
         name: allure-report
         path: allure-report
     ```

3. **Commit y Push**  
   ```bash
   git add .github/workflows/ci.yml
   git commit -m "Add HTML and Allure report upload to CI pipeline"
   git push origin main
   ```

4. **Verificar artefactos en GitHub Actions**  
   - Tras la ejecución, en la sección **Artifacts** del run, descarga **pytest-html-report** (y **allure-report** si se configuró).  
   - Asegúrate de que los reportes están completos y accesibles.

5. **Documentar**  
   - Actualiza el `README.md` indicando:
     - Cómo y dónde se generan los reportes (`report.html`, `allure-report`).  
     - Comandos de pytest local para generar los reportes.  
     - Visualización y descarga de artefactos desde GitHub Actions.