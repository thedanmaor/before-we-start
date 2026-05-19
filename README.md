# before-we-start

A Claude Code plugin that runs a pre-session ritual before you start work — saves memory, checks your plugins are active, shows context usage, and reminds you to compact + clear before things get bloated.

Run it with `/before-we-start` any time you're about to start a new task or when your context is getting long.

---

## What it does

1. **Saves memory** — scans the conversation for anything worth keeping across sessions (decisions, preferences, project state) and writes it to your Claude memory files
2. **Checks plugins** — verifies your required plugins are enabled and flags anything missing with the exact install command to fix it
3. **Shows context usage** — displays token stats so you know where you stand
4. **Guides compact + clear** — tells you when to run `/compact` and `/clear` so you never lose work
5. **Monitors context automatically** — after running, warns you at 10/25/33/50/66/75/90/95% context usage on every response. At 95% it tells you to run `/before-we-start` NOW.

---

## Install

```bash
claude plugins install github:thedanmaor/before-we-start
```

Then run `/before-we-start` in any Claude Code session.

---

## Dependencies

This plugin works best alongside these plugins. Install each one:

### Required

**[caveman](https://github.com/JuliusBrussee/caveman)** — powers the `/caveman-stats` context usage display
```bash
claude plugins install github:JuliusBrussee/caveman
```

### Recommended

**[superpowers](https://github.com/obra/superpowers)** — brainstorming, TDD, debugging, planning, and code review skills
```bash
claude plugins install github:obra/superpowers
```

**[smart-model-router](https://github.com/maiha28781-cloud/smart-model-router)** — auto-routes sub-agents to haiku/sonnet/opus by task complexity (cuts costs)
```bash
claude plugins install github:maiha28781-cloud/smart-model-router
```

**[ui-ux-pro-max](https://github.com/nextlevelbuilder/ui-ux-pro-max-skill)** — UI/UX design intelligence (50+ styles, 161 palettes, 25 chart types)
```bash
claude plugins install git:https://github.com/nextlevelbuilder/ui-ux-pro-max-skill.git
```

**[frontend-design](https://github.com/anthropics/claude-plugins-official)** — production-grade frontend component skill
```bash
claude plugins install github:anthropics/claude-plugins-official
# then enable: claude plugins enable frontend-design@claude-plugins-official
```

---

## Customize

After install, the plugin check in the skill checks for the plugin IDs above by default. Open the SKILL.md and edit the `required` list to match your own setup:

```
~/.claude/plugins/cache/thedanmaor/before-we-start/<version>/skills/before-we-start/SKILL.md
```

The `required` list is clearly marked with a `CUSTOMIZE THIS LIST` comment.

---

## Usage

Just type `/before-we-start` in your Claude Code session. Claude will:

1. Save anything worth remembering from the current session
2. Run the plugin check and report results
3. Show your context usage stats
4. Tell you to run `/compact` then `/clear`

After that, Claude will proactively warn you as context fills up — no manual checks needed.

---

## License

MIT
