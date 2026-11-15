# Vegeta Live – overlay interactivo para TikTok LIVE

Réplica del tipo de directo del vídeo (personaje en pantalla, **Top de poder**, **barra de taps con transformaciones**,
**regalos que activan acciones** y **el personaje contesta al chat**), pero con Vegeta.

## 1. Requisitos
- PC con **Node.js 18+** (https://nodejs.org).
- Tu cuenta debe poder hacer LIVE desde PC (TikTok LIVE Studio u OBS) – según los requisitos que TikTok te dé.

## 2. Instalación
```bash
cd vegeta-live
npm install
```
Edita `config.json` y pon tu usuario en `tiktokUsername` (sin @).

## 3. Probar sin estar en directo
```bash
npm run demo
```
Abre http://localhost:3000/?debug=1&audio=0 → panel de pruebas arriba a la izquierda (botones para cada regalo, subir taps, cambiar de transformación, chat…).
Pulsa el botón amarillo "Activar voz" una vez para oír a Vegeta (voz del navegador).

## 3b. Usarlo con un enlace (sin tener tu PC encendido)
Hay que alojar esta carpeta una vez en internet (ya trae `Dockerfile` y `render.yaml`):
1. Sube la carpeta a un repositorio de GitHub.
2. En render.com: *New → Blueprint* → elige el repositorio. Te dará una URL tipo `https://vegeta-live.onrender.com`.
3. En Render, en *Environment*, copia el valor de `ADMIN_KEY` (tu clave) y, si quieres IA, añade `ANTHROPIC_API_KEY`.
4. Cada directo, desde el móvil: abre `https://TU-URL/start.html`, escribe tu usuario + clave, **Conectar**, y abre `https://TU-URL/`.
Aviso: TikTok puede bloquear conexiones desde servidores en la nube; si no conecta, ejecútalo en tu PC (sección 5).

## 4. Poner a tu Vegeta
Ya viene configurado con la hoja `public/assets/vegeta/sheet.jpg` (tu imagen de las fases): el overlay recorta cada fase solo (ver `sheet` en config.json). Es de baja resolución; para mejor calidad pon una imagen por forma como se explica abajo, que tiene prioridad sobre la hoja.
Mira `public/assets/vegeta/LEEME.txt` (una imagen/vídeo por forma) y `public/assets/actions/LEEME.txt` (clips opcionales por acción).
**No incluyo imágenes de Vegeta**; usa material propio o con licencia.

## 5. Ir en directo
1. Empieza tu LIVE en TikTok.
2. En esta carpeta: `npm start` (o `npm start tuusuario`).
3. Abre http://localhost:3000 en una ventana del navegador de formato vertical (1080×1920 se escala solo).
4. En **TikTok LIVE Studio** añade esa ventana como fuente (captura de ventana / navegador). Con OBS: *Fuente de navegador* a `http://localhost:3000` (1080×1920) y emite por clave de retransmisión.
   Usa `?bg=transparent` si quieres superponerlo sobre otra escena.

## 6. Respuestas de Vegeta en el chat (opcional, con IA)
Sin clave, contesta con frases predefinidas. Con clave de Anthropic, genera respuestas en personaje:
- Windows (PowerShell): `$env:ANTHROPIC_API_KEY="tu_clave"; npm start`
- Mac/Linux: `ANTHROPIC_API_KEY=tu_clave npm start`
Límite: 1 respuesta cada `cooldownMs` (8 s) y cola de 3 mensajes (ajustable en `config.json`).

## 7. Personalizar (config.json)
- `goalTaps`, `tiers`: meta de taps y umbral de cada transformación.
- `actions`: qué regalo dispara qué acción. Los nombres de regalo (`gifts`) son los **nombres en inglés de TikTok**.
  Cada regalo que llegue aparece en la consola (`🎁 usuario -> NombreDelRegalo`): copia ese nombre exacto a `gifts` de la acción que quieras.
  Los que traigo son suposiciones de nombres habituales (Rose, GG, Finger Heart, Doughnut, Teddy Bear...) y **hay que verificarlos**.
- `state.json` guarda taps y ranking entre directos (`type=reset` en el panel de debug, o borra el archivo, para empezar de cero).

## Notas honestas
- Usa `tiktok-live-connector`, una librería **no oficial**: puede romperse si TikTok cambia algo y su uso es bajo tu responsabilidad respecto a las normas de TikTok.
- La parte de conexión con TikTok **no la he podido probar desde aquí** (sin acceso a internet ni a un directo); el overlay, ranking, taps, acciones, transformaciones y respuestas sí están probados en modo demo.
- Los efectos de acción (rayo, escáner, fusión...) son provisionales; con tus clips quedarán como los del vídeo.
- Los iconos de regalos son emojis (no uso los de TikTok).
