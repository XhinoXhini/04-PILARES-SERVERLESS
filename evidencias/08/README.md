# Bloque 08

## Resultado
- **Qué añadí al proyecto:** Despliegue de la API Serverless en AWS CloudFormation utilizando el rol preexistente del laboratorio (`LabRole`), variables de entorno para región y alumno, y aprovisionamiento de API Gateway junto a las 7 funciones Lambda en Node.js 22.
- **Ruta o pantalla comprobada:** Endpoint publicado en API Gateway: `https://xqzjsqsyi0.execute-api.us-east-1.amazonaws.com/dev/events`.
- **Estado y respuesta observados:** HTTP 200 OK devolviendo las dos actuaciones del catálogo (`EVT-101` y `EVT-102`) y registro de traza confirmado en CloudWatch (`LIST eventos: GET`).

## Comprensión
- **Recorrido del dato, explicado con mis palabras:** La petición viaja por Internet mediante HTTPS al dominio de API Gateway `xqzjsqsyi0.execute-api.us-east-1.amazonaws.com`. El servicio enruta el método GET hacia la Lambda `listEvents`, la cual se ejecuta en AWS bajo los permisos de `LabRole`. La salida de consola se redirige automáticamente al grupo de logs de CloudWatch y la respuesta HTTP regresa al cliente con las cabeceras CORS.
- **Cambio guiado y resultado:** Se invocó el endpoint publicado con `curl.exe` y se verificó la sincronización de la ejecución remota inspeccionando los logs de CloudWatch con `npx serverless logs -f listEvents`.
- **Error encontrado y cómo lo resolví:** Error de perfil no encontrado al configurar credenciales y variable de región no detectada por el shell; se solucionó configurando el perfil `academy` mediante `aws configure set` y asegurando valores de respaldo en `serverless.yml`.

## Evidencias
- **Captura o transcripción breve, sin credenciales:**
  - Petición: `GET https://xqzjsqsyi0.execute-api.us-east-1.amazonaws.com/dev/events -> 200 OK`
  - CloudWatch (`listEvents`): `LIST eventos: GET`
- **Limitaciones del ejercicio:** Las funciones Lambda corren en la nube de AWS de forma stateless sobre datos estáticos en memoria; no se dispone de persistencia en DynamoDB ni base de datos externa.