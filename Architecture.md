# File Architecture

```
websites/
│
├── Architecture.md                    ← This file
├── CLAUDE.md                          ← Project instructions and versioning workflow
├── releases.json                      ← Hub page version metadata (drives header version dropdown)
├── index.html                         ← Top-level hub: always the live version (copy of latest)
├── index-master.html                  ← Master index: always latest; git log -p shows full diff history
├── index-v0.1.html                    ← v0.1: initial hub page with tool cards
├── index-v0.2.html                    ← v0.2: version selector in header for hub page itself
├── CORS                               ← CORS config / notes (root-level)
├── chat-conversation.md               ← Misc conversation notes
│
├── apartment-request/
│   ├── index.html                     ← "Join the Apartment Slack!" — apartment request landing page
│   └── CORS
│
├── economic_networks/
│   ├── index.html                     ← Hub for both economic network tools
│   ├── economic_networks/
│   │   ├── index.html                 ← "Know Your Economic Landscape" — network visualization v1
│   │   ├── economic_networks.html     ← Underlying tool file
│   │   └── CORS
│   └── economic_networks2/
│       ├── index.html                 ← "Know Your Economic Landscape — City Networks" — v2
│       ├── economic_networks2.html    ← Underlying tool file
│       └── CORS
│
├── portfolio-scorecard/
│   ├── index.html                     ← Portfolio Scorecard tool
│   └── CORS
│
├── strategic-planning/
│   ├── index.html                     ← Strategic planning / planning horizons poster
│   ├── info.md
│   └── CORS
│
├── travel_aka_planning_readiness/
│   ├── index.html                     ← Hub for travel & planning tools (3 cards)
│   ├── chapter1_planning_horizons_poster_v8.html  ← "How Far Out Is Far Enough?" framework poster
│   ├── europe_itinerary_planner.html              ← Madrid/Lisbon/Barcelona/France day-by-day planner
│   ├── europe_trip_planning_poster.html           ← Barcelona/Madrid/France reference poster
│   ├── planning-readiness-conversation.md
│   └── CORS
│
└── ux_designer_scorecard/
    ├── index.html                     ← UX Designer Scorecard tool
    ├── MEMORY.md
    ├── CORS
    └── ux_competency_radar/
        └── competency-radar.html      ← UX competency radar chart
```

## Architecture Pattern

The site uses a **two-level hub-and-spoke** model with no build tools:

1. **Root hub (`index.html`)** — top-level page with a card for each subfolder. Each card links to that subfolder's `index.html`.
2. **Subfolder hub (`<folder>/index.html`)** — landing page for a topic area, linking to the actual tool HTML files within.
3. **Tool files** — single-file HTML applications (inline CSS/JS, CDN dependencies).

### Link Convention: Absolute Paths

All `href` values in hub pages use **absolute paths from the serve root** (e.g. `/travel_aka_planning_readiness/europe_itinerary_planner.html`), not relative paths.

**Why:** `npx serve` performs HTML extension rewriting — a relative link like `europe_itinerary_planner.html` gets rewritten to `/europe_itinerary_planner` (no folder prefix, no extension), which returns a 404. Absolute paths bypass this rewriting and resolve correctly regardless of which page initiates the navigation.

### Technology Stack

- Vanilla HTML/CSS/JS — no build step, no framework
- Fonts via Google Fonts CDN (IBM Plex Mono, Libre Baskerville, and others per tool)
- Dark green aesthetic: `#060e09` background, `#0c1a14` panels, `#e8ede6` text

### Running Locally

```bash
npx serve .
# Visit http://localhost:3000
```

## Hub Page Versioning

`index.html` is versioned using a snapshot pattern. The header contains a version dropdown driven by `releases.json`.

| File | Purpose |
|------|---------|
| `index.html` | Always the live version — copy of the latest snapshot |
| `index-master.html` | Master index; `git log -p index-master.html` is the full diff log |
| `index-vX.Y.html` | Immutable snapshots; never overwritten |
| `releases.json` | Version metadata; drives the header dropdown |

**To inspect what changed between versions:**

```bash
git log -p index-master.html
```

**Workflow when changing `index.html`:** see `CLAUDE.md` for the full 4-step process.
