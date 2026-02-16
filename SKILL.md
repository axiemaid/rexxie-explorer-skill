---
name: rexxie-explorer
description: Explore the Rexxie NFT collection (2,222 BSV NFTs). Use when users ask about Rexxie NFTs — browsing, searching by traits/owner/number, viewing ownership, trait analysis, or generating visual explorer pages. Handles natural language queries like "show me all blue rexxies", "which address holds the most NFTs", "find cute rexxies", etc. Agent-first: the LLM is the search engine.
---

# Rexxie Explorer

Agent-first NFT explorer for the Rexxie collection on BSV. You ARE the search bar — users ask natural language questions, you query the API, and render results as HTML.

## Setup

No local setup needed. The API is hosted at `https://rexxie.axiemaid.com`.
Test: `curl https://rexxie.axiemaid.com/health`

## Workflow

1. **Parse the query** — understand what the user wants (traits, owners, stats, specific NFTs)
2. **Query the API** — use `web_fetch` or `exec curl` against `https://rexxie.axiemaid.com` endpoints
3. **Reason about results** — for fuzzy queries, load `references/traits.md` and match trait names semantically
4. **Render HTML** — inject results into the template and present via canvas or write to a file

## API Reference

See `references/api.md` for all endpoints.

## Trait Reasoning

For vague/fuzzy queries, read `references/traits.md`. It contains all 298 trait values with counts and a fuzzy matching guide.

Examples:
- "blue bodies" → query multiple: Genesis Blue, Hybrid Blue, Imperial Blue, Stripped Blue, Dotted Blue
- "armor" → Obsidian Armor, Cyborg Armor, Exo Armor, Scout Armor, Knight Armor, etc.
- "cute rexxies" → combine Love/Innocent/Shy eyes + Grin/Smooch/Happy mouth
- "party rexxies" → Party Horn variants + Birthdat Hat + Clown Suit

Combine multiple `/traits/:type/:value` calls and intersect/union results as needed.

## Rendering Results

Use the HTML template at `assets/template.html`. Replace `__DATA__` with a JSON object.

### View Types

**Grid** — gallery of NFTs (default for browsing/search results):
```json
{
  "view": "grid",
  "title": "Blue Body Rexxies",
  "subtitle": "67 NFTs with Genesis Blue base",
  "stats": [{"value": "67", "label": "NFTs"}, {"value": "42", "label": "owners"}],
  "nfts": [{"number": 1, "image": "https://rexxie.axiemaid.com/images/1.png", "owner": "1Hej...", "traits": {"base": "Genesis Blue", "eye": "Happy"}, "burned": false}]
}
```

**Detail** — single NFT deep dive:
```json
{
  "view": "detail",
  "nft": {"number": 42, "image": "...", "owner": "1Abc...", "traits": {...}, "mintTxid": "...", "transfers": [...], "burned": false}
}
```

**Table** — stats, rankings, comparisons:
```json
{
  "view": "table",
  "title": "Top Holders",
  "headers": ["Address", "Count"],
  "rows": [["1Abc...", "45"], ["1Def...", "32"]]
}
```

**List** — owner lists, trait summaries:
```json
{
  "view": "list",
  "title": "Owners with Dragon bases",
  "items": [{"label": "1Abc...", "value": "12 NFTs", "image": "..."}]
}
```

### Rendering Steps

1. Read `assets/template.html`
2. Replace `__DATA__` with your JSON (use `JSON.stringify`)
3. Write to a temp file (e.g. workspace `rexxie-view.html`)
4. Present via `canvas` action=present with the file URL, OR tell user to open in browser

### Token Efficiency

- Do NOT generate HTML from scratch — always use the template
- For large result sets (>50 NFTs), paginate or show top results with a count
- Stats and summaries as table/list views are cheaper than full grids
- Only include traits in grid view when relevant to the query
