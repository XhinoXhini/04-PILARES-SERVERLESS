# Bloque 06

## Resultado
- **Qué añadí al proyecto:** Capa de comunicación HTTP en `mobile/src/api.ts` mediante `fetch` nativo tipado, función de comprobación de tipo en tiempo de ejecución (`isFestivalEvent`) y estados asíncronos (`loading`, `error`, `events`) gestionados con `useEffect` en `mobile/App.tsx`.
- **Ruta o pantalla comprobada:** Interfaz web en `http://localhost:8081` consumiendo `GET http://localhost:3000/dev/events`.
- **Estado y respuesta observados:**
  - Backend activo: Código 200 OK en Network y renderizado de las 2 tarjetas recibidas por HTTP.
  - Backend detenido: Bloque de captura detecta la caída de red y muestra en pantalla `Error de red`.
  - Backend reiniciado: El botón `GET recargar` reintenta la petición y recupera el listado eliminando el mensaje de error.

## Comprensión
- **Recorrido del dato, explicado con mis palabras:** Al montar el componente, `useEffect` invoca la función asíncrona `loadEvents()`. Esta activa el indicador `loading`, limpia errores previos y ejecuta `getEvents()`, que envía un `fetch` GET a `http://localhost:3000/dev/events`. Al recibir la respuesta, se valida `response.ok`, se deserializa el JSON y se pasa por el type guard `isFestivalEvent`. Si todo es correcto, `setEvents` actualiza el estado y dibuja las tarjetas; si hay fallo HTTP o de red, entra al `catch` actualizando `setError`. El bloque `finally` desactiva siempre el estado de carga.
- **Cambio guiado y resultado:**
  - Al cambiar la ruta a `/ruta-inexistente`, el servidor respondió con HTTP 404 Not Found, lo que permitió verificar que el cliente procesa la respuesta HTTP de error sin crashear.
  - Al restaurar `/events`, la petición volvió a responder 200 OK.
- **Error encontrado y cómo lo resolví:** Ninguno; la política CORS configurada en el bloque 04 permitió la comunicación entre el puerto 8081 (frontend) y el 3000 (backend) sin bloqueos del navegador.

## Evidencias
- **Captura o transcripción breve, sin credenciales:**
  - Pestaña Network: `GET http://localhost:3000/dev/events` -> `200 OK`
  - Payload recibido:
    `[{"id":"EVT-101","artist":"Neon Waves","space":"Main Stage","time":"22:30","status":"CONFIRMED"},{"id":"EVT-102","artist":"Midnight Session","space":"Club Stage","time":"23:45","status":"CONFIRMED"}]`
  - Fallo simulado: Servidor detenido -> `Error de red` visible en UI.
- **Limitaciones del ejercicio:** Solo se implementa lectura (`GET`); la aplicación todavía no envía mutaciones (POST, PUT, PATCH, DELETE) desde la interfaz de usuario.