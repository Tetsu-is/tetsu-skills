# tetsu-skills

A collection of Claude Code skills by tetsuro.

## Install

```bash
claude plugin install gh:Tetsu-is/tetsu-skills
```

## Skills

### explain-html

Generates beautiful dark-theme HTML explanations and opens them in the browser — instead of dumping walls of text in the terminal.

Invoked automatically when you say "explain X", your response would fill 30+ terminal lines, or you ask to "show as HTML" / "open in browser". Or invoke directly:

```
/tetsu-skills:explain-html explain how Redux works
```

## Versioning

Bump `"version"` in `.claude-plugin/plugin.json` and push to release. Users get the update on next `claude plugin update`.
