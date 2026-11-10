# Conversor Universal

Página web de conversión de archivos que funciona 100 % en el navegador del visitante (sin servidor, sin subir archivos). Un solo archivo: `index.html`.

## Qué convierte

| Categoría | Entradas | Salidas |
|---|---|---|
| Imágenes | png, jpg, webp, gif, bmp, svg, ico, avif, tiff, heic, jxl | PNG, JPG, WEBP, AVIF, JPEG XL, GIF, BMP, TIFF, ICO, SVG, PDF; GIF animado → MP4/WEBM |
| Audio | mp3, wav, ogg, m4a, aac, flac, opus, aiff, wma, amr | MP3, WAV, OGG, M4A, FLAC, OPUS, AIFF |
| Vídeo | mp4, webm, mov, avi, mkv, flv, wmv, m4v, 3gp, mpeg, ts, ogv | MP4, WEBM, MKV, MOV, AVI, GIF animado, MP3/WAV/M4A/OGG (extraer audio) |
| Documentos | pdf, docx, odt, rtf, txt, md, html, epub | PDF, DOCX, HTML, TXT, Markdown, EPUB; PDF → JPG/PNG por página, unir PDF, dividir PDF |
| Datos | xlsx, xls, ods, csv, tsv, json, xml, yaml | XLSX, ODS, CSV, TSV, JSON, YAML, XML, HTML, Markdown, SQL, PDF |
| Comprimidos | zip, tar, gz, tar.gz | Extraer, ZIP, TAR.GZ (cualquier archivo puede comprimirse a ZIP/TAR.GZ o pasarse a Base64) |
| Fuentes | ttf, otf, woff | WOFF, TTF/OTF |
| Subtítulos | srt, vtt, sbv | SRT, VTT, TXT (con desplazamiento de tiempos) |

Las librerías (FFmpeg.wasm, pdf.js, SheetJS, mammoth, docx, etc.) se descargan desde CDN solo cuando una conversión las necesita. El motor de vídeo pesa 31 MB y se descarga una sola vez.

## Poner tus anuncios de Google AdSense

Abre `index.html` y busca el bloque `const ADS = {` (al principio del `<script>`):

```js
const ADS = {
  enabled: false,                       // → true cuando tu cuenta esté aprobada
  client: 'ca-pub-XXXXXXXXXXXXXXXX',    // tu ID de editor
  autoAds: false,                       // true = además anuncios automáticos de Google (incluye anchor ads)
  sticky: true,                         // barra flotante inferior propia (con botón de cerrar)
  slots: {
    top: '1111111111',      // banner superior, bajo la cabecera (responsive horizontal)
    sidebar: '2222222222',  // lateral fijo, solo escritorio (300×600 / 300×250)
    mid: '3333333333',      // rectángulo entre el conversor y la lista de conversores
    results: '4444444444',  // bajo los resultados, solo se ve tras convertir
    bottom: '5555555555',   // rectángulo antes del pie
    sticky: '6666666666'    // barra flotante inferior
  }
};
```

1. En AdSense crea un bloque de anuncios por posición (tipo "Display", responsive) y copia el `data-ad-slot` de cada uno en `slots`.
2. Pon tu `ca-pub-…` en `client` y cambia `enabled` a `true`.
3. Si no quieres una posición, borra su ID (`''`) y la caja desaparece. Mientras `enabled` sea `false` se ven cajas de ejemplo para ubicar los anuncios.

Las posiciones en el HTML están marcadas con comentarios `<!-- ANUNCIO n -->`; puedes moverlas o duplicarlas (cada `div.ad-slot` necesita un `data-ad` que exista en `slots`).

### Requisitos de AdSense que ya cubre la página
- Política de privacidad y términos (secciones `#privacidad` y `#terminos`; cambia el correo de contacto).
- Contenido propio (preguntas frecuentes, cómo funciona, tabla de formatos).
- La barra flotante no tapa contenido (el cuerpo deja hueco) y se puede cerrar.

Pendiente de tu parte: dominio propio con HTTPS, y para visitantes de la UE activar el mensaje de consentimiento de Google en AdSense → "Privacidad y mensajes".

## Publicar la página

Es un archivo estático; sirve cualquier hosting:
- **Netlify / Vercel / Cloudflare Pages / GitHub Pages**: sube la carpeta `conversor-web`.
- **Hosting tradicional (cPanel, Hostinger…)**: sube `index.html` a `public_html`.

No necesita cabeceras especiales ni Node en el servidor. Para probar en local:

```bash
python -m http.server 8765 --directory conversor-web
```

## Personalizar
- Nombre y marca: busca "Conversor Universal" en `index.html`.
- Colores y fuentes: variables en `:root` al inicio del `<style>`.
- Conversores destacados: lista `POPULAR` en el script (cada entrada genera una tarjeta y una URL `#webp-a-png`).
