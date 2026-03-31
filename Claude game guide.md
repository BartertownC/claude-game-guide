---
name: game-guide
version: "1.1.0"
description: >
  Use when the user asks for help with a video game, including walkthroughs,
  boss strategies, puzzle solutions, collectibles, achievements, trophies,
  builds, loadouts, tier lists, character guides, game mechanics, hidden
  secrets, side quests, endings, new game plus tips, speedrun routes, or any
  in-game question. Also triggered by phrases like "how do I beat", "where do
  I find", "what is the best build for", "I am stuck in", "how does X work in",
  "what does Y do in", "show me the map", "what does X look like", "show me a
  screenshot", "show me the area", or when the user names a specific game and
  asks a question about it.
allowed-tools: WebSearch, WebFetch, ImageSearch
metadata:
  last-updated: "2026-03-31"
  target-users: gamers, casual and hardcore
  primary-sources: GameFAQs, Steam Community Guides, Image Search
  author: BartertownC
---

# Game Guide Skill

Comprehensive gaming assistant that fetches real, community-written guides,
walkthroughs, and strategies from GameFAQs and Steam Community Guides. Always
pulls live data — never relies on stale training knowledge for specifics.

---

## Source Priority

Always prefer sources in this order:

1. **Steam Community Guides** — best for PC games, builds, achievements, and
   mechanics written by verified players
2. **GameFAQs** — best for walkthroughs, collectibles, FAQs, and console games
3. **Web search fallback** — use if neither source has coverage

---

## Core Workflows

### Workflow 1 — Find a Guide or Walkthrough

When the user asks for a walkthrough, FAQ, or general help with a game:

**Step 1: Search Steam Guides**
```
WebSearch: site:steamcommunity.com/sharedfiles "GAME NAME" guide walkthrough
```
Then use WebFetch on the most relevant result URL to retrieve full guide content.

Steam guide URLs follow this pattern:
```
https://steamcommunity.com/sharedfiles/filedetails/?id=GUIDE_ID
```

**Step 2: Search GameFAQs**
```
WebSearch: site:gamefaqs.gamespot.com "GAME NAME" walkthrough FAQ
```
GameFAQs blocks direct page fetches, so use web search snippets and the
following URL patterns to construct direct links for the user:
```
https://gamefaqs.gamespot.com/{platform}/{game-slug}/faqs
https://gamefaqs.gamespot.com/{platform}/{game-slug}/faqs/{faq-id}
```

**Step 3: Deliver the answer**
- Extract the relevant section only — do not dump the entire guide
- Always cite which source and guide you used
- Flag spoilers before revealing story content with: **[SPOILER WARNING]**

---

### Workflow 2 — Boss Strategy

When the user says "how do I beat [boss]" or "I keep dying to [enemy]":

1. Search: `site:steamcommunity.com "GAME NAME" "BOSS NAME" guide strategy`
2. Search: `site:gamefaqs.gamespot.com "GAME NAME" "BOSS NAME"`
3. Extract: phase breakdown, attack patterns, recommended gear/level, counters
4. Format output as:

```
## [Boss Name] Strategy
**Recommended Level / Gear:** ...
**Phase 1:** ...
**Phase 2:** ...
**Key Tips:**
- ...
- ...
**Source:** [Guide title](URL)
```

---

### Workflow 3 — Build or Loadout

When the user asks for the best build, loadout, or character setup:

1. Search Steam Guides first for PC titles and live service games:
   `site:steamcommunity.com "GAME NAME" best build [class/character] guide`
2. Check if recent patches changed the meta:
   `WebSearch: "GAME NAME" patch notes 2026 build meta`
3. Format output as:

```
## [Class/Character] Build — [Playstyle]
**Stats to prioritize:** ...
**Core skills/perks:** ...
**Gear/weapons:** ...
**Playstyle notes:** ...
**Patch status:** [note if guide is pre/post a major patch]
**Source:** [Guide title](URL)
```

---

### Workflow 4 — Collectibles, Achievements, and Trophies

When the user asks about finding all items, 100% completion, or specific
achievements/trophies:

