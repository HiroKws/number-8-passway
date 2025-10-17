# GEMINI.md

This file provides guidance to Gemini when working with code in this repository.

## Project Overview

This is "8番通路" (Passage 8), a single-page browser-based horror/puzzle game inspired by "8番出口" (Exit 8). The entire game is contained within `index.html`, which includes all necessary HTML, CSS, and JavaScript.

Players navigate a looping corridor and must identify visual or textual anomalies to progress from passage 0 to passage 8. If they spot an anomaly, they should turn back. If not, they should proceed forward. A mistake sends them back to passage 0.

The game features several modes accessible via URL query parameters:
- **Normal Mode**: Default mode.
- **Hard Mode** (`?hard`): No feedback on mistakes.
- **Soft Mode** (`?soft`): Shows the description of the previous anomaly even after a correct detection.
- **Debug Mode** (`?debug`): Provides developer tools for testing specific anomalies.

## Building and Running

This is a static web project with no build process.

### Local Server
You can run a local web server to play the game.

**Using Python:**
```bash
python3 -m http.server 8000
```

**Using Node.js:**
```bash
npx serve .
```
Access the game at `http://localhost:8000`.

### Testing URLs
- **Normal:** `index.html`
- **Hard Mode:** `index.html?hard`
- **Soft Mode:** `index.html?soft`
- **Debug Mode:** `index.html?debug`

## Development Conventions

### Architecture
- **Single-File Structure:** The entire game is intentionally kept within `index.html` for simplicity and portability. Do not create separate CSS or JS files.
- **Game State:** The core game state is managed by JavaScript variables like `currentPassage`, `hasAnomaly`, and `currentAnomaly`.
- **Anomalies:** Anomalies are defined in the `anomalies` array within the `<script>` tag.

### Adding New Anomalies
1.  **Add to `anomalies` array**: Add a new object to the `anomalies` array in `index.html`.
    ```javascript
    {
      desc: 'The description of the passage, possibly with a textual anomaly.',
      class: 'anomaly-css-class', // Or '' for text-only anomalies
      hint: 'A hint for what the anomaly is.'
    }
    ```
2.  **Add CSS (if needed)**: If you specified a `class`, add the corresponding CSS rules in the `<style>` section.
3.  **Add JavaScript (if needed)**: For dynamic anomalies, add the necessary logic in the `<script>` section and ensure it's activated in `applyAnomaly()` and cleaned up in `resetVisuals()`.

### Coding Style
- **Indentation**: Use 4 spaces for JavaScript and 2 spaces for HTML/CSS.
- **Naming**: Use `camelCase` for JavaScript variables and functions.
- **Comments**: Keep comments in Japanese, but add English for complex logic if necessary.
- **Commits**: Use the [Conventional Commits](https://www.conventionalcommits.org/) format (e.g., `feat:`, `fix:`, `docs:`).

### Important Constraints
- **Preserve Single-File Structure**: Do not split the code into multiple files.
- **Preserve Japanese Language**: All user-facing text and most comments should remain in Japanese.
- **No Frameworks**: Do not introduce any new frameworks or libraries without explicit instruction.
