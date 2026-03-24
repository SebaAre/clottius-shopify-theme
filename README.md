# Clottius — Shopify Theme

A custom Shopify theme for **Clottius**, built on top of the Shopify Skeleton Theme. Modular, merchant-friendly, and styled with Tailwind CSS.

## Getting started

### Prerequisites

- [Shopify CLI](https://shopify.dev/docs/api/shopify-cli) — download, upload, and preview themes
- [Node.js](https://nodejs.org/) — required for the Tailwind CSS build pipeline
- [Shopify Liquid VS Code Extension](https://shopify.dev/docs/storefronts/themes/tools/shopify-liquid-vscode) (optional but recommended)

### Setup

```bash
git clone <repo-url>
cd Clottius
npm install
```

### Development

Run Tailwind in watch mode alongside the Shopify CLI dev server:

```bash
# Terminal 1 — Tailwind watcher
npm run build:css

# Terminal 2 — Shopify theme preview
shopify theme dev
```

### Build

```bash
npm run build:css
```

This compiles `assets/clottius.css` (Tailwind source) into `assets/clottius.min.css` (minified output loaded by the theme).

---

## Theme architecture

```
.
├── assets/       Static files: compiled CSS, SVG icons
├── blocks/       Reusable, nestable components editable in the theme editor
├── config/       Global theme settings schema and data
├── layout/       Top-level HTML wrappers (theme.liquid, password.liquid)
├── locales/      Translation files for i18n
├── sections/     Full-width, page-level modules
├── snippets/     Reusable Liquid fragments (not editable by merchants)
└── templates/    JSON templates defining which sections appear on each page
```

---

## Sections

| File | Description |
|------|-------------|
| `header.liquid` | Site header with navigation menu, account icon, and cart |
| `footer.liquid` | Site footer with menu links and payment icons |
| `hello-world.liquid` | Hero banner with brand name, tagline, and "Shop Now" CTA |
| `featured-products.liquid` | Grid of 6 featured products from the "All" collection |
| `custom-section.liquid` | Flexible section with optional background image and dynamic blocks |
| `product.liquid` | Product detail page: images, title, price, variants, quantity, add-to-cart |
| `collection.liquid` | Paginated collection grid (20 products per page) |
| `collections.liquid` | Collections overview grid with configurable width and gap |
| `blog.liquid` | Blog listing with article cards and pagination (5 per page) |
| `article.liquid` | Single blog post with content, metadata, comments, and comment form |
| `page.liquid` | Generic page content renderer |
| `cart.liquid` | Cart table with item management, quantity inputs, and checkout |
| `search.liquid` | Search results page with form and paginated results (20 per page) |
| `404.liquid` | 404 error page |
| `password.liquid` | Password-protected storefront landing page |

---

## Blocks

Blocks are smaller components that merchants can add, remove, and reorder inside sections via the theme editor.

| File | Description |
|------|-------------|
| `text.liquid` | Configurable text block with style (title/subtitle/normal) and alignment options |
| `group.liquid` | Layout wrapper block; arranges child blocks horizontally or vertically with configurable padding |

---

## Snippets

Snippets are code fragments included via `{% render %}`. They accept parameters but are not editable by merchants.

| File | Description |
|------|-------------|
| `image.liquid` | Responsive image with optional link wrapper. Parameters: `image`, `url`, `css_class`, `width`, `height`, `crop` |
| `meta-tags.liquid` | SEO meta tags, Open Graph, Twitter Card, canonical URL, and JSON-LD structured data for products |
| `css-variables.liquid` | Injects global CSS custom properties for fonts, colors, page width, and spacing |

---

## Templates

All templates are JSON files that wire sections to page types. Merchants can reorder and customize sections through the theme editor without code changes.

| Template | Page type |
|----------|-----------|
| `index.json` | Homepage (hero + featured products) |
| `product.json` | Product detail page |
| `collection.json` | Collection listing |
| `list-collections.json` | All collections overview |
| `blog.json` | Blog listing |
| `article.json` | Single blog post |
| `page.json` | Generic pages |
| `cart.json` | Shopping cart |
| `search.json` | Search results |
| `404.json` | Not found page |
| `password.json` | Password-protected store |

---

## Global theme settings

Configured in `config/settings_schema.json`, available to all sections and snippets via the `settings` object:

| Setting | Type | Description |
|---------|------|-------------|
| `type_primary_font` | font_picker | Primary typeface (default: Work Sans) |
| `max_page_width` | select | Page width: `90rem` (narrow) or `110rem` (wide) |
| `min_page_margin` | range | Horizontal page margin (10–100px, default 20px) |
| `background_color` | color | Site background color (default: `#FFFFFF`) |
| `foreground_color` | color | Site text color (default: `#333333`) |
| `input_corner_radius` | range | Input field border radius (0–10px, default 4px) |

---

## CSS & JavaScript

- **`assets/critical.css`** — Reset and base styles loaded on every page via `<link rel="preload">`.
- **`assets/clottius.min.css`** — Compiled Tailwind CSS output, loaded globally.
- **Component styles** — Each section, block, and snippet uses `{% stylesheet %}` tags for scoped CSS. The code is deduplicated at render time; no styles are loaded unless the component is on the page.
- **Component scripts** — Same pattern with `{% javascript %}` tags.

### CSS variable conventions

- **Single CSS property** → use an inline CSS variable:
  ```liquid
  <div style="--gap: {{ section.settings.gap }}px">
  ```
- **Multiple CSS properties** → use a CSS class:
  ```liquid
  <div class="group {{ block.settings.layout_direction }}">
  ```

---

## Translations

All user-facing strings use the `{{ 'key' | t }}` filter. English translations live in:

- `locales/en.default.json` — storefront strings
- `locales/en.default.schema.json` — theme editor UI strings

Keys follow a 3-level hierarchy: `section.group.key` (e.g., `cart.checkout`, `products.add_to_cart`).

---

## License

MIT
