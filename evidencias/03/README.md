# Bloque 03

## Resultado
- **Qué añadí al proyecto:** Función Lambda `createEvent` para peticiones POST `/events` con análisis sintáctico de JSON y validaciones de esquema/dominio en TypeScript.
- **Ruta o pantalla comprobada:** `http://localhost:3000/dev/events` (POST) y `http://localhost:3000/dev/events/EVT-NEW` (GET).
- **Estado y respuesta observados:**
  - POST válido: HTTP 201 Created con cuerpo que incluye `id: EVT-NEW` y los datos enviados.
  - JSON roto o payload incompleto: HTTP 400 Bad Request indicando el error de validación correspondiente.
  - GET `/events/EVT-NEW`: HTTP 200 OK con la ficha fija simulada (`Neon Waves`).

## Comprensión
- **Recorrido del dato, explicado con mis palabras:** La petición POST transporta el cuerpo serializado en texto dentro de `event.body`. Se procesa con `JSON.parse` dentro de un bloque `try/catch` tratando el resultado como `unknown`. Posteriormente se acota el tipo verificando que sea un objeto plano no nulo y validando la presencia, tipo y formato de los campos (`artist`, `space`, `time` vía RegExp `^([01]\d|2[0-3]):[0-5]\d$` y `status`). Si cumple las reglas, devuelve 201; de lo contrario, intercepta y retorna 400.
- **Cambio guiado y resultado:**
  - Status `CANCELLED`: Devuelve 201 Created al ser un estado permitido.
  - Status `PAUSED`: Devuelve 400 con mensaje `status debe ser CONFIRMED o CANCELLED`.
  - Time `29:80`: Devuelve 400 con mensaje `artist, space y time (HH:mm) son obligatorios`.
- **Error encontrado y cómo lo resolví:** Ninguno en ejecución; la tipificación estricta con `unknown` obligó a comprobar exhaustivamente la existencia de propiedades antes de acceder a ellas.

## Evidencias
- **Captura o transcripción breve, sin credenciales:**
  - Petición POST válida:
    `POST /dev/events -> 201 Created`
    `{"id":"EVT-NEW","artist":"Nova Pulse","space":"Main Stage","time":"21:00","status":"CONFIRMED"}`
  - Petición inválida:
    `POST /dev/events (time: 29:80) -> 400 Bad Request`
    `{"message":"artist, space y time (HH:mm) son obligatorios"}`
  - Log en terminal: `POST evento: Nova Pulse`
  POST /dev/events (λ: createEvent)
(λ: createEvent) RequestId: 5c6c94ce-58f8-4b41-a33b-b391bb731888  Duration: 115.08 ms  Billed Duration: 116 ms


POST /dev/events (λ: createEvent)
(λ: createEvent) RequestId: e10c689e-75ee-40bf-9f52-835e678c111a  Duration: 2.20 ms  Billed Duration: 3 ms


POST /dev/events (λ: createEvent)
(λ: createEvent) RequestId: b0ba8eac-3d55-4bfb-a568-e8579f1b4da1  Duration: 1.77 ms  Billed Duration: 2 ms


GET /dev/events/EVT-NEW (λ: getEvent)
GET evento: EVT-NEW
(λ: getEvent) RequestId: 93c2454d-627c-4add-8cb7-e432dd03a4db  Duration: 108.98 ms  Billed Duration: 109 ms


POST /dev/events (λ: createEvent)
(λ: createEvent) RequestId: 3689451d-4193-4776-a2ca-f6cd33b38ee7  Duration: 1.75 ms  Billed Duration: 2 ms


POST /dev/events (λ: createEvent)
(λ: createEvent) RequestId: b45a9a06-b9e6-4d30-a057-046b7809fc40  Duration: 1.87 ms  Billed Duration: 2 ms


POST /dev/events (λ: createEvent)
(λ: createEvent) RequestId: ddd75a4d-ec9a-4c13-ac7c-f102d7f5ac11  Duration: 1.75 ms  Billed Duration: 2 ms
- **Limitaciones del ejercicio:** Ausencia de base de datos o almacenamiento persistente; el ID generado `EVT-NEW` es estático y la información enviada por POST no es consultable posteriormente mediante GET.