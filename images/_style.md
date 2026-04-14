# Shared visual style preamble

Every `.prompt.md` file in this directory starts by including this preamble verbatim. It ensures all generated images share a single visual identity, so the onboarding guide feels coherent rather than like a collage of unrelated diagrams.

## Canonical preamble

> **Style**: A clean, modern technical diagram in flat vector illustration style. Dark-mode-friendly neutral background (deep slate #1e293b or near-black #0f172a). Two accent colors only: soft blue (#60a5fa) for primary flow elements and warm amber (#fbbf24) for emphasis or callouts. Sans-serif typography with clear visual hierarchy — titles ~24pt, labels ~14pt, all text legible and crisp. Minimal line icons only (no photo-realistic or 3D elements). Left-to-right composition unless the content is inherently circular. Clear labeled arrows between steps. No decorative ornamentation, no gradients beyond subtle depth cues, no shadows except where they aid legibility. Consistent stroke width across all shapes. All diagram labels must be spelled exactly as written in quotes — image models misspell labels frequently, so any required text is double-anchored.

## Parameter defaults

When calling the `mcp-image` generator via the `image-generation` skill:

| Parameter | Default | Reason |
|-----------|---------|--------|
| `aspectRatio` | `16:9` | Fits inline in markdown without forcing scroll |
| `imageSize` | `2K` | Crisp in docs without being wasteful |
| `quality` | `balanced` | Final docs need coherence; drafts can use `fast` |

## Anti-patterns to avoid

- Photo-realism — these are technical diagrams, not product shots
- Busy backgrounds — every pixel the eye does not need is noise
- Unlabeled flows — every arrow means something; label it
- More than two accent colors — palette discipline is the whole point
- Mixed iconography — pick line icons and stick with them
- Text-heavy composition — if the diagram needs a paragraph, write a paragraph instead

## Regeneration

Every generated image has a sibling `.prompt.md` file containing the exact prompt used. To regenerate any image:

```
/image-generation <read .prompt.md and pass through verbatim>
```

See [images/README.md](README.md) for the full regeneration workflow and path-handling notes.
