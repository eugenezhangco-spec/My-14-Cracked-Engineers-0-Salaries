---
description: "Hook system guidelines: lifecycle hooks, security hooks, session management"
---
# Hooks System

## Hook Types

- **PreToolUse**: Before tool execution (validation, parameter modification)
- **PostToolUse**: After tool execution (auto-format, checks)
- **Stop**: When session ends (final verification)

## Session Hooks (Installed)

- **SessionStart** (`.claude/hooks/session-start.sh`): Auto-loads `context/STATUS.md` + last-session flags into every new conversation. The team picks up where it left off.
- **Stop** (`.claude/hooks/session-stop.sh`): Saves session metadata when the session ends. Flags if STATUS.md was not updated (stale context warning for next session).

## Auto-Checkpoint (MANDATORY)

The team MUST save context to `context/STATUS.md` automatically at key moments. Do not wait for the user to ask. Do not skip this. Context loss between sessions is unacceptable for a founder who depends on continuity.

**Checkpoint triggers (auto-save STATUS.md when ANY of these happen):**
- After a PLAN is approved by the user
- After each completed build wave or major task
- After the QA chain produces a verdict (SHIP or HOLD)
- Before switching to a different feature or domain
- After any architectural decision
- When 20+ tool uses have passed since the last checkpoint
- When the conversation is approaching context compression

**What to save (update STATUS.md every time):**
- What was built or changed (summary with file names)
- Key decisions made and why
- Current pipeline position (where in the build flow)
- Open items or blockers
- Any unfinished work that should be resumed next session

**Format:** Update STATUS.md silently. Tell the user in one line: "Checkpointed." Then keep working. Do not make a production out of it.

**Why this matters:** If a session dies, the next session starts from the last checkpoint. The founder should never have to re-explain what was happening. STATUS.md is the team's memory between sessions — treat it like saving a game.

## Auto-Accept Permissions

Use with caution:
- Enable for trusted, well-defined plans
- Disable for exploratory work
- Never use dangerously-skip-permissions flag
- Configure `allowedTools` in `~/.claude.json` instead

## TodoWrite Best Practices

Use TodoWrite tool to:
- Track progress on multi-step tasks
- Verify understanding of instructions
- Enable real-time steering
- Show granular implementation steps

Todo list reveals:
- Out of order steps
- Missing items
- Extra unnecessary items
- Wrong granularity
- Misinterpreted requirements
