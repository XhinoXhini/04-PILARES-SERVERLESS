# Bloque 05

## Resultado
- **Qué añadí al proyecto:** Aplicación cliente React Native configurada con Expo Web en la carpeta `mobile/`, tipada con TypeScript (`types.ts`) y estructurada con componentes nativos (`View`, `Text`, `ScrollView`, `Pressable`, `StyleSheet`).
- **Ruta o pantalla comprobada:** Interfaz servida en navegador web vía Metro (`http://localhost:8081`).
- **Estado y respuesta observados:** Renderizado de dos tarjetas de eventos locales (`EVT-101` y `EVT-102`) y reactividad en el botón que actualiza dinámicamente el estado local a `Selección: EVT-101`.

## Comprensión
- **Recorrido del dato, explicado con mis palabras:** Los datos de las actuaciones se declaran en el cliente en una constante local tipada `localEvents`. La función `map` itera sobre el array para renderizar cada tarjeta con su identificador como `key`. Al pulsar el `Pressable`, el manejador `onPress` invoca `setSelected('EVT-101')`, actualizando el hook `useState` y disparando un re-render del componente para reflejar el valor en pantalla.
- **Cambio guiado y resultado:** Se modificó `setSelected` para registrar `EVT-102`, verificando que la UI mostró `Selección: EVT-102`. Al recargar el navegador, el estado volvió a `ninguna`, confirmando que no hay persistencia externa.
- **Error encontrado y cómo lo resolví:** Ninguno durante la instalación o tipado; `npm run typecheck` validó los componentes sin errores.

## Evidencias
- **Captura o transcripción breve, sin credenciales:**
  - Estado inicial: `Selección: ninguna`
  - Tras pulsar botón: `Selección: EVT-101`
  - Tarjetas renderizadas:
    - `EVT-101 · CONFIRMED | Neon Waves | Main Stage · 22:30`
    - `EVT-102 · CONFIRMED | Midnight Session | Club Stage · 23:45`
- **Limitaciones del ejercicio:** Los datos son puramente estáticos dentro del cliente `mobile`; no se realiza ninguna petición de red ni comunicación HTTP con el backend serverless.