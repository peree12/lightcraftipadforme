# LightCraft para móviles, iPad y GitHub Pages

Esta versión de LightCraft funciona en iPhone, iPad, Android y computadores desde el navegador,
y se publica sola en **GitHub Pages**. Es el mismo programa de escritorio (compilado a
WebAssembly): conserva todas sus funciones, la biblioteca, el revelado RAW, las máscaras,
los presets y la exportación.

## Qué se agregó

| Mejora | Detalle |
|---|---|
| **Pantallas táctiles** | Botones y bordes más fáciles de tocar, barras de desplazamiento anchas, arrastrar con el dedo para desplazar. En iPad la interfaz se agranda un 15 %. |
| **Barra táctil flotante** | Un botón redondo (•••) abajo a la derecha. Al tocarlo muestra lo que en el computador se hace con el teclado: Cuadrícula, Editar, Atrás, Deshacer, Rehacer, Importar fotos, paneles, tira de película, Mostrar original, Encajar, Zoom, Pantalla completa y Ajustes. Se puede arrastrar a otro lugar. |
| **Gestos** | Pellizcar para hacer zoom en la foto, arrastrar para moverla, doble toque para acercar, **mantener presionado = clic derecho** (menús contextuales). |
| **Teclado externo del iPad** (Magic Keyboard, Smart Keyboard, Bluetooth) | Todos los atajos funcionan, incluidos los de **⌘ Cmd** (⌘Z deshacer, ⌘⇧I importar, ⌘0 encajar, ⌘, ajustes…). El foco vuelve solo a la app después de usar el selector de fotos. |
| **Apple Pencil** | El pincel de máscaras usa la **presión** del Pencil (más fuerte = trazo más grueso). Con el dedo pinta a presión completa, como un mouse. |
| **Pantalla completa** | Vista ▸ Pantalla completa o el botón de la barra táctil (iPad, Android, computador). En iPhone, y para la mejor experiencia en iPad, usar "Agregar a pantalla de inicio". |
| **App web en la pantalla de inicio (iOS y Android)** | Icono propio, se abre a pantalla completa sin las barras de Safari, respeta el notch y la barra de inicio, y **funciona sin conexión**. |
| **Memoria** | En teléfonos y tablets usa menos procesos de renderizado y menos memoria, para que Safari no cierre la pestaña. |
| **Pantallas pequeñas** | En teléfonos el panel "Mis fotos" y la tira de película empiezan ocultos para dejar espacio a la foto y al panel de edición (se abren desde la barra táctil). |

## Publicarlo en GitHub Pages (una sola vez)

1. Crea un repositorio en GitHub (por ejemplo `lightcraft`) y sube **todo** el contenido de esta
   carpeta (incluida la carpeta oculta `.github`).
2. En el repositorio: **Settings ▸ Pages ▸ Build and deployment ▸ Source: "GitHub Actions"**.
3. Ve a la pestaña **Actions**, elige **"Deploy to GitHub Pages"** y pulsa **Run workflow**
   (también se ejecuta solo con cada cambio que subas a `main`).
4. La primera compilación tarda unos 30–60 minutos (las siguientes son más rápidas gracias a la
   caché). Al terminar, la app queda en:

   `https://TU-USUARIO.github.io/NOMBRE-DEL-REPO/`

> El repositorio original trae otros flujos de trabajo (CI, SonarCloud, versiones para Windows,
> etc.) que en tu copia pueden fallar porque necesitan claves del proyecto original. No afectan a
> GitHub Pages; si quieres, desactívalos en **Actions ▸ (flujo) ▸ ⋯ ▸ Disable workflow**.

## Si GitHub Pages muestra textos en vez del programa

Significa que Pages está en modo **"Deploy from a branch"**: publica los archivos del
repositorio tal cual (el README), sin compilar LightCraft. En ese caso ahora aparece una página
"Falta un paso para abrir LightCraft" con tres botones:

1. **Crear el archivo**: agrega `.github/workflows/pages.yml` (en Mac la carpeta oculta `.github`
   suele quedar fuera al subir; pulsa **⌘ ⇧ .** en el Finder para verla).
2. **Abrir ajustes de Pages**: en *Source* elige **GitHub Actions**.
3. **Abrir Actions**: **Run workflow** y espera el visto verde (30–60 min la primera vez).

Comprueba también que subiste **todo** el código (`apps`, `crates`, `xtask`, `assets`,
`Cargo.toml`, `Cargo.lock`…). La web de GitHub sube máximo 100 archivos por vez y este proyecto
tiene unos 1 400, así que es más fácil con **GitHub Desktop** (clonar el repositorio, copiar
todos los archivos dentro de la carpeta y hacer *Commit* + *Push*).

## Instalarla como app en iPhone / iPad

1. Abre la dirección de GitHub Pages en **Safari**.
2. Toca **Compartir** (cuadrado con flecha) ▸ **Agregar a pantalla de inicio** ▸ **Agregar**.
3. Ábrela desde el icono del lince: se abre a pantalla completa, como una app, y funciona sin
   conexión.

En Android (Chrome): menú ⋮ ▸ **Instalar app** / **Agregar a la pantalla principal**.

## Dónde quedan las fotos

Como en la versión web original, la biblioteca y las fotos importadas se guardan **en el
navegador del dispositivo** (no en GitHub). Usa **Archivo ▸ Hacer copia de seguridad de la biblioteca…** de
vez en cuando para descargar una copia, y **Archivo ▸ Restaurar biblioteca desde copia de
seguridad…** para recuperarla en otro equipo.

## Opciones en la dirección (URL)

| Agregar al final | Efecto |
|---|---|
| `?touch=1` / `?touch=0` | Forzar o desactivar el modo táctil. |
| `?zoom=1.3` | Tamaño de la interfaz (0.5 a 3). |
| `?store=idb` | Guardar en IndexedDB en vez de OPFS. |
| `?workers=1` | Menos procesos de renderizado (si un equipo antiguo se queda sin memoria). |

## Probarla en tu computador

```sh
rustup target add wasm32-unknown-unknown
cargo install wasm-bindgen-cli --version 0.2.129 --locked
cargo xtask web --serve      # luego abre http://127.0.0.1:8080/?touch=1
```

## Archivos modificados

- `apps/lightcraft-web/index.html`: detección de dispositivo, gestos, teclado del iPad, Apple
  Pencil, pantalla completa, instalación como app.
- `apps/lightcraft-web/manifest.webmanifest`, `sw.js`, `icons/`: app instalable y sin conexión.
- `apps/lightcraft-web/src/mobile.rs` (nuevo), `web.rs`, `store.rs`, `lib.rs`: integración con la app.
- `crates/ui-egui/src/touch_bar.rs` (nuevo), `theme.rs`, `lib.rs`: barra táctil y estilo táctil.
- `xtask/src/web.rs`: incluye los archivos nuevos en la compilación web.
- `.github/workflows/pages.yml` (nuevo): compila y publica en GitHub Pages.
