# Landing de Lead Master

Página de captación de **leadmaster.com.co**: titular, video, formulario para agendar la llamada, clientes y explicación corta.

- **Video:** vturb (ConverteAI). El reproductor se carga solo al dar clic; lo que se ve antes es la portada.
- **Formulario propio en dos pasos** (justo debajo del video), en lugar del calendario incrustado de GHL:
  1. *Tus datos*: WhatsApp con selector de país, nombre, leads al mes, cantidad de asesores e Instagram o página web.
  2. *Día y hora*: horarios libres del calendario de GHL.
- El formulario habla con el backend de Lead Master (`/embudo-landing/*` en `tracker_v1`). Ahí se crea el contacto en GHL apenas se escribe el WhatsApp, se agenda la cita y sale la confirmación por WhatsApp. Ver `docs/embudo-landing/README.md` en ese repo.
- **Pixel de Meta:** `PageView` al cargar, `Lead` al dejar el WhatsApp y `Schedule` al agendar.

## Archivos

| Archivo | Qué es |
|---|---|
| `index.html` | La página completa: HTML, CSS y JS en un solo archivo (19 KB comprimido), sin dependencias. |
| `portada-vsl.jpg` | Portada del video a 1120 px (respaldo y vista previa al compartir). |
| `portada-vsl.webp`, `portada-vsl-800.webp`, `portada-vsl-640.webp` | La misma portada en WebP a 1120, 800 y 640 px; cada pantalla baja la que le corresponde. |
| `web/` | Otra versión de la página, en `/web`. |

## Cambios frecuentes

- **Cambiar el video:** en `index.html`, el atributo `data-vsl` del bloque `#vsl`, y reemplazar las cuatro portadas.
- **Cambiar las opciones de las preguntas:** los botones `.opt` de los bloques `data-q="leads_mes"` y `data-q="asesores"`.
- **Datos y clientes:** la sección "PRUEBA SOCIAL". Solo datos reales.
- **Backend del formulario:** la constante `API` al inicio del `<script>`.
