# Chess Fortune — Astro + Tailwind v4 (diseño v2)

Sitio de lanzamiento de Chess Fortune. Puerto fiel del prototipo aprobado:
dorado de marca `#E0A957`, todo con border-radius, fondo azul único `#090C16`,
sin barra de progreso ni sección de acceso anticipado, y la **Seed de Oro en 3D**
(Three.js, empaquetado localmente — sin CDN).

## Desarrollo

```bash
npm install
npm run dev      # http://localhost:4321
npm run build    # genera dist/ estático
npm run preview  # sirve dist/ en local
```

Requiere Node 18.17+ o 20.3+.

## Desplegar en Vercel

**Opción A — con GitHub (recomendada, redespliega en cada push):**
1. Sube esta carpeta a un repositorio de GitHub.
2. En vercel.com → *Add New → Project* → importa el repo.
3. Vercel detecta Astro automáticamente. No toques nada: *Deploy*.

**Opción B — desde la terminal, sin GitHub:**
```bash
npx vercel        # primera vez: login + preguntas (acepta los defaults)
npx vercel --prod # publica en producción
```

No hace falta `vercel.json` ni adapter: la salida es estática y Vercel
sirve `dist/` con caché correcta para los assets con hash.

## Sistema de diseño

Todo vive en `src/styles/global.css`, bloque `@theme`:

- **Colores** — `--color-gold #e0a957` (del logo), `--color-navy #090c16`
  (fondo único), `--color-surface / surface-2 / line` para tarjetas y bordes.
- **Radios** — cuatro tokens controlan el redondeo de toda la web:
  `--radius-btn 14px` · `--radius-card 22px` · `--radius-panel 32px` ·
  `--radius-chip 999px`. Se usan como `rounded-btn`, `rounded-card`, etc.
- **Filetes** — clase `rule` (+ `rule-fade`): línea de 1px con extremos
  redondeados que sustituye a todos los `border-t/b` rectos.

El copy completo está en `src/data/site.ts`. Los assets de marca en
`public/brand/` (logo y Seed).

## Moneda 3D

`src/components/Seeds.astro`. Three.js se importa de npm y Vite lo trocea en
un chunk propio (~510 KB, 133 KB gzip) que **solo carga en `/` y `/seeds`**.
La moneda extruye el trazado real de `public/brand/seed.svg` en relieve sobre
ambas caras. Ajustes en el objeto `CFG` del script: grosor, relieve, velocidad
de giro, flotación. Incluye: fallback al SVG plano sin WebGL, pausa fuera de
pantalla, fotograma estático con `prefers-reduced-motion`, y liberación del
contexto al navegar (View Transitions).

## Antes de publicar

1. Cifras de `stats` en `site.ts` — son marcador de posición.
2. Fotos reales en `public/img/social-0X.jpg` (ahora placeholders).
3. `public/og.jpg` (1200×630) para compartir en redes.
4. Formulario de contacto: valida y confirma pero no envía — conecta tu
   endpoint (Resend, Formspree…).
5. `terminos.astro` es plantilla, no texto legal validado.
6. Si el dominio final no es chessfortune.com, cambia `site` en
   `astro.config.mjs`.
