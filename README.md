# Prueba de georreferenciación para censo web (CAWI)

Prototipo para medir qué precisión de ubicación entrega el navegador según el tipo de dispositivo, y probar el flujo de **ubicación obligatoria antes del llenado**.

## Qué hace
1. Diagnóstico: HTTPS, soporte de la API, estado del permiso, tipo de dispositivo, SO, navegador y si se abrió dentro de una app (WhatsApp/Facebook WebView).
2. Consentimiento explícito y solicitud de ubicación (`watchPosition`, alta precisión) durante hasta 30 s, quedándose con la mejor lectura; se detiene antes si llega a ±10 m.
3. Clasifica la precisión (Muy alta ≤10 m · Alta ≤30 · Media ≤100 · Baja ≤1000 · Muy baja) y estima la fuente (GNSS, Wi-Fi, celda/IP).
4. La persona confirma o arrastra el punto de su vivienda; se mide la distancia entre la lectura y el punto confirmado.
5. Cuestionario bloqueado hasta obtener ubicación (`CONFIG.OBLIGATORIO`).
6. Pestaña **Resultados**: resumen por dispositivo (mediana, P90, % ≤30 m), mapa y exportación CSV/GeoJSON.

## Publicar en GitHub Pages
1. Cree un repositorio y suba `index.html` (y este README).
2. Settings › Pages › Deploy from branch › `main` / root.
3. Abra `https://<usuario>.github.io/<repo>/` (HTTPS es obligatorio para la geolocalización).

## Configuración (inicio del `<script>` en index.html)
| Parámetro | Uso |
|---|---|
| `OBLIGATORIO` | `true` bloquea el cuestionario sin ubicación; `false` permite marcar la vivienda manualmente en el mapa |
| `PRECISION_OBJETIVO_M` | Precisión a la que se detiene la captura |
| `TIEMPO_MAX_S` | Tiempo máximo de captura |
| `ENDPOINT` | URL del Apps Script (`apps-script.gs`) para consolidar todos los dispositivos en Google Sheets |

Sin `ENDPOINT`, los registros quedan solo en el navegador de cada dispositivo (exportables a CSV).

## Protocolo de prueba sugerido
Repetir con cada combinación y anotar en *Observaciones*:
- Dispositivo: Android, iPhone, laptop Windows/Mac, PC de escritorio por cable.
- Entorno: interior lejos de ventanas, junto a ventana, exterior.
- Wi-Fi encendido/apagado; ubicación exacta activada/aproximada.
- Navegador normal vs. enlace abierto desde WhatsApp.

## Notas
- La precisión reportada (`accuracy`) es un radio de confianza aproximado al 68 %, no el error real; compárela contra un punto de control conocido para validar.
- La ubicación del dispositivo **no** necesariamente es la de la vivienda (llenado desde el trabajo, etc.): por eso se registra `lugar_llenado` y el punto confirmado.
- Registro de prueba: no usar con datos reales sin cumplir la Ley 8968 y la confidencialidad estadística (Ley 7839).
