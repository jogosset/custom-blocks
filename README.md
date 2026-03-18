# Custom Block Library

A standalone block library for use with the [Adobe Demo Builder](https://github.com/skukla/demo-builder-vscode) VS Code extension. Blocks in this library can be installed into any EDS Commerce storefront during project creation or reset.

## Blocks

| Block | Description |
|-------|-------------|
| **Hero (JPG)** | Configurable hero banner with image, title, subtitle, CTA, and custom text color/button opacity |
| **Cards (JPG)** | Card grid with per-card image, title, body text, and customizable background/text colors |
| **Product Teaser (JPG)** | Commerce product teaser displaying product data from a SKU, with optional details and cart buttons |
| **Promotional Hero (JPG)** | Multi-card promotional layout with configurable columns, image ratios, content styles, and CTA buttons |
| **Account Dashboard (JPG)** | B2B account dashboard showing recent orders and purchase orders |
| **Age Gate (JPG)** | Date-of-birth age verification overlay with configurable minimum age and messaging |

## How to Use

### Add to Demo Builder

1. Open VS Code Settings
2. Search for `demoBuilder.blockLibraries.custom`
3. Add this repository URL:
   ```
   https://github.com/jogosset/custom-blocks
   ```
4. When creating or editing an EDS project, select this library in the Architecture Modal under "Custom Libraries"

### What Gets Installed

When selected, the Demo Builder:
- Copies all block files (`blocks/*/`) into the storefront repo
- Merges `component-definition.json` entries into the storefront's component definitions
- Merges `component-models.json` field definitions for DA.live editing
- Merges `component-filters.json` section allowlists
- Generates documentation pages in DA.live for each block (via `plugins.da.unsafeHTML`)

## Repository Structure

```
custom-blocks/
├── blocks/                              # Block source files
│   ├── hero-jg/
│   │   ├── hero-jg.js                  # Block JavaScript
│   │   ├── hero-jg.css                 # Block CSS
│   │   └── _hero-jg.json              # Block-level definition (DA.live fields)
│   ├── cards-jg/
│   ├── product-teaser-jg/
│   ├── promotional-hero-jg/
│   ├── account-dashboard-jg/
│   └── age-gate-jg/
├── component-definition.json            # All block definitions with unsafeHTML
├── component-models.json                # Field models for DA.live editing
└── component-filters.json               # Section allowlists
```

## Creating Your Own Block Library

To create a custom block library compatible with the Demo Builder:

### Required Files

**`blocks/` directory** — Each subdirectory is one block. The directory name is the block ID (lowercase, hyphen-separated).

**`component-definition.json`** — Registers blocks in the DA.live authoring library. Each block **must** include `plugins.da.unsafeHTML` to generate a documentation page — without it, the block will be installed but invisible in the DA.live authoring UI.

Example entry:
```json
{
  "title": "My Block (Initials)",
  "id": "my-block-initials",
  "plugins": {
    "da": {
      "unsafeHTML": "<div class=\"my-block-initials\"><div><h2>Title</h2><p>Description</p></div></div>"
    }
  }
}
```

The `unsafeHTML` represents one example instance of the block:
- The outermost `<div>` must have the block's class name (matching the block ID)
- Each row of the block table is a child `<div>`
- For key-value blocks: `<div><div>key</div><div>value</div></div>`

### Optional Files

**`component-models.json`** — Defines editable fields per block for the DA.live editing UI.

**`component-filters.json`** — Controls which blocks or sub-components can appear in sections.

### Naming Conventions

- Block IDs: lowercase, hyphen-separated, suffixed with your initials (e.g., `hero-jg`, `cards-jpg`)
- Titles: descriptive name followed by initials in parentheses (e.g., "Hero (JPG)")
- This prevents naming collisions with blocks from other libraries

### Per-Block Definition Files

Each block can optionally include a `_blockname.json` file with its own `definitions`, `models`, and `filters`. These are the newer AEM/EDS pattern for block-level metadata. Note that the Demo Builder currently reads metadata from the **top-level** `component-definition.json`, `component-models.json`, and `component-filters.json` — so ensure your block metadata is present in both locations.

## Reference

For complete documentation on custom block libraries, see:
[Custom Block Libraries — Demo Builder Docs](https://github.com/skukla/demo-builder-vscode/blob/develop/docs/systems/custom-block-libraries.md)
