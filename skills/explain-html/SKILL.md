---
name: explain-html
description: Generate a beautiful dark-theme HTML explanation and open it in the browser. Use this skill whenever: the user uses "explain" in their request (e.g., "explain this architecture", "explain how X works", "explain hogehoge"); you are presenting an implementation plan in plan mode; you are about to produce a long complex response that would fill 30+ terminal lines; or the user asks to "show as HTML", "open in browser", or "read as HTML". When in doubt — if structure, diagrams, or visual hierarchy would make the content clearer — use this skill instead of dumping text into the terminal.
---

## What this skill does

Instead of outputting a wall of text in the terminal, generate a styled dark-theme HTML file and open it in the browser. Complex plans, architectural explanations, and data-heavy descriptions are dramatically easier to understand with visual hierarchy, color-coded sections, and diagrams.

## When to use it

- User says "explain X", "describe X", "walk me through X", "what is X" — especially for non-trivial topics
- You're presenting an implementation plan (plan mode output)
- Your response would fill more than ~30 terminal lines
- User explicitly says "show as HTML", "open in browser", "read as HTML"
- Any explanation that has multiple components, steps, or relationships that benefit from visual structure

## How to generate

### 1. Write the HTML file

Save to `/tmp/claude-explain-<topic-slug>.html` where `<topic-slug>` is a short kebab-case description (e.g., `auth-flow`, `implementation-plan`, `redux-architecture`).

Use this base template:

```html
<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>TITLE HERE</title>
<style>
  :root {
    --bg: #0d1117;
    --surface: #161b22;
    --surface2: #21262d;
    --border: #30363d;
    --text: #e6edf3;
    --muted: #8b949e;
    --accent: #58a6ff;
    --green: #3fb950;
    --yellow: #d29922;
    --red: #f85149;
    --purple: #bc8cff;
    --orange: #ffa657;
  }
  * { box-sizing: border-box; margin: 0; padding: 0; }
  body {
    background: var(--bg);
    color: var(--text);
    font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', sans-serif;
    font-size: 15px;
    line-height: 1.6;
    padding: 2rem;
    max-width: 960px;
    margin: 0 auto;
  }
  h1 { font-size: 2rem; color: var(--accent); margin-bottom: 0.4rem; }
  h2 { font-size: 1.35rem; color: var(--text); margin: 2rem 0 0.75rem; border-bottom: 1px solid var(--border); padding-bottom: 0.4rem; }
  h3 { font-size: 1.05rem; color: var(--accent); margin: 1.25rem 0 0.5rem; }
  p { margin: 0.6rem 0; }
  a { color: var(--accent); }
  .meta { color: var(--muted); font-size: 0.85rem; margin-bottom: 2rem; }
  code {
    background: var(--surface2);
    border: 1px solid var(--border);
    border-radius: 4px;
    padding: 0.1em 0.4em;
    font-family: 'SF Mono', 'Fira Code', 'Cascadia Code', monospace;
    font-size: 0.88em;
    color: var(--orange);
  }
  pre {
    background: var(--surface);
    border: 1px solid var(--border);
    border-radius: 8px;
    padding: 1rem 1.25rem;
    overflow-x: auto;
    margin: 1rem 0;
  }
  pre code { background: none; border: none; padding: 0; font-size: 0.875rem; color: var(--text); }
  .card {
    background: var(--surface);
    border: 1px solid var(--border);
    border-radius: 8px;
    padding: 1.25rem;
    margin: 1rem 0;
  }
  .step {
    display: flex;
    gap: 1rem;
    margin: 0.75rem 0;
    align-items: flex-start;
  }
  .step-num {
    background: var(--accent);
    color: #000;
    border-radius: 50%;
    width: 1.6rem;
    height: 1.6rem;
    display: flex;
    align-items: center;
    justify-content: center;
    font-size: 0.8rem;
    font-weight: 700;
    flex-shrink: 0;
    margin-top: 0.1rem;
  }
  .note {
    background: rgba(88, 166, 255, 0.08);
    border-left: 3px solid var(--accent);
    border-radius: 0 6px 6px 0;
    padding: 0.75rem 1rem;
    margin: 1rem 0;
  }
  .warn {
    background: rgba(210, 153, 34, 0.08);
    border-left: 3px solid var(--yellow);
    border-radius: 0 6px 6px 0;
    padding: 0.75rem 1rem;
    margin: 1rem 0;
  }
  .danger {
    background: rgba(248, 81, 73, 0.08);
    border-left: 3px solid var(--red);
    border-radius: 0 6px 6px 0;
    padding: 0.75rem 1rem;
    margin: 1rem 0;
  }
  table { width: 100%; border-collapse: collapse; margin: 1rem 0; }
  th { background: var(--surface2); color: var(--accent); text-align: left; padding: 0.6rem 1rem; border: 1px solid var(--border); font-weight: 600; }
  td { padding: 0.6rem 1rem; border: 1px solid var(--border); vertical-align: top; }
  tr:nth-child(even) td { background: var(--surface); }
  ul, ol { padding-left: 1.5rem; margin: 0.5rem 0; }
  li { margin: 0.3rem 0; }
  .tag {
    display: inline-block;
    background: var(--surface2);
    border: 1px solid var(--border);
    border-radius: 12px;
    padding: 0.15em 0.7em;
    font-size: 0.78rem;
    color: var(--muted);
    margin: 0.15em;
  }
  .tag.green { border-color: var(--green); color: var(--green); }
  .tag.yellow { border-color: var(--yellow); color: var(--yellow); }
  .tag.red { border-color: var(--red); color: var(--red); }
  .tag.purple { border-color: var(--purple); color: var(--purple); }
  .flow {
    display: flex;
    align-items: center;
    flex-wrap: wrap;
    gap: 0.4rem;
    margin: 1rem 0;
  }
  .flow-box {
    background: var(--surface2);
    border: 1px solid var(--border);
    border-radius: 6px;
    padding: 0.35rem 0.8rem;
    font-size: 0.88rem;
  }
  .flow-arrow { color: var(--muted); font-size: 1.1rem; }
  .grid-2 { display: grid; grid-template-columns: 1fr 1fr; gap: 1rem; margin: 1rem 0; }
  @media (max-width: 600px) { .grid-2 { grid-template-columns: 1fr; } }
</style>
</head>
<body>

<!-- CONTENT GOES HERE -->

</body>
</html>
```

