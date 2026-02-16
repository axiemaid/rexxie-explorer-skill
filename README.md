# Rexxie Explorer Skill

An [OpenClaw](https://openclaw.ai) skill that lets your AI agent explore the **Rexxie** NFT collection on BSV — 2,222 unique dinosaurs minted via RelayX.

## How It Works

There's no traditional search bar. **Your agent IS the search engine.** Ask questions in natural language, and it queries the Rexxie API, reasons about traits, and builds a visual HTML page with the results.

### Example Queries

- "Show me all ninja rexxies"
- "Which address holds the most NFTs?"
- "Find rexxies with blue backgrounds and happy eyes"
- "How many different body traits are there?"
- "Show me cute rexxies" _(agent reasons about which traits look cute)_
- "List all burned NFTs"
- "Who owns Rexxie #42?"

### What Happens Behind the Scenes

1. You ask a question
2. Your agent queries the [public API](https://rexxie.axiemaid.com)
3. For vague queries ("blue bodies", "military-looking"), the agent reasons about which traits match
4. Results are rendered into a clean dark-theme HTML page
5. Page is presented in your browser — grid view, detail cards, tables, or lists

## Install

Just ask your OpenClaw agent:

> "Install the rexxie explorer skill"

Or via ClawHub CLI:

```bash
clawhub install rexxie-explorer
```

Or manually — clone into your workspace skills folder:

```bash
git clone https://github.com/axiemaid/rexxie-explorer-skill.git ~/.openclaw/workspace/skills/rexxie-explorer
```

## What's Included

| File | Purpose |
|---|---|
| `SKILL.md` | Agent instructions — how to query, reason, and render |
| `assets/template.html` | Lightweight HTML template (dark theme, 4 view types) |
| `references/api.md` | API endpoint documentation |
| `references/traits.md` | Full trait catalog (298 values) + fuzzy matching guide |

## API

The skill uses the public Rexxie API — no local server needed.

**Base URL:** https://rexxie.axiemaid.com

| Endpoint | Description |
|---|---|
| `/collection` | Collection metadata |
| `/nfts?page=1&limit=50` | Paginated NFT list |
| `/nft/:number` | Single NFT (1–2222) |
| `/owner/:address` | NFTs owned by address |
| `/traits` | Trait distribution |
| `/traits/:type/:value` | NFTs with a specific trait |
| `/search?q=...` | Search by number, address, or trait |
| `/stats` | Collection statistics |
| `/images/:number.png` | NFT image |

## View Types

The HTML template supports 4 views, chosen by the agent based on your query:

- **Grid** — gallery of NFT cards with images, traits, and owners
- **Detail** — deep dive on a single NFT with transfer history
- **Table** — stats, rankings, comparisons
- **List** — owner summaries, trait breakdowns

## Collection Info

- **Name:** Rexxie
- **Total:** 2,222 NFTs
- **Chain:** BSV (Bitcoin SV)
- **Protocol:** Run
- **Minted by:** RelayX
- **Traits:** 6 types — background, base, body, eye, mouth, head

## Links

- **Indexer:** [rexxie-indexer](https://github.com/axiemaid/rexxie-indexer) — backend that powers the API
- **OpenClaw:** [openclaw.ai](https://openclaw.ai)
