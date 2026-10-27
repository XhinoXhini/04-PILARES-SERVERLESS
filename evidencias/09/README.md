# Bloque 09

## Resultado
- **Qué añadí al proyecto:** Conexión del cliente React Native con la infraestructura cloud de AWS sustituyendo `API_URL` por el endpoint público de API Gateway (`https://xqzjsqsyi0.execute-api.us-east-1.amazonaws.com/dev`), ejecutando la aplicación con el entorno local offline completamente apagado.
- **Ruta o pantalla comprobada:** Interfaz web en `http://localhost:8081` realizando llamadas HTTPS directas a API Gateway y Lambdas en `us-east-1`.
- **Estado y respuesta observados:**
  - GET `/events`: 200 OK renderizando las tarjetas remotas.
  - POST `/events`: 201 Created retornando `id: EVT-NEW`.
  - PUT `/events/EVT-101`: 200 OK retornando `operation: REPLACED`.
  - PATCH `/events/EVT-101`: 200 OK retornando `operation: UPDATED`.
  - DELETE `/events/EVT-102`: 200 OK retornando `operation: DELETED`.
  - CloudWatch Logs: Trazas confirmadas en las Lambdas `updateEvent` y `listEvents`.

## Comprensión
- **Recorrido del dato, explicado con mis palabras:** Las acciones de la interfaz envían peticiones HTTPS al endpoint de API Gateway en AWS. API Gateway resuelve el enrutamiento y dispara la ejecución en frío o caliente de la función Lambda correspondiente. La respuesta HTTP incluye las cabeceras CORS configuradas (`Access-Control-Allow-Origin: *`), lo que permite al navegador web recibir y procesar los payloads sin bloqueos de seguridad entre orígenes.
- **Cambio guiado y resultado:** Se validó la ejecución en la nube comprobando que todas las peticiones inspeccionadas en la pestaña Network apuntaban al dominio `xqzjsqsyi0.execute-api.us-east-1.amazonaws.com` y contrastando que las invocaciones de `PATCH` y `GET` generaron las entradas correspondientes en CloudWatch Logs.
- **Error encontrado y cómo lo resolví:** Ninguno; mantener el contrato tipado intacto entre frontend y backend permitió cambiar de entorno local a cloud modificando exclusivamente la variable `API_URL`.

## Evidencias
- **Captura o transcripción breve, sin credenciales:**
  - Pestaña Network:
    - `Request URL: https://xqzjsqsyi0.execute-api.us-east-1.amazonaws.com/dev/events/EVT-101`
    - `Request Method: PATCH`
    - `Status Code: 200 OK`
  - CloudWatch (`updateEvent`): `PATCH evento: EVT-101`
  - CloudWatch (`listEvents`): `LIST eventos: GET`
- **Limitaciones del ejercicio:** Las funciones Lambda operan de manera efímera e inmutable en memoria; no se cuenta con una base de datos persistente (como DynamoDB) que almacene los cambios de estado entre ejecuciones.