# Payload CMS — Lexical Editor & Relationship Batching Contributions

**Repository:** https://github.com/ossaidqadri/payload (fork of `payloadcms/payload`)

## My Contributions

### Commit `1424768044` — fix(richtext-lexical): prevent invalid `h0` heading creation when sizes disabled

Fixed a bug in the Markdown-to-heading transformer where pressing `Space` after `#` in the Lexical rich text editor would create an invalid `h0` heading when all heading sizes were configured as disabled in the field schema.

**Root cause:** The transformer created `HeadingTagType` from markdown shortcut match length (`match[1]?.length`) without checking if that heading level was in the enabled sizes list.

**Fix:** Validate heading level against `enabledSizes` before creating the node — if disabled, return `null` (falls through to paragraph):

```typescript
// Validate that the heading level is enabled before creating the node
const level = match[1]?.length
if (level === undefined || !enabledSizes.includes(level)) {
  return null  // stays as paragraph
}
const tag = ('h' + level) as HeadingTagType
```

**Files changed:** `packages/richtext-lexical/src/features/heading/markdownTransformer.ts` + new test collection `LexicalHeadingFeatureDisabled`

---

### Commit `a53ff13613` — fix(ui): address PR review comments on relationship batching

Addressed review feedback on the relationship batching PR, improving the batched fetch implementation.

---

### Commit `289a0ddbab` — perf(ui): batch relationship requests to prevent N+1 queries

**Problem:** The `RelationshipInput` component fired a separate `fetch()` per related collection when loading array fields with relationship data — classic N+1 query pattern causing poor performance with many related documents.

**Solution:** Introduced a global `RelationshipBatcher` utility that collects pending relationship lookups and batches them into a single request per collection, grouped by `relationTo`.

**Pattern change:**
```typescript
// Before: N fetches (one per collection)
for (const [relation, ids] of Object.entries(relationMap)) {
  await fetch(`/${relation}`, { body: qs.stringify({ where: { id: { in: ids } } } })  // ← N requests
}

// After: 1 batched fetch per collection
const batcher = getGlobalRelationshipBatcher({ apiRoute: api, locale, i18nLanguage })
await batcher.batchFetch(relationshipsToFetch)  // ← 1 request per unique collection
```

**Files changed:** `packages/ui/src/fields/Relationship/Input.tsx` (72-line reduction via batching util), new `RelationshipBatcher.js` utility
