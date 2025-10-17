# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

This is "8番通路" (Passage 8), a single-page browser-based horror/puzzle game inspired by "8番出口" (Exit 8). Players navigate a looping corridor and must identify visual/textual anomalies to progress from passage 0 to passage 8. The game features:

- **Normal Mode**: Shows feedback when returning to passage 0 after mistakes
- **Hard Mode** (`?hard`): No feedback on mistakes
- **Soft Mode** (`?soft`): Shows previous anomaly description even after correct detection
- **Debug Mode** (`?debug`): Developer tools for testing specific anomalies

## Development Commands

### Local Server
```bash
# Python
python3 -m http.server 8000

# Node.js
npx serve .
```

Access at `http://localhost:8000` (or port shown).

### Code Quality (Optional)
```bash
# Check formatting
npx prettier --check .

# Auto-format
npx prettier --write .

# Validate HTML
npx html-validate index.html
```

### Testing URLs
- Normal: `index.html`
- Hard mode: `index.html?hard`
- Soft mode: `index.html?soft`
- Debug mode: `index.html?debug`

## Architecture

### Single-File Structure
The entire game is contained in `index.html` with embedded CSS and JavaScript. This is intentional for simplicity and portability.

### Game State Management
- **currentPassage**: Current passage number (0-8)
- **hasAnomaly**: Boolean indicating if current passage has an anomaly
- **currentAnomaly**: Object containing `{ desc, class, hint }`
- **isFirstTurn**: First passage (0) always has no anomaly
- **shuffledAnomalies**: Randomized anomaly queue (refilled when empty)

### Anomaly System
Anomalies are defined in the `anomalies` array (lines 994-1132) with three types:

1. **Text Anomalies** (`class: ''`): Modifications to the description text
   - Examples: Punctuation changes, typos, missing/extra characters, kanji variants

2. **CSS Anomalies** (`class: 'anomaly-*'`): Visual changes applied via CSS classes
   - Applied to `#game-wrapper` or `body` element
   - Examples: Color changes, animations, position shifts, missing elements

3. **JS-Driven Anomalies**: Dynamic behavior requiring JavaScript
   - `anomaly-buttons-avoid-cursor`: Buttons repel cursor (lines 874-907)
   - `anomaly-dim-on-leave`: Screen dims when cursor leaves (lines 908-915)
   - `anomaly-cursor-trail`: Cursor leaves trailing dots (lines 916-932)
   - `anomaly-url-ganbare`: URL hash shows "がんばれー" (lines 933-935, 1349-1355)

### Visual Elements (Lines 64-168)
- `.ceiling-light`: Animated fluorescent light at top
- `.wall-sign`: Emergency exit sign (right side, 3D rotated)
- `.poster`: Safety poster (left side, 3D rotated)
- `.floor`: Tiled floor pattern
- `.puddle` / `.crack`: Additional anomaly objects (hidden by default)

### Game Flow
1. `setupNewPassage()` (lines 1378-1414): Initialize passage with 70% anomaly chance
2. `processTurn(decision)` (lines 1416-1470): Handle forward/back button clicks
3. `applyAnomaly(anomaly)` (lines 1322-1362): Apply anomaly without transition flash
4. `resetVisuals()` (lines 1279-1320): Clean up anomaly classes/styles
5. `clearGame()` (lines 1472-1614): Show success screen with stairs/light effect

### Transition Prevention
To avoid giving away anomalies through visual transitions:
- `.no-transition` class temporarily disables all transitions when applying/removing anomalies
- Applied to `#game-wrapper` and `body` before state changes
- Removed on next frame using `requestAnimationFrame`

### Randomness
Uses `crypto.getRandomValues()` for better unpredictability (lines 1135-1145), falling back to `Math.random()` if unavailable.

## Adding New Anomalies

1. **Add to `anomalies` array** (line 994):
   ```javascript
   {
     desc: normalDescription, // or modified text
     class: 'anomaly-new-feature', // or '' for text-only
     hint: '説明' // shown in debug and after mistakes
   }
   ```

2. **Add CSS** (if `class` is not empty):
   ```css
   .anomaly-new-feature .target-element {
     /* your styles */
   }

   /* For body-level effects */
   body.anomaly-new-feature {
     /* your styles */
   }
   ```

3. **Add JS logic** (if dynamic behavior needed):
   - Activate in `applyAnomaly()` (line 1322)
   - Clean up in `resetVisuals()` (line 1279)
   - See existing examples: `avoidCursorActive`, `dimOnLeaveActive`, `cursorTrailActive`

## Important Constraints

### Do Not Modify
- **Game balance**: 70% anomaly rate (line 1390)
- **Passage count**: 8 passages total
- **Transition suppression**: Critical for hiding anomaly changes
- **Debug mode indexing**: Anomaly array indices must remain stable

### Preserve
- **Japanese language**: All text content and comments
- **Single-file structure**: Keep everything in `index.html`
- **Crypto randomness**: Prefer `crypto.getRandomValues()` over `Math.random()`
- **Accessibility**: Keyboard navigation, semantic HTML

## Code Style

- **Indentation**: 4 spaces (JavaScript), aligned for readability
- **Naming**: `camelCase` for JS variables/functions
- **CSS**: Descriptive class names (`anomaly-*`, `no-transition`)
- **Comments**: Japanese comments are acceptable; add English for complex logic
- **Line length**: Keep reasonable (≤120 chars when practical)

## Testing Checklist

When modifying anomalies:
1. Test in normal mode (can you spot it?)
2. Test in debug mode (dropdown shows correct hint)
3. Verify no transition flash when applying/removing
4. Check that anomaly cleans up properly when navigating
5. Test on Chrome/Firefox/Edge
6. Verify keyboard navigation works (Tab to buttons, Enter to click)

## Git Workflow

- **Branch**: `main` (current)
- **Commit format**: Use conventional commits when appropriate
  - Example: `feat: add mirror text anomaly`
  - Example: `fix: prevent transition flash on anomaly change`
- Reference: See AGENTS.md for detailed commit guidelines
