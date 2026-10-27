# Bloque 02

## Resultado
- **Qué añadí al proyecto:** Función Lambda `getEvent` tipada con TypeScript y vinculada en `serverless.yml` al path dinámico `events/{id}` con método GET y soporte CORS.
- **Ruta o pantalla comprobada:** `http://localhost:3000/dev/events/EVT-104` y `http://localhost:3000/dev/events/EVT-330`.
- **Estado y respuesta observados:** HTTP 200 OK devolviendo el objeto JSON con el `id` correspondiente recibido en la URL (`EVT-104`, `EVT-330`) y los datos estáticos del evento.

## Comprensión
- **Recorrido del dato, explicado con mis palabras:** El cliente solicita la URL indicando el parámetro en la ruta. `serverless-offline` parsea el patrón `events/{id}` y rellena `event.pathParameters.id`. El handler extrae ese valor de forma segura mediante optional chaining (`event.pathParameters?.id`), comprueba que exista (devolviendo 400 si falta) y lo inyecta en el JSON serializado de respuesta.
- **Cambio guiado y resultado:** Se probó `/events/EVT-330` sin tocar el código fuente; el handler capturó e inyectó dinámicamente `"id": "EVT-330"` en la respuesta y en el log `console.log('GET evento:', id)`.
- **Error encontrado y cómo lo resolví:** Ninguno relevante; la verificación con `npm run typecheck` confirmó tipos válidos antes de levantar Offline.

## Evidencias
- **Captura o transcripción breve, sin credenciales:**
  Petición:
  `GET http://localhost:3000/dev/events/EVT-104 -> 200 OK`
  `{"id":"EVT-104","artist":"Neon Waves","space":"Main Stage","time":"22:30","status":"CONFIRMED"}`
  Log en terminal backend:
  GET /dev/events/EVT-104 (λ: getEvent)
GET evento: EVT-104
(λ: getEvent) RequestId: e75f04ff-004f-4ac7-9456-439db44dcb06  Duration: 945.84 ms  Billed Duration: 946 ms


GET /dev/events/EVT-205 (λ: getEvent)
GET evento: EVT-205
(λ: getEvent) RequestId: cb12a796-9629-4d79-9171-5c8d9f02b2dc  Duration: 2.68 ms  Billed Duration: 3 ms


GET /dev/events/EVT-330 (λ: getEvent)
GET evento: EVT-330
(λ: getEvent) RequestId: da4e4e5c-3fb6-4b25-85c2-fa02e9bc3a79  Duration: 2.24 ms  Billed Duration: 3 ms
- **Limitaciones del ejercicio:** Los datos son estáticos (mock); cualquier identificador devuelve el mismo artista y sala ya que todavía no hay persistencia ni base de datos conectada.