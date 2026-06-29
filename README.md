# explain-html

A Claude Code plugin that generates beautiful dark-theme HTML explanations and opens them in the browser — instead of dumping walls of text in the terminal.

## Install

```bash
claude plugin install gh:tetsuro/explain-html-plugin
```

## Usage

Invoke the skill directly:

```
/explain-html:explain-html explain how Redux works
```

Or Claude will use it automatically when:
- You say "explain X", "describe X", "walk me through X"
- Your response would fill 30+ terminal lines
- You ask to "show as HTML" or "open in browser"
- You're presenting an implementation plan in plan mode

## What it generates

A styled HTML file saved to `/tmp/claude-explain-<topic>.html`, opened in your default browser. Features:
- Dark GitHub-style theme
- Cards, numbered steps, callout boxes (note / warn / danger)
- Flow diagrams, comparison tables, tag badges
- Two-column grid layout
- Mermaid diagram support for complex graphs

## Versioning

Bump `"version"` in `.claude-plugin/plugin.json` and push to release a new version. Users get the update on next `claude plugin update`.
