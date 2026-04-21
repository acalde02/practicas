
# Unidad 6 — Documentación de pruebas (Claude Desktop + MCP Gmail)

Fecha: 21/04/2026

## Objetivo

Validar un servidor MCP que expone operaciones básicas de Gmail para poder:

- Listar emails recientes.
- Consultar el perfil de Gmail.
- Enviar un email.
- Redactar un email (borrador) y, opcionalmente, enviarlo.

## Implementación (servidor MCP)

Servidor: [practicas/unidad6/gmail-mcp-server/gmail_mcp_server.py](practicas/unidad6/gmail-mcp-server/gmail_mcp_server.py)

### Dependencias

Definidas en: [practicas/unidad6/gmail-mcp-server/requirements.txt](practicas/unidad6/gmail-mcp-server/requirements.txt)

- `fastmcp`
- `google-auth`
- `google-auth-oauthlib`
- `google-api-python-client`

### Autenticación

El servidor gestiona OAuth 2.0 de Google:

- Archivo de credenciales: `credentials.json`
- Token persistido: `token.json`

Ambos están en la carpeta del servidor:

- [practicas/unidad6/gmail-mcp-server/credentials.json](practicas/unidad6/gmail-mcp-server/credentials.json)
- [practicas/unidad6/gmail-mcp-server/token.json](practicas/unidad6/gmail-mcp-server/token.json)

Scopes solicitados (según el servidor):

- `https://www.googleapis.com/auth/gmail.readonly`
- `https://www.googleapis.com/auth/gmail.send`

### Herramientas y recursos expuestos

Tools:

- `list_emails(max_results=10, query="")` → lista emails con `id`, `from`, `subject`, `snippet`.
- `send_email(to, subject, body)` → envía email y devuelve estado e `id`.

Resource:

- `gmail://profile` (función `get_profile`) → devuelve `email`, `total_messages`, `total_threads`.

Prompt (plantilla):

- `redactar_email(destinatario, asunto)` → genera un cuerpo de email profesional y sugiere enviar con `send_email`.

## Pruebas realizadas en Claude Desktop (con evidencias)

Carpeta de capturas: [practicas/unidad6/screenshots](practicas/unidad6/screenshots)

> Nota: algunas capturas no muestran el prompt inicial completo. En esos casos, se documenta exactamente lo visible y se marca lo no visible.

### Prueba 1 — Listar los últimos emails

Evidencia: [practicas/unidad6/screenshots/Screenshot 2026-04-21 at 5.34.25 PM.png](practicas/unidad6/screenshots/Screenshot%202026-04-21%20at%205.34.25%E2%80%AFPM.png)

1) Pregunta/instrucción escrita en Claude Desktop

- No visible en la captura.

2) Respuesta de Claude (herramienta/recurso utilizado)

- Claude ejecuta la tool `list_emails`.
- Responde con: “Aquí están tus últimos 5 emails: …”

3) Resultado final

- Se listan 5 emails con asunto/remitente y un fragmento. En la captura aparecen, entre otros:
	- “Parking Place — 614 | Greystar”
	- “Beauty recommendations … — 204 | Greystar”
	- “Welcome to Overleaf — Overleaf”
	- “Get started with your new Analytics account — Google Analytics”
	- “PAELLA ALERT! — Nera Living Atocha | Greystar”

### Prueba 2 — Consultar perfil de Gmail

Evidencia: [practicas/unidad6/screenshots/Screenshot 2026-04-21 at 5.34.39 PM.png](practicas/unidad6/screenshots/Screenshot%202026-04-21%20at%205.34.39%E2%80%AFPM.png)

1) Pregunta/instrucción escrita en Claude Desktop

- “¿Cuál es mi perfil de Gmail?”

2) Respuesta de Claude (herramienta/recurso utilizado)

- En la captura no aparece explícitamente la etiqueta del recurso/herramienta utilizado.
- Por implementación del servidor, esta consulta corresponde al resource `gmail://profile` (función `get_profile`).
- Claude responde con: “Tu perfil de Gmail es: …”

3) Resultado final

- En la captura se muestra el email de la cuenta: `adriancalderondeamat@gmail.com`.
- (Los contadores `total_messages` y `total_threads` existen en el recurso, pero no se ven reflejados en esta captura.)

### Prueba 3 — Enviar un email de prueba

Evidencia: [practicas/unidad6/screenshots/Screenshot 2026-04-21 at 5.34.49 PM.png](practicas/unidad6/screenshots/Screenshot%202026-04-21%20at%205.34.49%E2%80%AFPM.png)

1) Pregunta/instrucción escrita en Claude Desktop

- “Envía un email de prueba a adrian.calderon.de.amat@hotmail.com con asunto \"Test MCP\" y cuerpo \"Este es un email enviado desde mi servidor MCP\"”

2) Respuesta de Claude (herramienta/recurso utilizado)

- Claude ejecuta la tool `send_email`.
- Claude responde: “¡Email enviado con éxito!”

3) Resultado final

- Confirmación del envío, con:
	- Para: `adrian.calderon.de.amat@hotmail.com`
	- Asunto: `Test MCP`
	- Cuerpo: “Este es un email enviado desde mi servidor MCP”

### Prueba 4 — Redactar un email (borrador) para una reunión

Evidencia: [practicas/unidad6/screenshots/Screenshot 2026-04-21 at 5.35.01 PM.png](practicas/unidad6/screenshots/Screenshot%202026-04-21%20at%205.35.01%E2%80%AFPM.png)

1) Pregunta/instrucción escrita en Claude Desktop

- No visible en la captura.
- Sí se ven las aclaraciones usadas para construir el borrador:
	- “¿Cuál es el propósito principal de la reunión del lunes?” → “Revisión de proyecto”
	- “¿Qué tono prefieres para el email?” → “Formal (Recommended)”

2) Respuesta de Claude (herramienta/recurso utilizado)

- No se observa ejecución de tools MCP en la captura (no aparece `send_email` ni `list_emails`).
- Claude entrega el texto del email redactado y pide confirmación para enviarlo o ajustar detalles.

3) Resultado final

- Borrador generado con:
	- Para: Juan García
	- Asunto: “Reunión de revisión de proyecto — Lunes 27 de abril”
	- Cuerpo: email formal confirmando reunión, objetivos y próximos pasos.
- No hay confirmación de envío en la captura (queda pendiente de confirmación del usuario).

## Conclusión

Las evidencias muestran que:

- La integración puede invocar tools MCP para leer (listar emails) y actuar (enviar emails).
- El recurso de perfil (`gmail://profile`) permite recuperar información de la cuenta.
- La redacción de emails puede hacerse como borrador antes del envío, y el envío queda como acción explícita mediante `send_email`.

