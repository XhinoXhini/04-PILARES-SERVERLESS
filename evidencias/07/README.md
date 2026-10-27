# Bloque 07

## Resultado
- **Qué añadí al proyecto:** Funciones de cliente HTTP (`createEvent`, `replaceEvent`, `updateEvent`, `deleteEvent`) en `mobile/src/api.ts` y botones interactivos en `mobile/App.tsx` con control de concurrencia/bloqueo (`busy`) y visor del resultado JSON retornado.
- **Ruta o pantalla comprobada:** Pantalla de Programación en `http://localhost:8081` ejecutando mutaciones HTTP contra `http://localhost:3000/dev/events`.
- **Estado y respuesta observados:**
  - POST crear: 201 Created retornando `id: EVT-NEW`.
  - PUT reemplazar EVT-101: 200 OK retornando `operation: REPLACED`.
  - PATCH cancelar EVT-101: 200 OK retornando `operation: UPDATED` y `changes: { status: "CANCELLED" }`.
  - DELETE EVT-102: 200 OK retornando `operation: DELETED`.
  - GET recargar tras escrituras: 200 OK manteniendo los dos elementos intactos.

## Comprensión
- **Recorrido del dato, explicado con mis palabras:** Al pulsar cualquiera de los botones de acción, se invoca la función asíncrona `run(label, task)`, la cual activa `busy = true` para deshabilitar las acciones y evitar peticiones concurrentes. La función correspondiente en `api.ts` envía la petición con su método HTTP específico (`POST`, `PUT`, `PATCH`, `DELETE`), serializando los datos si corresponde (`JSON.stringify`). Al recibir el objeto de respuesta, `run` lo formatea con indentación y lo inyecta en el estado `result` para que se imprima en pantalla en texto monoespaciado.
- **Cambio guiado y resultado:**
  - Se modificó temporalmente el valor de PATCH a `CONFIRMED`.
  - El backend aceptó el valor y respondió con HTTP 200 y `"changes": { "status": "CONFIRMED" }`.
  - Se restauró el valor `CANCELLED` para preservar el contrato de la práctica.
- **Error encontrado y cómo lo resolví:** Ninguno durante la integración; el estado `busy` garantizó que no se enviaran múltiples peticiones simultáneas mientras una Promise estaba pendiente.

## Evidencias
- **Captura o transcripción breve, sin credenciales:**
  - Petición PATCH: `PATCH /dev/events/EVT-101` -> `200 OK`
  ```json
  {
    "id": "EVT-101",
    "changes": {
      "status": "CANCELLED"
    },
    "operation": "UPDATED"
  }