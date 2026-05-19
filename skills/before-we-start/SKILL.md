---
name: before-we-start
description: Pre-session ritual — flush memory to disk, verify all plugins/skills active, show token usage, guide compact + clear
metadata:
  tags: session, memory, compact, housekeeping, plugins
---

## When to use

User types `/before-we-start` before starting a new task or when context is getting long.

## Steps — follow in order

---

### 1. Save memory

Scan the current conversation for anything worth persisting across sessions:
- New user preferences or working style discovered this session
- Key decisions made (architecture, naming, approach)
- Project state changes (new files added, models changed, features completed)
- Anything the user explicitly said to remember

Write each to the appropriate memory file using the memory format (frontmatter + body). Update `MEMORY.md` index.

If nothing new: say so in one line and move on.

---

### 2. Verify plugins + settings are enabled

Run this check (customize the `required` list for your own installed plugins):

```bash
python3 -c "
import json, sys, os
settings_path = os.path.expanduser('~/.claude/settings.json')
s = json.load(open(settings_path))
enabled = s.get('enabledPlugins', {})

# ── CUSTOMIZE THIS LIST ─────────────────────────────────────────────────────
# Add or remove plugin IDs to match the plugins you have installed.
# Format: 'plugin-name@marketplace-name'
required = [
  'superpowers@claude-plugins-official',
  'caveman@caveman',
  'smart-model-router@smart-model-router',
  'ui-ux-pro-max@ui-ux-pro-max-skill',
  'frontend-design@claude-plugins-official',
]
# ────────────────────────────────────────────────────────────────────────────

for p in required:
    status = '✓' if enabled.get(p) else '✗ MISSING'
    print(f'{status}  {p}')
"
```

Also check CLAUDE.md is present:

```bash
ls ~/.claude/CLAUDE.md && echo "✓  CLAUDE.md present" || echo "✗  CLAUDE.md MISSING"
```

Print the results as a checklist. If anything is missing, tell the user exactly what to run to fix it (e.g. `claude plugins install github:JuliusBrussee/caveman`).

---

### 3. Remind Claude what's available for the new session

After the checklist, print the active plugins block so the new session knows what to use.
Adjust this block to match your actual installed plugins:

```
ACTIVE PLUGINS FOR NEXT SESSION:
  superpowers     — brainstorming, TDD, debugging, writing-plans, executing-plans skills
  caveman         — terse mode (full), caveman-stats for token tracking
  smart-model-router — auto-routes sub-agents to haiku/sonnet/opus by task
  ui-ux-pro-max   — invoke for any UI/UX/design work (skill: ui-ux-pro-max:ui-ux-pro-max)
  frontend-design — invoke for frontend component design (skill: frontend-design:frontend-design)

CLAUDE.md loaded: yes (global behavioral guidelines active)
```

---

### 4. Show usage stats

Invoke:

```
Skill("caveman:caveman-stats")
```

Requires the [caveman](https://github.com/JuliusBrussee/caveman) plugin.

---

### 5. Guide user to compact + clear

Tell the user:

> Run `/compact` to snapshot this session.
> Then run `/clear` for a clean context window.
> Memory, compact summary, and all plugins carry over automatically.

Do NOT run these yourself — user-triggered UI commands only.

---

### Standing session rules (active for the rest of this session)

After invoking this skill, maintain these behaviors for the entire session:

**After any `git push` completes:** immediately say — "Feature pushed — run `/compact` now to trim context and save cost."

**When the user opens with new-task language** (e.g. "I want to build...", "Let's add...", "Next up...", "Can we create...", "Moving on to...", "New feature:..."): interrupt BEFORE diving in — say "Run `/clear` first to reset context and save cost. Then resend." Do not start planning or asking questions until they confirm or override.

**Context usage monitoring (active every response after this skill runs):**

On each response, invoke `Skill("caveman:caveman-stats")` silently to get current context usage %. Track the last-announced threshold in your working context. If usage has crossed a new threshold since your last check, prepend a banner **before** your response:

| Threshold crossed | Banner |
|---|---|
| 10% | `📊 Context: 10%` |
| 25% | `📊 Context: 25% — keep tasks focused` |
| 33% | `⚠️ Context: 33% — consider /compact after this task` |
| 50% | `⚠️ Context: 50% — run /compact when task ends` |
| 66% | `🟠 Context: 66% — run /compact soon` |
| 75% | `🟠 Context: 75% — run /compact now` |
| 90% | `🔴 Context: 90% — run /compact immediately` |
| 95% | `🚨 Context: 95% — run /before-we-start NOW, then /compact + /clear` |

Rules:
- Show banner only once per threshold (don't repeat same level on every response).
- If caveman-stats is unavailable, estimate from conversation length.
- At 95%, repeat the banner on every subsequent response until user acts.

---

### 6. Done

One line: what was saved to memory, plugin check result, stats shown.
