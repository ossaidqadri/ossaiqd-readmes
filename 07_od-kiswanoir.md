<img src="https://socialify.git.ci/ossaidqadri/od-kiswanoir/image?custom_description=Production-ready+Shopify+theme+built+with+Tailwind+CSS%2C+Bun%2C+and+modern+development+tools.+&description=1&font=KoHo&language=1&name=1&owner=1&pattern=Charlie+Brown&stargazers=1&theme=Auto" />



<h1 align="center" style="position: relative;">
  <br>
    <img src="./assets/logo-kiswa-noir.svg" alt="logo" width="200">
  <br>
  Kiswa-Noir Shopify Theme
</h1>

A modern, carefully structured Shopify theme designed for e-commerce success. Built with modularity, maintainability, and Shopify's best practices in mind.
Based on Shopify's skeleton theme with customizations by OtherDev.

<p align="center">
  <a href="./LICENSE.md"><img src="https://img.shields.io/badge/License-MIT-green.svg" alt="License"></a>
</p>

## Getting started

### Prerequisites

Before starting, ensure you have the latest Shopify CLI installed:

- [Shopify CLI](https://shopify.dev/docs/api/shopify-cli) – helps you download, upload, preview themes, and streamline your workflows

If you use VS Code:

- [Shopify Liquid VS Code Extension](https://shopify.dev/docs/storefronts/themes/tools/shopify-liquid-vscode) – provides syntax highlighting, linting, inline documentation, and auto-completion specifically designed for Liquid templates

### Clone

Clone this repository using Git or Shopify CLI:

```bash
git clone git@github.com:your-organization/kiswa-noir.git
# or
shopify theme init
```

### Preview

Preview this theme using Shopify CLI:

```bash
shopify theme dev
```

## Kiswa-Noir Theme Architecture

The Kiswa-Noir theme is structured with modularity and performance in mind:

```bash
.
├── assets          # Stores compiled and static assets (CSS, JS, images, fonts, etc.)
├── blocks          # Reusable, nestable, customizable UI components
├── config          # Global theme settings and customization options
├── layout          # Top-level wrappers for pages (layout templates)
├── locales         # Translation files for theme internationalization
├── sections        # Modular full-width page components
├── snippets        # Reusable Liquid code or HTML fragments
├── src             # Source files (uncompiled CSS and JavaScript)
└── templates       # Templates combining sections to define page structures
```

To learn more, refer to the [theme architecture documentation](https://shopify.dev/docs/storefronts/themes/architecture).

### Templates

[Templates](https://shopify.dev/docs/storefronts/themes/architecture/templates#template-types) control what's rendered on each type of page in a theme.

The Kiswa-Noir Theme scaffolds [JSON templates](https://shopify.dev/docs/storefronts/themes/architecture/templates/json-templates) to make it easy for merchants to customize their store.

None of the template types are required, and not all of them are included in the Kiswa-Noir Theme. Refer to the [template types reference](https://shopify.dev/docs/storefronts/themes/architecture/templates#template-types) for a full list.

### Sections

[Sections](https://shopify.dev/docs/storefronts/themes/architecture/sections) are Liquid files that allow you to create reusable modules of content that can be customized by merchants. They can also include blocks which allow merchants to add, remove, and reorder content within a section.

Sections are made customizable by including a `{% schema %}` in the body. For more information, refer to the [section schema documentation](https://shopify.dev/docs/storefronts/themes/architecture/sections/section-schema).

### Blocks

[Blocks](https://shopify.dev/docs/storefronts/themes/architecture/blocks) let developers create flexible layouts by breaking down sections into smaller, reusable pieces of Liquid. Each block has its own set of settings, and can be added, removed, and reordered within a section.

Blocks are made customizable by including a `{% schema %}` in the body. For more information, refer to the [block schema documentation](https://shopify.dev/docs/storefronts/themes/architecture/blocks/theme-blocks/schema).

## Schemas

When developing components defined by schema settings, we recommend these guidelines to simplify your code:

- **Single property settings**: For settings that correspond to a single CSS property, use CSS variables:

  ```liquid
  <div class="collection" style="--gap: {{ block.settings.gap }}px">
    ...
  </div>

  {% stylesheet %}
    .collection {
      gap: var(--gap);
    }
  {% endstylesheet %}

  {% schema %}
  {
    "settings": [{
      "type": "range",
      "label": "gap",
      "id": "gap",
      "min": 0,
      "max": 100,
      "unit": "px",
      "default": 0,
    }]
  }
  {% endschema %}
  ```

- **Multiple property settings**: For settings that control multiple CSS properties, use CSS classes:

  ```liquid
  <div class="collection {{ block.settings.layout }}">
    ...
  </div>

  {% stylesheet %}
    .collection--full-width {
      /* multiple styles */
    }
    .collection--narrow {
      /* multiple styles */
    }
  {% endstylesheet %}

  {% schema %}
  {
    "settings": [{
      "type": "select",
      "id": "layout",
      "label": "layout",
      "values": [
        { "value": "collection--full-width", "label": "t:options.full" },
        { "value": "collection--narrow", "label": "t:options.narrow" }
      ]
    }]
  }
  {% endschema %}
  ```

## CSS & JavaScript

For CSS and JavaScript, we recommend using the [`{% stylesheet %}`](https://shopify.dev/docs/api/liquid/tags#stylesheet) and [`{% javascript %}`](https://shopify.dev/docs/api/liquid/tags/javascript) tags. They can be included multiple times, but the code will only appear once.

### Build Process

The Kiswa-Noir Theme uses modern development tools:

- **Tailwind CSS** for utility-first styling
- **Bun** as the JavaScript runtime for builds
- **Terser** for JavaScript minification
- **PostCSS** with Autoprefixer for CSS processing

### `critical.css`

The Kiswa-Noir Theme explicitly separates essential CSS necessary for every page into a dedicated `critical.css` file.

## Contributing

We're excited for your contributions to the Kiswa-Noir Theme! This repository aims to remain modern, performant, and a great foundation for e-commerce themes, and we kindly ask your contributions to align with this intention.

Visit our [CONTRIBUTING.md](./CONTRIBUTING.md) for a detailed overview of our process, guidelines, and recommendations.

## Key Features

The Kiswa-Noir Theme includes several modern e-commerce features:

### Performance Optimizations
- **Critical CSS inlining**: Essential CSS is inlined in the layout for faster initial render
- **Link prefetching**: Navigation links are prefetched on hover/touch for faster page transitions
- **Lazy loading**: Images and content are loaded as needed to improve performance
- **Resource hints**: DNS prefetching for faster external resource loading

### Modern Development Stack
- **Tailwind CSS**: Utility-first CSS framework with custom configuration
- **Bun runtime**: Fast JavaScript runtime for development and builds
- **Modular JavaScript**: Organized JS modules for header, lazy loading, and product functionality
- **PostCSS processing**: With Autoprefixer for cross-browser compatibility

### Responsive Design
- **CSS Grid layout**: Modern grid-based layout system
- **Mobile-first approach**: Design optimized for all device sizes
- **Flexible components**: Sections and blocks that adapt to different screen sizes

### Developer Experience
- **Component-based architecture**: Reusable sections and blocks
- **Schema-based customization**: Easy-to-use settings for merchants
- **Modern build tools**: Automated compilation and minification
- **Theme settings**: Comprehensive customization options

## License

Kiswa-Noir Theme is open-sourced under the [MIT](./LICENSE.md) License.
