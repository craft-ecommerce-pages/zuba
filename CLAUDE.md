# CLAUDE.md — Las Hamburguesas del Gordo Paul

Menú digital para **Las Hamburguesas del Gordo Paul** (smash burgers artesanales, Quito, Ecuador).

## Tecnología

Mismo stack que pizza-planet: HTML/CSS/JS vanilla, sin build.

```
index.html          # UI
config.json         # config tienda (tema, whatsapp, mapa, category_order) — sync solo pisa `categories`
productos.json      # generado por catalogsync (no editar a mano)
assets/app.js       # lógica
assets/style.css    # estilos
media/logo.webp     # logo (fondo transparente, 1:1 ~800×800) — lo sube el cliente
media/favicon.png   # favicon — lo sube el cliente
```

Imágenes de productos: las mapea catalogsync en el push (no se versionan a mano).

## Fuente de verdad de los productos

**craft-crm** (nodo core, producción). Los productos viven en la DB por `business_id`.
`catalogsync` regenera `productos.json` + la clave `categories` de `config.json` desde la DB y hace push a este repo → Cloudflare Pages despliega.

Todo lo demás de `config.json` (tema, tipografías, WhatsApp, ubicación, redes) lo controla este repo y **se preserva** en cada sync. No editar `productos.json` a mano.

## Datos del cliente

- **WhatsApp pedidos**: +593986131942
- **Ubicación**: Quito, Ecuador (mapa embebido en `config.location.map_embed`)
- **Cloudflare Pages**: `las-hamburguesas-del-gordo-paul.pages.dev`

## Branding

- **Paleta**: primary `#E4801C` (naranja) · accent `#F5B301` (ámbar) · fondo casi negro. En `config.json` (`theme_primary`/`theme_accent`) y defaults en `style.css`.
- **Tipografías**: Anton (títulos display) + DM Sans (cuerpo).

## Modelo de productos (hamburguesas)

- Variante **Presentación**: Simple / Doble / Triple (recargo por tamaño).
- Variante **Tipo**: Sola / Combo (+ papas fritas + cola).
- Variante **Vegetales** (solo las que llevan vegetales frescos): Con vegetales (lechuga, tomate, cebolla) / Sin vegetales.
- Imágenes: se cargan desde la UI de craft-crm (no viven en el repo).

## Deploy

```bash
git add . && git commit -m "descripción" && git push
```

Cloudflare Pages despliega automáticamente en push a `main` (workflow en `.github/workflows/deploy.yml`).
