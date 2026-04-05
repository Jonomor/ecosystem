# Schema Patterns — Jonomor Ecosystem

JSON-LD schema patterns used across all Jonomor properties to establish bidirectional entity relationships.

## @id Convention

Every entity in the ecosystem uses a consistent `@id` format:

```
https://www.{domain}/#{type-fragment}
```

Examples:
- `https://www.jonomor.com/#organization`
- `https://www.guard-clause.com/#method`
- `https://www.xrnotify.io/#app`
- `https://www.jonomor.com/ali-morgan#person`

The `www.` prefix is used consistently on all @id values. No exceptions.

## Bidirectional hasPart / isPartOf

Every property declares `isPartOf` pointing to Jonomor. Jonomor declares `hasPart` pointing to every property. This creates a bidirectional graph that AI systems can traverse in either direction.

### Child → Parent (isPartOf)

On every property's schema:

```json
{
  "@type": "SoftwareApplication",
  "@id": "https://www.guard-clause.com/#method",
  "name": "Guard-Clause",
  "isPartOf": {
    "@type": "Organization",
    "@id": "https://www.jonomor.com/#organization"
  }
}
```

### Parent → Child (hasPart)

On jonomor.com's Organization schema:

```json
{
  "@type": "Organization",
  "@id": "https://www.jonomor.com/#organization",
  "name": "Jonomor",
  "hasPart": [
    {
      "@type": "DefinedTermSet",
      "@id": "https://www.guard-clause.com/#method",
      "name": "Guard-Clause",
      "url": "https://www.guard-clause.com"
    },
    {
      "@type": "SoftwareApplication",
      "@id": "https://www.xrnotify.io/#app",
      "name": "XRNotify",
      "url": "https://www.xrnotify.io"
    }
  ]
}
```

## Creator / Publisher Pattern

Every property declares its creator (Ali Morgan) and publisher (Jonomor):

```json
{
  "creator": {
    "@type": "Person",
    "@id": "https://www.jonomor.com/ali-morgan#person"
  },
  "publisher": {
    "@type": "Organization",
    "@id": "https://www.jonomor.com/#organization"
  }
}
```

## Rules

- No `datePublished` or `dateModified` fields anywhere in the ecosystem
- `sameAs` only for real, verified profile URLs
- All schema is server-rendered in the root layout, never injected client-side
- Every nested object that is a schema entity must have `@type`
- Single `@context` at the graph root, not repeated in nested objects
