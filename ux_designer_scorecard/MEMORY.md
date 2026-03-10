# Project Memory

## Key Files
- `ux_designer_scorecard/index.html` — main scorecard app (React + Babel CDN, single file)
- `ux_designer_scorecard/ux_competency_radar/competency-radar.html` — D3 radar chart embedded via iframe

## Iframe Theme Sync (Important Gotcha)

The scorecard embeds the competency radar in an iframe and syncs the dark/light theme.

**The radar uses React state (`isDark`) to drive D3 rendering.** Setting `data-theme` directly on the iframe's `documentElement` only updates CSS variables — it does NOT update the React state, so the D3 chart colors stay wrong.

**Correct approach:** programmatically click the radar's `.theme-toggle` button. Even though it's hidden via injected CSS (`display: none !important`), `element.click()` still fires the event and triggers `setIsDark(!isDark)` in React.

```js
const radarTheme = frame.contentDocument.documentElement.getAttribute('data-theme') || 'dark';
if (radarTheme !== t) {
  const btn = frame.contentDocument.querySelector('.theme-toggle');
  if (btn) btn.click();
}
```

**Do NOT replace this with:** `frame.contentDocument.documentElement.setAttribute('data-theme', t)` — that breaks D3 rendering.
