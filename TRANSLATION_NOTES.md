# Traducción al inglés — estado y cómo seguir

Este doc es para retomar el trabajo de la versión en inglés del sitio en otra conversación.

## Estructura

- Sitio en español: `index.html`, `carbondig.html`, `alpogo.html` (raíz)
- Sitio en inglés: `en/index.html`, `en/carbondig.html`, `en/alpogo.html`
- Los assets (`assets/`, imágenes, CSS, JS) están **compartidos** — las páginas de `en/` los referencian con `../assets/...`. No se duplica nada salvo el HTML con el texto.
- Selector de idioma (EN/ES) ya está agregado en las 6 páginas: en `index.html`/`carbondig.html` es un `<li>` más en el nav; en `alpogo.html` (que no tiene nav visible) es un link suelto al lado del botón hamburguesa.

## Estado por página

### `en/index.html` — ✅ Completa
Todo el contenido traducido: nav, hero, sección visión, enfoque, qué hacemos, equipo/founders, ODS, sección "impacto medible" (case study Alpogo), quiénes somos, footer, menú lateral (offcanvas).

**Pendiente en esta página:** convertir los títulos (h2/h3/h4/h5) a Title Case (cada palabra con mayúscula inicial, no solo la primera del texto). El usuario pidió esto último y quedó a mitad de camino. Ubicaciones exactas encontradas (línea aprox., puede correrse si se edita el archivo):

- L130: `<h3 class="tp-offcanvas-title">Join our<br>Digital Pioneers<br>community</h3>`
- L432: `We believe in a new way of building technology`
- L479: `From data to action`
- L626: `Measurement that turns into business value`
- L638: `Opening the conversation` (título de card, sigue en la línea siguiente con "in the region")
- L886: `Measurable impact, real transformation`
- L1033: `Women-led founding team<br> with 15+ years<br> in technology`
- L1416: `<br><strong>Climate <br> action:</strong>` (hay otros 2 títulos de tarjetas ODS arriba de esta: "Sustainable innovation and infrastructure:" y "Responsible production and consumption:" — también pendientes)
- L1674: `The digital future<br>  is designed today.`

No se tocaron los labels que ya están en mayúsculas completas (ej. "OUR APPROACH", "WHAT WE DO", "ABOUT US") ni los botones ni el copy del cuerpo (párrafos) — solo títulos.

### `en/carbondig.html` — 🟡 Solo arrancado
Lo único hecho: selector de idioma agregado, y el "Comunicate con nosotras" del menú lateral → "Contact us". **Falta absolutamente todo el resto del contenido** (hero, metodología, cómo trabajamos, stats, logos de clientes, equipo, footer).

### `en/alpogo.html` — 🟡 Solo arrancado
Lo único hecho: selector de idioma agregado (link "ES" suelto junto al botón de menú, porque esta página no tiene nav visible — está comentado en el HTML original).

**Pregunta abierta sin resolver:** el usuario mostró una captura con el headline "Impacto medible, transformación real" / body "Mejorar el negocio y cuidar el planeta..." / botón "VER EL CASO DE USO" con un logo "alpogo.com" sobre foto de recital — buscamos ese texto en `alpogo.html` (español e inglés) y en la versión en producción, y **no existe en ningún lado del código actual**. Ya se tradujo y aplicó ese mismo texto en `index.html` (sección "impacto medible" que linkea a Alpogo), así que puede que el usuario se refiriera a esa sección y ya esté resuelto — pero si vuelve a aparecer el pedido para `alpogo.html` específicamente, aclarar con el usuario si es contenido nuevo a crear o dónde exactamente va.

## Convenciones de traducción usadas hasta ahora

- Nombres de marca, personas, emails, nombres de comunidades (ej. "Mujeres en Tecnología", "Green Software Argentina") **no se traducen**.
- Excepción explícita del usuario: en el copyright del footer, "Ceibo Estudio" → "Ceibo Studio" (sí se tradujo esa palabra puntual).
- Botones y labels en mayúscula completa se mantienen en mayúscula completa en inglés (ej. "QUÉ HACEMOS" → "WHAT WE DO").
- Cuando el texto en español es muy largo para el layout, el usuario prefiere una versión más corta en inglés en vez de traducción literal (pasó con "Join the Digital Pioneers community" → "Join our Digital Pioneers community" en 3 líneas).
- Los links internos de las páginas en inglés deben apuntar a otras páginas en inglés (`/en/carbondig.html`, no `/carbondig.html`), excepto los que ya estaban en formato relativo simple (`carbondig.html` sin barra) que al estar dentro de `en/` resuelven solos a la carpeta correcta.

## Cómo probar localmente

Servidor local ya configurado en session anteriores:
```bash
cd /Users/luciagarbarino/ceiboweb
python3 -m http.server 8000
```
Luego abrir `http://localhost:8000/en/index.html`, etc.

## Cómo seguir

1. Terminar el Title Case pendiente en `en/index.html` (lista arriba).
2. Pedirle al usuario el copy en inglés de `carbondig.html` sección por sección (así se hizo con `index.html` — el usuario fue pasando texto en bloques: hero, metodología, cómo trabajamos, stats, equipo, footer).
3. Ídem para `alpogo.html`.
4. Confirmar con el usuario la duda de la sección "Impacto medible" en Alpogo (ver arriba).
5. Verificar todo visualmente antes de dar por terminado (el asistente tiene acceso de solo lectura al navegador vía capturas — no puede hacer click/scroll salvo que el usuario habilite la extensión de Chrome).
