# Pipeline de CI/CD para Automatización de QA & DevOps

Este repositorio contiene el diseño e implementación de un **Pipeline de Integración y Despliegue Continuo (CI/CD)** desarrollado para un proyecto integrado de un MVP de Distribución de Agua. El objetivo principal de este flujo es garantizar la calidad del software (QA) de forma automatizada antes de que el código sea desplegado en el entorno de pruebas.

*Nota: Para la aceleración y optimización de la sintaxis en formato YAML, este proyecto utilizó Inteligencia Artificial (IA) como copiloto de desarrollo, manteniendo la lógica, diseño de arquitectura y control de entornos bajo mi criterio de ingeniería.*

---

## Tecnologías y Herramientas Utilizadas

*   **CI/CD Engine:** GitHub Actions
*   **Formatos:** YAML
*   **Entorno de Ejecución:** Node.js (v20) en contenedores Linux (Ubuntu)
*   **Control de Calidad (QA):** Linters de sintaxis estricta (TypeScript/Vue) y suites de pruebas unitarias (`npm run test:unit`)
*   **Integraciones Simuladas (Mocks):** Sandbox de Webpay (Transbank) y Meta WhatsApp API (Variables de Entorno)

---

## Estructura y Funcionamiento del Pipeline

El flujo de trabajo (`workflow`) está dividido estratégicamente en dos etapas consecutivas para optimizar los recursos y asegurar la estabilidad del software:

### 1. Etapa de Inspección y Calidad (`build-and-test`)
Se activa automáticamente con cada `push` o `pull_request` en las ramas `main` y `testing`. El robot realiza las siguientes acciones:
*   Descarga el código fuente de forma segura.
*   Configura el entorno de desarrollo con la versión correcta de Node.js.
*   Instala las dependencias del proyecto de manera aislada en el directorio del frontend.
*   Ejecuta herramientas de análisis estático (*linter*) para validar que el código de TypeScript y Vue no contenga errores de sintaxis.
*   Corre la suite de pruebas unitarias para certificar el correcto funcionamiento de la lógica de la aplicación.

### 2. Etapa de Despliegue Controlado (`deploy-to-staging`)
Posee una condicional de seguridad (`needs: build-and-test`). Solo se ejecuta si la etapa anterior fue exitosa **Y** si los cambios van dirigidos específicamente a la rama de pruebas (`testing`). En este paso:
*   Se aíslan las credenciales mediante variables de entorno simuladas (Mocks) para la pasarela de pagos Webpay y los tokens de mensajería de WhatsApp, protegiendo los datos reales del negocio.
*   Se simula el despliegue del MVP en el servidor de Staging (Pruebas).

---

## Impacto en mi Perfil como QA

Este proyecto demuestra mi capacidad para:
1.  **Automatizar procesos de calidad:** Evitando que código con errores de sintaxis o pruebas fallidas llegue al servidor de pruebas.
2.  **Gestionar Entornos (Staging vs. Producción):** Comprendiendo las metodologías ágiles de despliegue por ramas.
3.  **Manejar arquitecturas de software modernas:** Conectando flujos con APIs críticas del mercado chileno (Transbank).

---
## 📂 Casos de Prueba Documentados
*   [Reporte Unificado de Pruebas de API (Sprint QA)](tests/reporte-pruebas-apis.md) — Documentación técnica completa con capturas de pantalla de terminal y análisis de endpoints (Autenticación, WhatsApp y Webpay).

   
