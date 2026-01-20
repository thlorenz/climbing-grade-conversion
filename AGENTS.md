# 🧗 Climbing Grade Conversion - Agent Workflow Guide

## Project Overview

This is a **lightweight, zero-dependency Progressive Web App (PWA)** for converting climbing grades across 6 international grading systems. The entire application is contained in three files: `index.html` (app + all CSS/JS inlined), `sw.js` (service worker), and `manifest.json` (PWA config).

**Tech Stack:**
- Pure HTML, CSS, and vanilla JavaScript (no frameworks, no build tools)
- Service Worker for offline support and PWA functionality
- localStorage for persistent user preferences
- GitHub Pages deployment (gh-pages branch is main branch)

## Project Structure

```
climbing-table/
├── index.html           # Main app (table, controls, styling, logic all in one file)
├── sw.js                # Service worker (offline caching & PWA support)
├── manifest.json        # PWA manifest
├── README.md            # User documentation
└── AGENTS.md            # This file - agent workflow guide
```

## Core Features

✨ **Interactive Column Toggle** - Show/hide grading systems (Sport/French, British Trad, UIAA, YDS, Norway, Australia) with checkboxes
📱 **Responsive Design** - Works on desktop, tablet, mobile
🌙 **Dark Mode** - Toggle dark/light theme with CSS variables
📏 **Font Size Slider** - Adjust text size (12-24px range, default 14px)
💾 **Persistent Preferences** - All settings saved to localStorage (column visibility, font size, dark mode preference)
📲 **Progressive Web App** - Works offline, installable on phones/desktops
🚀 **Zero Dependencies** - Pure vanilla code, no external libraries

## Data Structure

### Climbing Grades Data (`gradesData` array in index.html)


The app contains 37 climbing grades with these properties:
- `sport` - Sport/French climbing grade
- `britTrad` - British traditional climbing grade
- `uiaa` - UIAA/Alpine grade
- `yds` - Yosemite Decimal System (USA)
- `norway` - Norwegian grade
- `australia` - Australian grade
- `category` - Skill level (Beginner, Experienced, Advanced, Expert, Elite)

**Default visible columns:** Sport, YDS, Category

### Column Configuration


Controlled by `columns` array in index.html with properties:
- `id` - Column identifier (matches data property)
- `name` - Display name for checkbox
- `visible` - Current visibility state (persisted to localStorage as `col-{id}`)

## Key Functions & Workflow

### Initialization

1. `loadSettings()` - Restores user preferences from localStorage (runs BEFORE rendering to avoid flashing)
2. `init()` - Renders table, controls, and attaches event listeners
3. `syncUIState()` - Ensures UI controls reflect loaded state

### Table Management

- `renderTable()` - Builds table rows from `gradesData`, applying visible columns and category styling
- `updateTableHeaders()` - Shows/hides headers based on column visibility

### Controls & Preferences

- `renderControls()` - Generates checkboxes for column toggling (skips Sport grade - always visible)
- `setFontSize(size)` - Updates font size CSS variable and localStorage
- `toggleDarkMode(enable)` - Toggles dark-mode class on body and localStorage
- Event listeners for checkboxes, dark mode button, and font size slider

### localStorage Keys

- `col-{columnId}` - Boolean (true/false) for column visibility
- `fontSize` - Integer (12-24)
- `darkMode` - Boolean (true/false)

## CSS Themes & Styling

### CSS Variables

- Light mode: `--bg-primary`, `--bg-secondary`, `--text-primary`, `--text-secondary`, `--border-color`, `--table-border`
- Dark mode: Same variables with darker colors
- Category row colors: `.grade-{category}` classes with light/dark variants

### Category Badges

- `.badge-{category}` - Colored pills showing skill level (Beginner=Green, Experienced=Orange, Advanced=Red, Expert=Purple, Elite=Dark)

## Deployment

**Hosted on GitHub Pages:** https://thlorenz.github.io/climbing-grade-conversion/
- Branch: `gh-pages` is the main deployment branch
- Service worker enables offline access after first visit
- PWA can be installed on mobile/desktop

## Service Worker Caching Strategy

**Cache Version:** Currently `climbing-v1` (defined in `sw.js` line 1)

**Caching Strategy:** Cache-first with network fallback
1. Check if request is in cache, serve if available
2. If not cached, fetch from network
3. Cache successful responses (200 status only)
4. If offline, serve cached version or error message

