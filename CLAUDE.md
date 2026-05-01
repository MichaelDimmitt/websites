# Project Instructions

## Workflow

- Commit after every change
- Ensure we are on a new branch any time we go from having a clean state to a non-clean state
- Keep `Architecture.md` up to date whenever files or directories are added, removed, or renamed

## Website Versioning

Every change to `index.html` (the root hub page) requires a version bump.

### Hub page (`index.html`)

The hub has its own version dropdown in the header, driven by `releases.json` at the project root.

1. **Create a new versioned file** — Copy `index.html` → `index-vX.Y.html` (increment from the previous version)
2. **Update `index-master.html`** — Copy the new version over `index-master.html` and commit with message `index-master: vX.Y`
3. **Update `releases.json`** — Add a new entry at the top of the `releases` array:
   - `version`: New version number (e.g., "v0.3")
   - `file`: New filename (e.g., "index-v0.3.html")
   - `date`: Today's date (YYYY-MM-DD)
   - `description`: Brief description of what changed
   - `cheeky`: false (unless it's a playful/joke release)
4. **Never overwrite previous versions** — Old `index-vX.Y.html` files remain for rollback

### Tool subfolders

Individual tool pages (e.g., `portfolio-scorecard/index.html`) do not currently use the versioning system. If a tool accumulates meaningful changes over time, apply the same pattern: versioned snapshots + master file + releases.json inside the subfolder.

## Running Locally

```bash
npx serve .
# Visit http://localhost:3000
```
