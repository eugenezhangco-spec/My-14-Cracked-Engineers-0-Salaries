---
description: "Performance guidelines: model routing, cost optimization, caching strategies"
---
# Performance Optimization

## Model Routing (MANDATORY)

Model routing is now enforced per-agent. See `.claude/rules/agents.md` for the full routing table.

**Summary:**
- **Haiku** ($) — Documentation, simple lookups, formatting. Rachel's default.
- **Sonnet** ($$) — All coding, testing, reviews, builds, refactoring. Most agents' default.
- **Opus** ($$$) — Architecture, strategy, planning, audits, complex data pipeline design. Leadership + auditor.

**Rule:** When spawning sub-agents via the Agent tool, ALWAYS pass the `model` parameter matching the agent's tier. Do not run Opus for tasks Sonnet can handle. Do not run Sonnet for tasks Haiku can handle.

**Cost principle:** Under-provisioning (using a model too weak for the task) costs more than over-provisioning because you pay for the failed attempt AND the retry. When in doubt, use the agent's assigned tier.

## Context Window Management

Avoid last 20% of context window for:
- Large-scale refactoring
- Feature implementation spanning multiple files
- Debugging complex interactions

Lower context sensitivity tasks:
- Single-file edits
- Independent utility creation
- Documentation updates
- Simple bug fixes

## Extended Thinking + Plan Mode

Extended thinking is enabled by default, reserving up to 31,999 tokens for internal reasoning.

Control extended thinking via:
- **Toggle**: Option+T (macOS) / Alt+T (Windows/Linux)
- **Config**: Set `alwaysThinkingEnabled` in `~/.claude/settings.json`
- **Budget cap**: `export MAX_THINKING_TOKENS=10000`
- **Verbose mode**: Ctrl+O to see thinking output

For complex tasks requiring deep reasoning:
1. Ensure extended thinking is enabled (on by default)
2. Enable **Plan Mode** for structured approach
3. Use multiple critique rounds for thorough analysis
4. Use split role sub-agents for diverse perspectives

## Build Troubleshooting

If build fails:
1. Use **build-fixer** agent
2. Analyze error messages
3. Fix incrementally
4. Verify after each fix
