# Ecosistema de Automatización IA para Negocios

## Descripción

Proyecto final de automatización inteligente para la gestión de solicitudes de clientes.

El sistema recibe automáticamente correos electrónicos de clientes mediante Gmail, registra la información en Notion, utiliza inteligencia artificial para analizar y clasificar la solicitud y generar una respuesta, y solicita aprobación humana antes de enviar una respuesta al cliente.

## Tecnologías utilizadas

* **n8n:** orquestación y automatización del flujo.
* **Gmail:** recepción y envío de correos.
* **Notion:** almacenamiento y seguimiento de las solicitudes: https://app.notion.com/p/3ed91322724e8072b550e350b1660e54?v=3ed91322724e80f7971b000c229372e2&source=copy_link
* **Gemini 2.5:** análisis, clasificación, priorización y generación de respuestas mediante IA.

## Arquitectura

El flujo principal del sistema es:


Gmail Trigger
      ↓
IF Anti-Bucle
      ↓
Validación de datos
      ↓
Notion — Crear solicitud
      ↓
Gemini 2.5 — Analizar solicitud
      ↓
Notion — Actualizar información
      ↓
Aprobación humana (HITL)
      ↓
   ┌───────────────┐
   │               │
Aprobado        Rechazado
   ↓               ↓
 Gmail          Notion
   ↓           Rechazado
Notion
Enviado


El diagrama completo de arquitectura se encuentra en el archivo de git hub

## Funcionamiento
### 1. Recepción

Gmail Trigger detecta los correos recibidos y obtiene información como:

* Remitente
* Asunto
* Mensaje

### 2. Control anti-bucle

Antes de continuar, el sistema verifica que el correo no haya sido generado por el propio sistema.

Los mensajes que contienen la etiqueta `SENT` son detenidos para evitar que una respuesta automática vuelva a iniciar el flujo.

### 3. Registro en Notion

La solicitud se registra en Notion con estado inicial:

`Pendiente`

La base de datos almacena la información necesaria para realizar el seguimiento de cada solicitud.

### 4. Procesamiento mediante IA

Gemini 2.5 analiza el contenido del correo y devuelve información estructurada:

* Categoría
* Prioridad
* Respuesta propuesta

Las categorías utilizadas son:

* Consulta
* Reclamo
* Soporte
* Otro

Las prioridades utilizadas son:

* Alta
* Media
* Baja

### 5. Actualización del registro

Los resultados obtenidos mediante IA se almacenan nuevamente en Notion.

El estado de la solicitud pasa a:

Procesado por IA

Luego pasa a:

Esperando aprobación

### 6. Human-in-the-Loop

Antes de enviar una respuesta al cliente, se requiere una aprobación
