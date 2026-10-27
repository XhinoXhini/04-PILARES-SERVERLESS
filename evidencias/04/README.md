# Bloque 04

## Resultado
- **Qué añadí al proyecto:** Arquitectura modular de handlers en `src/events/` implementando contrato CRUD completo con CORS (`list.ts`, `get.ts`, `create.ts`, `replace.ts`, `update.ts`, `delete.ts` y `model.ts`).
- **Ruta o pantalla comprobada:** `/events` y `/events/{id}` bajo métodos GET, POST, PUT, PATCH, DELETE y OPTIONS.
- **Estado y respuesta observados:**
  - GET `/events`: 200 OK con 2 eventos iniciales.
  - GET `/events/EVT-101`: 200 OK con detalle del evento.
  - GET `/events/NO-EXISTE`: 404 Not Found.
  - POST `/events`: 201 Created con `id: EVT-NEW`.
  - PUT `/events/EVT-101`: 200 OK con `operation: REPLACED`.
  - PATCH `/events/EVT-101`: 200 OK con `operation: UPDATED`.
  - DELETE `/events/EVT-102`: 200 OK con `operation: DELETED`.
  - OPTIONS `/events`: 200 OK con cabeceras CORS.

## Comprensión
- **Recorrido del dato, explicado con mis palabras:** Cada método HTTP delega en su handler correspondiente. El helper común `json` en `model.ts` normaliza los códigos de estado y adjunta las cabeceras `Access-Control-Allow-Origin: *`. PUT requiere la totalidad del objeto (`isEventInput`), mientras que PATCH aísla únicamente el campo `status`. Al no existir persistencia en base de datos, el catálogo en memoria permanece inmutable frente a mutaciones.
- **Cambio guiado y resultado:**
  - PUT parcial arrojó 400 Bad Request por omisión de campos obligatorios.
  - PATCH con solo `{ "status": "CANCELLED" }` procesó con 200 OK.
  - La consulta posterior confirmó que el catálogo mantiene los valores base iniciales.
- **Error encontrado y cómo lo resolví:** Timeout al invocar el CLI de Serverless v4 por verificación de telemetría externa; se resolvió compilando con `npm run build` y ejecutando offline directamente vía `npx serverless offline`.

## Evidencias
- **Captura o transcripción breve, sin credenciales:**
  - `GET /dev/events/EVT-101` -> 200 OK: `{"id":"EVT-101","artist":"Neon Waves",...}`
  - `GET /dev/events/NO-EXISTE` -> 404 Not Found: `{"message":"Evento no encontrado"}`
  - `OPTIONS /dev/events` -> 200 OK con `access-control-allow-origin: *`
  GET /dev/events (λ: listEvents)
LIST eventos: GET
(λ: listEvents) RequestId: a9bc1559-d72f-4828-b34b-4d08a0999172  Duration: 117.38 ms  Billed Duration: 118 ms


GET /dev/events/EVT-101 (λ: getEvent)
GET evento: EVT-101
(λ: getEvent) RequestId: 330201dc-cc40-478a-acbe-32b7dabe80b2  Duration: 109.98 ms  Billed Duration: 110 ms


GET /dev/events/NO-EXISTE (λ: getEvent)
GET evento: NO-EXISTE
(λ: getEvent) RequestId: cd261c4f-f879-400e-a72d-9ce92f133cb5  Duration: 2.21 ms  Billed Duration: 3 ms


POST /dev/events (λ: createEvent)
(λ: createEvent) RequestId: 3deb9bdd-bb83-4da9-90c6-b546751f58dd  Duration: 111.25 ms  Billed Duration: 112 ms


PUT /dev/events/EVT-101 (λ: replaceEvent)
(λ: replaceEvent) RequestId: e8d8c87a-fb07-4788-b7ba-f5ed93215166  Duration: 109.52 ms  Billed Duration: 110 ms


PATCH /dev/events/EVT-101 (λ: updateEvent)
PATCH evento: EVT-101
(λ: updateEvent) RequestId: cda7bc93-3b65-41ee-8a26-38d94483e403  Duration: 110.60 ms  Billed Duration: 111 ms


DELETE /dev/events/EVT-102 (λ: deleteEvent)
DELETE evento: EVT-102
(λ: deleteEvent) RequestId: 3806d38d-aa0f-428e-9c0f-a71c888a9454  Duration: 107.66 ms  Billed Duration: 108 ms


PUT /dev/events/EVT-101 (λ: replaceEvent)
(λ: replaceEvent) RequestId: 26099357-5fcd-4906-b349-b73c95f1de63  Duration: 2.27 ms  Billed Duration: 3 ms


PATCH /dev/events/EVT-101 (λ: updateEvent)
(λ: updateEvent) RequestId: de6e8ef3-cf1e-40fb-b97e-9509d182cf85  Duration: 2.53 ms  Billed Duration: 3 ms


GET /dev/events (λ: listEvents)
LIST eventos: GET
(λ: listEvents) RequestId: 3150cf5f-cfb5-4c41-88ef-312ad79e40c6  Duration: 2.53 ms  Billed Duration: 3 ms
- **Limitaciones del ejercicio:** Operaciones simuladas en memoria; las llamadas de escritura no persisten cambios en disco ni base de datos.