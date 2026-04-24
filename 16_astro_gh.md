# Astro — CSS & View Transitions Contributions

**Repository:** https://github.com/ossaidqadri/astro (fork of `astro/astro`)

## My Contributions

Six commits fixing Astro's CSS handling during ClientRouter soft navigations, with focus on scoped style persistence and CSS cache plugin ordering.

### Commits

#### `77f227f` — Fix: persist Astro component styles during ClientRouter navigations

Fixed a bug where Astro component styles were being replaced with raw (pre-transform) CSS instead of the transformed version during ClientRouter `View Transitions`.

**Problem:** During client-side navigation, Vite injects `<style data-vite-dev-id="...">` elements. Astro's style swap logic was only preserving Vue scoped styles, not Astro's own scoped styles (`?astro&type=style&...lang.css`). This caused FOUC and incorrect styling.

**Fix:** Extended `vueScopedStyleId` → `devStyleId` logic to also match Astro component style patterns via regex: `/\?astro&type=style&.*lang\.css$/`

```typescript
// Before: only matched Vue scoped
const isVueScoped = url.searchParams.get('vue') !== null && ...;
// After: also matches Astro scoped
const isAstroStyle = /\?astro&type=style&.*lang\.css$/.test(viteDevId);
return isVueScoped || isAstroStyle ? viteDevId : '';
```

#### `0b9432a` — Refactor: extract CSS caching into separate plugin

Split `astroDevCssPlugin` into two plugins — the main plugin and a deferred `cssCachePlugin` that must be registered **after** integration plugins. This ensures integration transforms (e.g. UnoCSS's `@apply` directives) run before CSS is cached, so the cache contains fully-processed CSS.

**Root cause:** If caching runs before integration transforms, stale raw CSS gets cached and served during navigations.

#### `fce64ac` — Fix: guard cssCachePlugin push with `command === 'dev'`

The CSS cache plugin should only be added during `dev` mode. Added a guard to prevent it from being registered during production builds.

#### `1c7c27a` — Fix: support all Astro style preprocessor langs

Extended scoped style matching to cover Astro component styles across all preprocessor languages (Sass, Less, etc.), not just CSS.

#### `9f87533` — Refactor: rename `vueScopedStyleId` → `devStyleId`

Renamed the utility function to better reflect that it now handles both Vue and Astro scoped styles.

#### `f983897` — Fix: use `!` assertion instead of type cast for `result.plugins`

TypeScript fix using strict `!` non-null assertion instead of type cast (`as Plugin[]`) on `result.plugins`.

#### `8483851` — Docs: add fix for #16373 UnoCSS View Transitions issue

Documented the UnoCSS interaction in the changeset comment, explaining why the auto-persist logic applies to styles whose content is stable for a given component but may differ between raw HTML and HMR-applied versions.
