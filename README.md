# Festival Control · Bloque 01

## Qué he construido
Se ha desplegado localmente una función Lambda (`hello`) expuesta mediante un evento HTTP GET en la ruta `/dev/hello` utilizando el plugin `serverless-offline`.

## Qué he comprendido
- **Tipos del handler:** Se importan `APIGatewayProxyEvent` y `APIGatewayProxyResult` desde `aws-lambda` para garantizar que la función reciba la carga de API Gateway tipada y retorne obligatoriamente un objeto con `statusCode` y `body` serializado.
- **Compilación:** TypeScript compila el código fuente de `src/hello.ts` a JavaScript compatible en `dist/hello.js`, que es el archivo que Serverless Framework ejecuta a través de su propiedad `handler: dist/hello.handler`.
- **Ubicación de logs:** La traza `EVENT RECIBIDO:` se imprime directamente en la salida estándar de la consola donde se ejecuta `npm start` (en AWS real se enviaría a CloudWatch Logs).

## Qué he modificado
Se mantuvo la estructura original del mensaje JSON en `src/hello.ts`. La URL no cambia al modificar el payload devuelto porque el endpoint depende exclusivamente de la propiedad `events.http.path` definida en `serverless.yml`.

## Cómo lo he comprobado
- Petición ejecutada vía terminal contra `http://localhost:3000/dev/hello`.
- Respuesta obtenida: Código 200 OK con payload `{"message":"Festival Control · equipo preparado","service":"hello"}`.
- Evidencia en consola: Log `EVENT RECIBIDO:` GET /dev/hello (λ: hello)
EVENT RECIBIDO: {
  body: null,
  headers: {
    'User-Agent': 'Mozilla/5.0 (Windows NT; Windows NT 10.0; es-ES) WindowsPowerShell/5.1.26100.9711',
    Host: 'localhost:3000',
    Connection: 'Keep-Alive'
  },
  httpMethod: 'GET',
  isBase64Encoded: false,
  multiValueHeaders: {
    'User-Agent': [
      'Mozilla/5.0 (Windows NT; Windows NT 10.0; es-ES) WindowsPowerShell/5.1.26100.9711'
    ],
    Host: [ 'localhost:3000' ],
    Connection: [ 'Keep-Alive' ]
  },
  multiValueQueryStringParameters: null,
  path: '/hello',
  pathParameters: null,
  queryStringParameters: null,
  requestContext: {
    accountId: 'offlineContext_accountId',
    apiId: 'offlineContext_apiId',
    domainName: 'offlineContext_domainName',
    domainPrefix: 'offlineContext_domainPrefix',
    extendedRequestId: '14bb0aac-5856-4701-8af7-3a34af7ac260',
    httpMethod: 'GET',
    identity: {
      accessKey: null,
      accountId: 'offlineContext_accountId',
      apiKey: 'offlineContext_apiKey',
      apiKeyId: 'offlineContext_apiKeyId',
      caller: 'offlineContext_caller',
      cognitoAuthenticationProvider: 'offlineContext_cognitoAuthenticationProvider',
      cognitoAuthenticationType: 'offlineContext_cognitoAuthenticationType',
      cognitoIdentityId: 'offlineContext_cognitoIdentityId',
      cognitoIdentityPoolId: 'offlineContext_cognitoIdentityPoolId',
      principalOrgId: null,
      sourceIp: '::1',
      user: 'offlineContext_user',
      userAgent: 'Mozilla/5.0 (Windows NT; Windows NT 10.0; es-ES) WindowsPowerShell/5.1.26100.9711',
      userArn: 'offlineContext_userArn'
    },
    operationName: undefined,
    path: '/hello',
    protocol: 'HTTP/1.1',
    requestId: '3636b3e2-c42e-4b3a-852c-ff26670bac4d',
    requestTime: '27/Oct/2026:08:44:57 +0100',
    requestTimeEpoch: 1793087097106,
    resourceId: 'offlineContext_resourceId',
    resourcePath: '/dev/hello',
    stage: 'dev'
  },
  resource: '/hello',
  stageVariables: null
}
(λ: hello) RequestId: 4534cddc-f8cd-4783-aca4-15c9def778fd  Duration: 121.38 ms  Billed Duration: 122 ms