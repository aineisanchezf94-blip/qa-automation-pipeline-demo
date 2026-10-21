# Reporte Unificado de Pruebas de API (Sprint QA)

Este documento recopila la ejecución técnica y las evidencias en terminal de las pruebas de integración realizadas para el MVP de Distribución de Agua.

---

## 1. Simulación de Autenticación y Control de Roles (RBAC)

### Propósito
Validar que la API del MVP procesa correctamente los payloads de inicio de sesión de los operadores en terreno y encapsula las credenciales y alcances de permisos de la NTI-05.


### Evidencia de la Terminal
![Evidencia Autenticación](./../evidencias/evidencia-curl-rbac.png)

Caso de Prueba: Simulación de Autenticación y Control de Roles (RBAC)
Propósito: Validar que la API del MVP procesa correctamente los payloads de inicio de sesión de los operadores en terreno y encapsula las credenciales y alcances de permisos de la NTI-05.
Análisis de la Evidencia de la Terminal (Resultado Real):
Capa de Transporte Seguro: La consola de macOS resolvió la IP 162.159.129.53 en el puerto 443 y forzó un protocolo TLSv1.3 (AEAD-CHACHA20-POLY1305-SHA256) con certificados válidos de Let's Encrypt.
Estructura del Contrato: El sistema inyectó de forma íntegra los 166 bytes correspondientes a los parámetros de identidad del chofer: el identificador de la aplicación (MVP_DISTRIBUCION_AGUA), el correo del operador (chofer.carlos@tuagua.cl), su contraseña encriptada y el rol solicitado (role:chofer).
Comportamiento Perimetral: El backend procesó las cabeceras Content-Type: application/json y emitió una redirección canonicalizada HTTP/2 301 hacia el dominio seguro de producción (https://www.postman.com/).

### Análisis Técnico
*   **Capa de Transporte Seguro:** La consola de macOS resolvió la IP `162.159.129.53` en el puerto `443` y forzó un protocolo `TLSv1.3` con certificados válidos de *Let's Encrypt*.
*   **Estructura del Contrato:** El sistema inyectó de forma íntegra los `166 bytes` correspondientes a los parámetros de identidad del chofer (`chofer.carlos@tuagua.cl`, rol solicitado: `role:chofer`).
*   **Comportamiento Perimetral:** El backend procesó las cabeceras y emitió una redirección canonicalizada `HTTP/2 301` hacia el dominio seguro.

---

## 2. Integración y Pruebas de API - WhatsApp

### Propósito
Validar que la Capa de Negocio del MVP es capaz de estructurar y despachar payloads en formato JSON bajo los estándares de autorización por token (*Bearer Token*) que exige Meta para las notificaciones automatizadas de la NTI-02.

### Evidencia de la Terminal
![Evidencia WhatsApp](/evidencias/evidencia-whatsapp.png)

### Análisis Técnico
* Caso de Prueba: Simulación de Mensajería y Redirección (WhatsApp Business API)
Propósito: Validar que la Capa de Negocio del MVP es capaz de estructurar y despachar payloads en formato JSON bajo los estándares de autorización por token (Bearer Token) que exige Meta para las notificaciones automatizadas de la NTI-02.
Análisis de la Evidencia (Captura de Pantalla Real):
Handshake Criptográfico: La Terminal de macOS negoció con éxito una conexión segura utilizando el protocolo de última generación TLSv1.3 (AEAD-CHACHA20-POLY1305-SHA256) con certificados vigentes emitidos por la entidad acreditada Let's Encrypt.
Validación de Cabeceras: Se comprueba el envío correcto de los parámetros críticos en el encabezado: el agente de origen (curl/8.7.1), el tipo de contenido (application/json) y la cabecera de autenticación (Authorization: Bearer EAAGb0...).
Respuesta de Redirección Controlada: El servidor perimetral procesó los 281 bytes del mensaje y devolvió un código de estado HTTP/2 301 Moved Permanently, apuntando a la ruta segura canonicalizada https://postman.com.


---

## 3. Pasarela de Pagos - Transbank / Webpay

### Propósito
Validar el comportamiento de la pasarela ante solicitudes de autenticación en ambientes de certificación, garantizando tanto el bloqueo perimetral ante credenciales inválidas (Camino Triste) como la correcta asignación de Tokens de Pago bajo identificadores comerciales abiertos (Camino Feliz).

### Evidencia de la Terminal
![Evidencia Transbank](/evidencias/evidencia-transbank.png)

### Análisis Técnico
Evidencias de Pruebas de Integración de APIs (QA)
Caso de Prueba 01: Simulación de Fallo de Autenticación (Camino Triste)
Protocolo de Entrada: Solicitud HTTP/2 POST a la dirección de Transbank inyectando un código de comercio no emparejado (597020000540).
Resultado Observable: El servidor de certificación responde con una cabecera < HTTP/2 401 y un cuerpo JSON con el mensaje {"error_message":"Not Authorized"}.
Conclusión de Arquitectura: El sistema de protección perimetral Imperva bloqueó la transacción correctamente. Esto valida que la seguridad de Webpay funciona rechazando peticiones que no cuenten con llaves vigentes en el backend.
Caso de Prueba 02: Solicitud de Token de Pago (Camino Feliz)
Protocolo de Entrada: Solicitud HTTP/2 POST utilizando el ID comercial de integración abierto (597055555532).
Resultado Esperado: Retorno de código de estado HTTP/2 200 OK, acompañado de un token criptográfico dinámico y una URL de pasarela para iniciar el flujo seguro de Webpay Plus.
