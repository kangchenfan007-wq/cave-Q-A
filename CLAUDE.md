# CLAUDE.md — AI Assistant Guide for cave-Q-A

## Project Overview

**地心探秘：50题大挑战** (Cave Exploration Mystery: 50-Question Challenge) is a standalone, single-file interactive quiz game built with pure HTML5, CSS3, and vanilla JavaScript. No build tools, no dependencies, no frameworks.

- **Language:** Chinese (zh-CN)
- **Theme:** Cave geology / karst geoscience education
- **Format:** Browser-based game with 10 randomly selected questions per session

---

## Repository Structure

```
cave-Q-A/
├── index.html    # The entire application — HTML, CSS, and JS in one file
├── README.md     # Minimal description (2 lines)
└── CLAUDE.md     # This file
```

There are no subdirectories, no build artifacts, no configuration files, and no package manager files.

---

## Tech Stack

| Layer      | Technology                              |
|------------|-----------------------------------------|
| Markup     | HTML5                                   |
| Styling    | Inline CSS3 (glassmorphism, CSS vars)   |
| Logic      | Vanilla JavaScript (ES6, no libraries) |
| Fonts      | PingFang SC (system font, Chinese)      |
| Background | Remote image from Baidu CDN             |

**No Node.js, no npm, no TypeScript, no framework, no bundler.**

---

## How to Run

Open `index.html` directly in any modern browser — no server or build step required:

```bash
# Option 1: direct file open
open index.html

# Option 2: simple local server (optional, avoids some browser CORS quirks)
python3 -m http.server 8080
# then visit http://localhost:8080
```

---

## Application Architecture

All code lives in `index.html` in three sections:

### 1. HTML (lines 79–117)
Four top-level UI layers controlled via `display` toggling:

| Element ID        | Role                                         |
|-------------------|----------------------------------------------|
| `#start-screen`   | Nickname input — shown on load               |
| `#game-container` | Cave background with 10 clickable hotspots   |
| `#quiz-box`       | Fixed-position modal for the active question |
| `#result-screen`  | Final score and achievement title            |

### 2. CSS (lines 7–75)
- Uses CSS custom properties: `--accent`, `--correct`, `--wrong`, `--bg-glass`
- Dark theme: black background, cyan (`#00f2ff`) accent
- Glassmorphism cards with `backdrop-filter: blur`
- `.hotspot` elements use a `pulse` keyframe animation
- Mobile-first (`user-scalable=no`, percentage-based positioning)

### 3. JavaScript (lines 119–217)
Core globals:

| Variable          | Purpose                                            |
|-------------------|----------------------------------------------------|
| `fullPool`        | Array of all question objects (currently 10 of 50)|
| `activeQuizzes`   | 10 randomly shuffled questions for this session    |
| `score`           | Running correct-answer count                       |
| `answeredInSpot`  | Tracks how many hotspots have been visited         |

Key functions:

| Function        | Behavior                                                    |
|-----------------|-------------------------------------------------------------|
| `initGame()`    | Validates nickname, shuffles pool, wires hotspot click handlers |
| `openQuiz(idx, el)` | Renders question + options into `#quiz-box`           |
| `closeQuiz()`   | Hides modal, increments counter, calls `showFinal()` at 10 |
| `showFinal()`   | Calculates achievement tier, renders result screen          |

### Question Object Format

```js
{
  q: "Question text",               // string
  a: ["Option A", "Option B", "Option C"],  // array of 3 strings
  c: 0,                             // index of correct answer (0-based)
  info: "Educational explanation"   // shown after answering
}
```

### Achievement Tiers

| Score | Title (Chinese) | Icon | Color    |
|-------|-----------------|------|----------|
| 10    | 溶洞之神         | 👑   | `#ffd700` |
| 9     | 溶洞精灵         | 💎   | `#00ffcc` |
| 8     | 溶洞巫师         | 🔮   | `#bd00ff` |
| 5–7   | 溶洞土著         | 🪨   | `#ff7e00` |
| 3–4   | 溶洞新手         | 🔦   | `#4a90e2` |
| 0–2   | 溶洞白痴         | 🧊   | `#777`    |

---

## Development Conventions

### Editing Style
- All code is in a single file (`index.html`). Keep it that way unless there is a clear, agreed reason to split.
- CSS is in a `<style>` block in `<head>`. Do not use external stylesheets.
- JS is in a `<script>` block at the bottom of `<body>`. Do not use external scripts.
- Question content is in Chinese. Maintain that language for all in-game text.

### Extending the Question Pool
The `fullPool` array in `index.html` is designed to hold 50 questions. Currently only 10 are populated. To add more:
1. Append new objects to `fullPool` following the exact format above.
2. Keep `c` as a 0-based index into `a`.
3. Write `info` as a concise educational fact (1–2 sentences).
4. Do not change the shuffle logic in `initGame()` — the game always plays exactly 10 questions.

### Styling Changes
- Modify CSS variables (`:root`) to change the global color scheme.
- Hotspot positions are inline `style` attributes on `.hotspot` divs — percentage-based to stay responsive.
- Do not introduce CSS frameworks or utility libraries.

### No Build / No Tests
- There is no test suite. Manual browser testing is the only method.
- There is no linter or formatter configured. Keep code readable and consistent with the existing style.
- Do not add a build system unless explicitly requested.

---

## Git Workflow

- **Default branch:** `master`
- **Feature branches:** Use the `claude/` prefix convention (e.g., `claude/claude-md-mm3awufeg9qnwzt9-9S1QF`)
- **Remote:** `http://local_proxy@127.0.0.1:57426/git/kangchenfan007-wq/cave-Q-A`
- **Commit author:** Claude (noreply@anthropic.com)

Push commands:
```bash
git push -u origin <branch-name>
```

Branch names must start with `claude/` or pushes will fail with HTTP 403.

---

## Known Limitations / TODO

- `fullPool` only has 10 questions; the title promises 50. Add 40 more following the existing format.
- The cave background image is loaded from Baidu CDN — may fail outside mainland China. Consider a local fallback or alternative CDN.
- No error handling if `fullPool` has fewer than 10 questions.
- No accessibility (ARIA) attributes on interactive elements.
- Scores are not persisted — every reload starts fresh.
