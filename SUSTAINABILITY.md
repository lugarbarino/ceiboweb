# Sostenibilidad digital del sitio — qué se hizo y qué no romper

Ceibo vende sostenibilidad digital, así que este sitio tiene que predicar con el ejemplo: liviano, sin código muerto, sin peso innecesario. Este doc resume las optimizaciones hechas y sirve para que cualquier sesión de Claude (o de otro dev) entienda el estado actual antes de tocar algo — para no deshacer sin querer el trabajo hecho.

## Resultado medido

Test real en [websitecarbon.com](https://www.websitecarbon.com) sobre `ceiboestudio.com`: pasó de **F** (más sucio que el 95% de la web) a **D** (más limpio que el 52%) después de esta ronda de optimización.

## Qué se hizo (no repetir, no deshacer)

### Archivos y código eliminados
- `node_modules/` (era de Sass para compilar, no hace falta para servir el sitio) — **no reinstalar en el repo**.
- `.DS_Store` en todo el proyecto — está en `.gitignore`, no debería volver a colarse.
- Imágenes, fuentes e íconos del template original que no se usaban en ninguna página real (se verificó cruzando cada archivo contra las 3 páginas antes de borrar).
- `PHPMailer/`, `mail.php`, `mail-contact-us.php`, `ajax-form.js` — el sitio **no tiene formulario de contacto**, todo el contacto es por link de WhatsApp. Si en algún momento se agrega un formulario real, ahí sí hay que reinstalar algo de esto (o usar un servicio como Formspree).
- Secciones muertas del template que estaban comentadas en el HTML (blog nunca usado, testimonios genéricos, etc.) — se dejaron comentadas donde no molestaban, se borraron del todo donde sí.

### Imágenes
- Todas las imágenes de fondo/fotos pesadas se convirtieron a **WebP** cuando eso las hacía más livianas (no todas mejoran con WebP — se comprobó cada una antes de reemplazar).
- Imágenes con resolución absurdamente mayor a su tamaño real en pantalla se redimensionaron antes de comprimir.
- **Regla para nuevas imágenes:** subir en WebP directamente si se puede, o pedir que se optimicen antes de subirlas al repo. No subir JPG/PNG sin comprimir de cámara/diseño sin pasar por optimización.

### CSS y JS
- `main.css`, `three.js`, `bootstrap-bundle.js`, `swiper-bundle.js`, `main.js`, `slider-init.js` están **minificados** (sin espacios/comentarios). Si hay que editarlos a mano, tener cuidado: el código está comprimido en pocas líneas larguísimas, no es código "legible" a simple vista pero sigue siendo CSS/JS válido y editable con cuidado (buscar el selector exacto con grep en vez de tratar de leer visualmente).
- `main.css` además se **podó**: se sacaron del todo las reglas de features que el template traía pero Ceibo nunca usa (sistema de blog, formulario de contacto, mapa, carrito, checkout, login, perfil de usuario, y variantes de secciones — hero/banner/equipo en estilos que no se usan). Bajó de 690KB a 184KB (~184KB → 25KB con gzip).
  - **Antes de purgar más CSS:** si se quiere seguir sacando reglas sin usar, el método seguro que funcionó fue: extraer las clases usadas en las 3 páginas HTML + todos los strings de los archivos JS (para no romper clases que el JS agrega dinámicamente tipo `active`, `show`, `collapsed`, `swiper-slide-active`), y solo borrar una regla CSS si el 100% de las clases de su selector no aparecen en ese set combinado. Una herramienta automática tipo PurgeCSS a ciegas **rompió el header** en un intento anterior — no confiar en herramientas automáticas sin esta verificación manual clase por clase.

### Lazy loading
- La mayoría de las imágenes tienen `loading="lazy"` (no cargan hasta que el usuario scrollea hasta ahí). Se dejó sin lazy loading solo la primera imagen de cada página (la del hero), a propósito, para no perjudicar la carga inicial.
- **Regla para contenido nuevo:** cualquier `<img>` nueva que no sea la primera de la página debería llevar `loading="lazy"`.

### Servidor (Donweb)
- Hay un `.htaccess` en la raíz que activa **compresión Gzip** y **cache del navegador** (imágenes/fuentes cacheadas 1 año, CSS/JS 1 mes, HTML 1 hora). Confirmado funcionando en producción (`content-encoding: gzip` en las respuestas reales). No borrar este archivo.

## Qué NO se hizo (y por qué)

- **Minificar el HTML** (sacar espacios en blanco): se probó y el ahorro real es mínimo (2-4%) porque Gzip ya comprime esos espacios casi del todo. No vale la pena el riesgo de romper algo por tan poca ganancia.
- **Purgar más CSS de forma agresiva**: se llegó a un punto conservador y verificado. Se podría seguir (hay variantes del template tipo "-it", "-wd", "-cst" mezcladas, algunas usadas y otras no), pero requiere el mismo proceso cuidadoso de verificación clase por clase — no apurarse con herramientas automáticas.
- **Optimizar `carbondig.html`/`alpogo.html` a fondo como se hizo con `index.html`**: en general todas las páginas comparten los mismos assets optimizados (CSS/JS/imágenes), así que se benefician igual. Lo que puede faltar es limpieza de contenido específico de cada página (revisar si tienen secciones muertas propias sin detectar).

## Cómo verificar que sigue liviano

```bash
cd /Users/luciagarbarino/ceiboweb
du -sh assets/                    # peso total de assets
du -h assets/css/main.css         # CSS principal
gzip -c assets/css/main.css | wc -c   # peso real que viaja (comprimido)
```

Y en producción, test real de carbono: https://www.websitecarbon.com/website/ceiboestudio-com/ (usar "Test again", no el resultado cacheado).