1. Search GameFAQs first — best for collectible guides:
   `site:gamefaqs.gamespot.com "GAME NAME" collectible achievement guide`
2. Search Steam for achievement guides:
   `site:steamcommunity.com "GAME NAME" "100%" OR achievement guide`
3. For a specific missable item, extract only that entry and give precise
   in-game directions (area name, coordinates if available, prerequisites)

---

### Workflow 5 — Quick Question

When the user asks a short factual question about a mechanic, item, or lore:

1. Search: `"GAME NAME" "QUESTION KEYWORD" site:steamcommunity.com OR site:gamefaqs.gamespot.com`
2. If not found, broaden: `WebSearch: "GAME NAME" QUESTION`
3. Answer concisely — one to three sentences with a source link

---

### Workflow 6 — Show Me a Map, Screenshot, or Visual

When the user asks to **see** something visual — a map, area, character, item,
boss, UI screen, or anything else — use ImageSearch directly. Do not try to
scrape screenshots from GameFAQs or Steam as those are unreliable.

**Triggers:** show me, what does X look like, show me the map, show me the
world map, what area is this, show me a screenshot of, show me the boss,
what does [item/character/weapon] look like

**Step 1 — Build a precise image search query**

Always include the game name and be as specific as possible:

| User asks | ImageSearch query |
|---|---|
| World map of FF7 | Final Fantasy 7 world map |
| What does Midgar look like | Final Fantasy 7 Midgar city screenshot |
| Show me the Margit boss | Elden Ring Margit the Fell Omen boss fight |
| Show me the Master Sword | Zelda Breath of the Wild Master Sword |
| Show me the inventory screen | [Game Name] inventory UI screenshot |

**Step 2 — Display 3 images inline**

After displaying them, add a one-line caption describing what the user is
looking at, noting the game and context.

**Step 3 — Pair with text if helpful**

If the image is a map and the user is navigating somewhere, follow up with
a brief text description of key locations or where to go next.

Example response format:

Here is the world map of Final Fantasy 7. The world opens up after you
leave Midgar. Key locations: Kalm (northeast), Junon (western coast),
Chocobo Farm (southeast). Want directions to a specific spot?

---

## Output Rules

- **Always cite sources** with the guide title and URL
- **Flag spoilers** before story or ending content — print [SPOILER WARNING]
  and ask the user to confirm before proceeding
- **Note patch dates** on builds — state if a guide may be outdated
- **Never fabricate** guide content — if neither source has an answer, say so
  and suggest the game's Steam discussion board or GameFAQs Q&A board
- **Keep answers focused** — extract the relevant section, not the whole guide
- If the game is **console-only**, skip Steam and go straight to GameFAQs

---

## URL Reference

**GameFAQs** (use web search to find, then construct direct links):

| Content | URL pattern |
|---|---|
| FAQ list | `gamefaqs.gamespot.com/{platform}/{slug}/faqs` |
| Specific FAQ | `gamefaqs.gamespot.com/{platform}/{slug}/faqs/{id}` |
| Cheats | `gamefaqs.gamespot.com/{platform}/{slug}/cheats` |
| Q&A | `gamefaqs.gamespot.com/{platform}/{slug}/answers` |
| Boards | `gamefaqs.gamespot.com/{platform}/{slug}/boards` |

Common platform slugs: `ps5`, `ps4`, `switch`, `xbox-series-x`, `pc`, `3ds`

**Steam Guides** (WebFetch works directly):

| Content | URL pattern |
|---|---|
| All guides for a game | `steamcommunity.com/app/{APP_ID}/guides/` |
| Specific guide | `steamcommunity.com/sharedfiles/filedetails/?id={GUIDE_ID}` |
| Discussions | `steamcommunity.com/app/{APP_ID}/discussions/` |

To find a Steam App ID: `WebSearch: "GAME NAME" steam app id`

---

## Fallback Behavior

If neither source has coverage (very new release or niche title):

1. Search Reddit: `site:reddit.com "GAME NAME" QUESTION`
2. Search the game wiki: `"GAME NAME" wiki QUESTION`
3. Tell the user which fallback source was used and note that community
   coverage may still be growing