### 2. Pick the right visual elements

Think about what makes the content clearest, not just what fills the page:

- **`<div class="card">`** — group a concept or component
- **`<div class="step"><div class="step-num">1</div><div>content</div></div>`** — numbered steps in a process
- **`<div class="note">`** / **`<div class="warn">`** / **`<div class="danger">`** — callouts
- **`<div class="flow">...<div class="flow-box">A</div><div class="flow-arrow">→</div>...</div>`** — linear data flow
- **`<table>`** — comparisons, tradeoffs, option lists
- **`<span class="tag green">`** etc. — labels/badges with semantic color
- **`<div class="grid-2">`** — two-column layout for side-by-side comparison
- **`<pre><code>`** — code blocks

For complex graphs or diagrams (dependency trees, state machines, multi-node flows), add Mermaid just before `</body>`:
```html
<script src="https://cdn.jsdelivr.net/npm/mermaid/dist/mermaid.min.js"></script>
<script>mermaid.initialize({startOnLoad:true,theme:'dark'});</script>
```
Then use `<div class="mermaid">graph LR; A-->B; B-->C</div>` in the body.

### 3. Open in browser

```bash
xdg-open /tmp/claude-explain-<topic-slug>.html 2>/dev/null || open /tmp/claude-explain-<topic-slug>.html 2>/dev/null
```

### 4. Keep terminal output minimal

After opening, print one line in the terminal:
```
Opened in browser: /tmp/claude-explain-<topic-slug>.html
```

Don't repeat the explanation in the terminal — the HTML is the explanation. The terminal line is just confirmation.