**Cached Assets:**
- `./` (root)
- `./index.html`
- `./manifest.json`
- `./sw.js`

## ⚠️ CRITICAL: Service Worker Version Management

**Problem:** Service workers cache aggressively. Users who have already visited the app will continue serving **stale cached content** after new commits unless the cache version is updated.

**Solution:** On each new commit/release, update the cache version in `sw.js` line 1:

```javascript
// Current (outdated example)
const CACHE_NAME = 'climbing-v1';

// After next commit, bump to:
const CACHE_NAME = 'climbing-v2';

// Version pattern: 'climbing-v{number}'
```

**When to update:**
- After ANY changes to `index.html`, `manifest.json`, or `sw.js`
- Before committing (increment the version number)
- Push to gh-pages branch → users get the new version on reload

**Why it matters:**
- Without version bumping, users see outdated features/fixes
- Service workers intercept ALL requests - stale cache = broken app for returning users
- Version bump forces browsers to invalidate old cache and fetch fresh assets

**Current version:** `climbing-v1` (in `sw.js` line 1)
**Next version to use:** `climbing-v2` (when making next commit)

## Git Workflow

**Recent commits** (newest first):
- `ebd8b20` - feat: move controls menu to top
- `fcd2d73` - feat: persist all settings to localStorage
- `6a24272` - feat: add dark mode toggle
- `1fe3ee7` - feat: add font size slider
- `3b7d963` - docs: add README.md
- `42e75fa` - feat: show only French, YDS, and Category by default
- `2180b40` - feat: allow hiding all columns
- `495e30f` - feat: convert to pwa
- `edc26ba` - feat: toggle visibility of climbing grades
- `bd95e27` - feat: climbing grade conversion tables (initial)

**Commit convention:** Use `feat:` for features, `fix:` for bugs, `docs:` for documentation, `refactor:` for code restructuring

## Testing & Verification

### Local Testing

```bash
# Start local server
python3 -m http.server 8000

# Visit http://localhost:8000
# Test features: column toggles, dark mode, font size, localStorage persistence
# Test offline: DevTools → Network → Offline → refresh → should still work
```

### After Making Changes

1. Verify table renders correctly with sample data
2. Check column toggles work and persist to localStorage
3. Verify dark mode styling applies correctly
4. Test font size slider works (12-24px range)
5. Test on mobile viewport (responsive design)
6. **BEFORE COMMITTING:** Update `CACHE_NAME` in `sw.js`
7. Commit and push to gh-pages branch

## Common Agent Tasks

### Adding a New Climbing Grade

1. Add row to `gradesData` array in index.html (maintain alphabetical or difficulty order)
2. Verify category is valid (Beginner, Experienced, Advanced, Expert, Elite)
3. Test table renders correctly
4. Bump service worker version in sw.js
5. Commit with `feat: add {grade name} grade`

### Modifying Column Visibility Defaults

1. Edit `columns` array `visible` property in index.html
2. Test that defaults apply on fresh browser (incognito mode)
3. Verify localStorage doesn't override defaults on first visit
4. Bump service worker version
5. Commit with `feat: update default columns`

### Adding New Features

1. Keep all code in index.html (no external files)
2. Use CSS variables for theming (dark mode compatibility)
3. Persist to localStorage if user preference-based
4. Load persisted state in `loadSettings()` BEFORE rendering
5. Sync UI state in `syncUIState()` after rendering
6. Test on mobile viewport
7. Bump service worker version
8. Commit with `feat: {feature name}`

### Fixing Bugs

1. Identify root cause in index.html, sw.js, or manifest.json
2. Make minimal changes
3. Test fix locally
4. Bump service worker version
5. Commit with `fix: {bug description}`

### Updating Documentation

1. Edit README.md or this AGENTS.md file
2. No need to bump service worker version (code unchanged)
3. Commit with `docs: {change description}`

## Important Notes

- **No build tools** - Any CSS/JS changes go directly in index.html
- **No external dependencies** - Keep it lightweight and zero-dependency
- **Dark mode** - All new styling must include `.dark-mode` variants
- **localStorage keys** - Document in this file if adding new preference types
- **Service worker** - Update version on EVERY meaningful commit to ensure users get updates
- **Testing** - Always verify in fresh/incognito window to test default behavior
