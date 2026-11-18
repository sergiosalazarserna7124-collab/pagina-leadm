# Landing de Lead Master

Página de captación de **leadmaster.com.co**: titular, video, formulario para agendar la llamada, clientes y explicación corta.

- **Video:** el reproductor se carga solo al dar clic; lo que se ve antes es la portada. Si la versión de la página tiene un video alojado aquí (`mp4`), usa el reproductor propio; si no, el de vturb (ConverteAI).
- **Dos versiones de la página (prueba A/B):** la misma página con otro video. La versión se sortea la primera vez que alguien entra y se le queda en ese navegador. Se configura en el bloque `VERSIONES DE LA PÁGINA`, al inicio de `index.html`.
- **Seguimiento:** la página le avisa al backend la visita (con el anuncio que la trajo), el clic al video y cuánto se vio. También manda, una vez por visita, la **velocidad real** en el teléfono del visitante (cuándo respondió el servidor, cuándo se vio la página y cuándo terminó de cargar); sale en el reporte por versión y por navegador (Facebook, Instagram, Safari, Chrome). Con eso y el formulario sale el embudo de cada versión: visitas, clic al video, retención, teléfonos, agendas y confirmaciones por WhatsApp. Ver `docs/embudo-landing/README.md` en `tracker_v1` (ahí está también cómo pedir el reporte).
- **Formulario propio en dos pasos** (justo debajo del video), en lugar del calendario incrustado de GHL:
  1. *Tus datos*: nombre, WhatsApp con selector de país (desde el 17-nov-2026 el nombre va primero, pedido de Sergio), leads al mes, cantidad de asesores, rol en la inmobiliaria (dueño, director comercial, marketing, asesor u operaciones), presupuesto («Nuestro plan mensual arranca desde 197 USD. ¿Esto está dentro de tu presupuesto?», Sí / No, con el aproximado en pesos si eligió México o Colombia; desde el 18-nov-2026, pedido de Sergio: va aquí y no por WhatsApp), Instagram o página web y correo (obligatorio: va al contacto de GHL antes de agendar, así le llega la invitación del calendario con el enlace de la videollamada).
  2. *Día y hora*: horarios libres del calendario de GHL.
- El formulario habla con el backend de Lead Master (`/embudo-landing/*` en `tracker_v1`). Ahí se crea el contacto en GHL apenas se escribe el WhatsApp, se agenda la cita y sale la confirmación por WhatsApp. Ver `docs/embudo-landing/README.md` en ese repo.
- **Pixel de Meta:** `PageView` al cargar, `Lead` al dejar el WhatsApp y `Schedule` al agendar, pero solo si la agenda califica (100 leads al mes o más): lo decide el backend y manda el mismo evento por la API de conversiones con el mismo id (`lm-agenda-<id>`), así Meta lo cuenta una vez. Ver «Qué agenda le cuenta a Meta» en `docs/embudo-landing/README.md` de `tracker_v1`.

## Archivos

| Archivo | Qué es |
|---|---|
| `index.html` | La página completa: HTML, CSS y JS en un solo archivo (19 KB comprimido), sin dependencias. |
| `portada-vsl.jpg` | Portada del video a 1120 px (respaldo y vista previa al compartir). |
| `portada-vsl.webp`, `portada-vsl-800.webp`, `portada-vsl-640.webp` | La misma portada en WebP a 1120, 800 y 640 px; cada pantalla baja la que le corresponde. |
| `portada-editado.jpg`, `.webp`, `-800.webp`, `-640.webp` | La portada de la versión B: Sergio a cámara, video editado. |
| `portada-anterior.jpg`, `.webp`, `-800.webp`, `-640.webp` | La portada del video original de Sergio (la B hasta el 14-nov); queda por si se vuelve a él. |
| `videos/` | Los videos alojados aquí (MP4 H.264 + AAC, con el índice al inicio: `-movflags +faststart`). |
| `web/` | Otra página, en `/web`. |

## Cambios frecuentes

- **Cambiar el video de una versión:** en el bloque `VERSIONES DE LA PÁGINA` de `index.html`, el `mp4` (o el `vturb`) de esa versión, su `video` (el nombre con el que sale en el reporte) y su `portada` (nombre base de las cuatro imágenes: `.jpg` y `.webp` a 1120 px, `-800.webp` y `-640.webp`).
- **Las dos versiones:** A es el video nuevo («Lead Master explicado fácil») y B Sergio a cámara en su versión editada (animaciones y subtítulos nuevos, `sergio-editado` en el reporte), con sus portadas `portada-editado*`. Compiten mitad y mitad, las dos con el reproductor de vturb. La B usó el video original (`sergio-anterior`) solo el 14-nov en la noche; para comparar, pedir el reporte `desde` el 15-nov.
- **Prender o apagar la versión B:** el `peso` de cada versión. `peso:1` en las dos reparte mitad y mitad; `peso:0` la apaga (solo se ve con `?v=b`).
- **Ver una versión a propósito:** `leadmaster.com.co/?v=a` o `?v=b`.
- **Que las visitas del equipo no cuenten:** abrir una vez `leadmaster.com.co/?interno=1` en ese navegador (`?interno=0` lo desmarca).
- **Enlace de los anuncios:** `https://leadmaster.com.co/` con `utm_source`, `utm_medium`, `utm_campaign`, `utm_term`, `utm_content` y los ids `ad_id`, `adset_id` y `campaign_id`. La página los guarda con la visita y con el lead.
- **Cambiar las opciones de las preguntas:** los botones `.opt` de los bloques `data-q="leads_mes"`, `data-q="asesores"`, `data-q="rol"` y `data-q="presupuesto"`. El precio y el aproximado en pesos de la pregunta del presupuesto: el texto de `#q-presu` y `APROX_PLAN` en el script.
- **Datos y clientes:** la sección "PRUEBA SOCIAL". Solo datos reales. Cada cliente es un `<span>` (texto, sin enlace) dentro de la cinta que se mueve; al agregar o quitar uno, hazlo en las dos listas de `.cinta-pista` (la segunda es la copia que da la vuelta).
- **Backend del formulario:** la constante `API` al inicio del `<script>`.
