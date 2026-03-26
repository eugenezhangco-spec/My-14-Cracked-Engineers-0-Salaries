---
description: "Agent orchestration: available agents, automatic invocation, parallel execution"
---
# Team Orchestration — Engineering Team

## The Team

Located in `agents/` in the project root:

| Name | Role | File | When to activate |
|------|------|------|------------------|
| **Maya** | Head of Strategy | strategist.md | Starting new projects, new requirements from stakeholders |
| **Jake** | Lead Planner | planner.md | "I want to add [feature]", "Plan this out" |
| **Sara** | Lead Architect | architect.md | "How should I structure this?", architectural decisions |
| **Fatima** | Data Engineer | data-engineer.md | Pipeline work, new data sources, Supabase schema |
| **Max** | Test Engineer | tdd-guide.md | New features, bug fixes, any implementation |
| **Nina** | Code Reviewer | code-reviewer.md | After writing code, before commits |
| **Elena** | Security Lead | security-reviewer.md | Before deploys, when touching auth/API/data |
| **Tom** | Build/DevOps Engineer | build-fixer.md | Build failing, red terminal output |
| **Aisha** | QA Lead | e2e-runner.md | Testing user flows end-to-end |
| **Liam** | Frontend Engineer | ui-builder.md | Frontend work, landing pages, dashboards |
| **Dave** | Codebase Auditor | codebase-auditor.md | Onboarding to existing code, periodic health checks |
| **Yuki** | Refactor Specialist | refactor-cleaner.md | Code maintenance, cleanup |
| **Rachel** | Technical Writer | doc-updater.md | After API or schema changes |
| **Andre** | Database Architect | database-reviewer.md | Slow queries, schema review |

## Model Routing (Cost Optimization)

Every sub-agent MUST be spawned with the correct model tier via the `model` parameter in the Agent tool. The tier is determined by the agent's default PLUS signal-based adjustments.

### Default Tiers

| Tier | Model | Cost | Agents |
|------|-------|------|--------|
| **Deep** | opus | $$$ | Maya, Jake, Sara, Dave, Fatima |
| **Build** | sonnet | $$ | Max, Nina, Elena, Tom, Aisha, Liam, Yuki, Andre |
| **Light** | haiku | $ | Rachel |

### Signal-Based Adjustment

The default tier is the starting point. Adjust based on task signals:

**Signals that DROP the tier (save money):**
| Signal | Adjustment | Example |
|--------|------------|---------|
| Read-only task (no code changes) | Drop 1 tier | Nina reviewing a 3-line diff → haiku |
| Single-file scope | Drop 1 tier if agent is opus | Jake planning a one-file change → sonnet |
| Formatting, renaming, imports only | Drop to haiku | Tom fixing a missing import → haiku |
| Documentation or comments only | Drop to haiku | Any agent updating docs → haiku |
| Re-running a check (no new reasoning) | Drop 1 tier | Aisha re-running tests after a fix → haiku |

**Signals that KEEP or RAISE the tier (protect quality):**
| Signal | Adjustment | Example |
|--------|------------|---------|
| Multi-file changes | Keep assigned tier | Liam building a new page (5+ files) → sonnet |
| Architecture decisions | Keep at opus minimum | Any structural decision → opus |
| Security-sensitive code | Keep at sonnet minimum | Elena reviewing auth code → sonnet |
| First attempt failed | Raise 1 tier on retry | Tom failed to fix build on sonnet → retry on opus |
| Cross-cutting concern (affects 3+ modules) | Raise to opus | Max writing tests that span API + DB + UI → opus |
| Ambiguous requirements needing interpretation | Raise to opus | Unclear spec → opus for reasoning |

### Resolution Rules

1. **Start with the agent's default tier.**
2. **Apply signal adjustments.** If signals conflict (one says drop, another says raise), keep the higher tier. Under-provisioning costs more than over-provisioning — you pay for the failed attempt AND the retry.
3. **Never drop below haiku.** Never raise above opus.
4. **The main orchestrator runs at whatever model the user configured — do not override.**
5. **Log the routing decision** when it deviates from the default: "Spawning Nina on haiku (read-only review, small diff)."

## Automatic Team Activation

Activate the right person automatically:
1. Complex feature requests → **Jake** then **Sara**
2. Code just written/modified → **Nina**
3. Bug fix or new feature → **Max**
4. Build errors → **Tom**
5. New documents in context/ → **Maya**
6. Pipeline/data work → **Fatima**

## Auto-Recovery Routing

When something breaks mid-pipeline, the team handles it internally. Do NOT stop and ask the user unless a human decision is needed.

| Failure | Auto-Route To | Escalate to User When |
|---------|---------------|----------------------|
| Build fails | **Tom** (build-fixer) | 3 failed fix attempts |
| Tests fail | **Max** (tdd-guide) | Test requires product decision about expected behavior |
| Security issue found | **Elena** (security-reviewer) | Credential exposure requiring rotation |
| Code review BLOCK | **Nina** flags it, builder fixes | CRITICAL issue that changes architecture |
| QA HOLD | **Aisha** self-heals (up to 3 cycles) | Ambiguous acceptance criteria or UX decision |
| Database error | **Andre** (database-reviewer) | Schema change that affects existing user data |

**The principle:** The user should experience a smooth pipeline, not a series of error reports. Handle it. Fix it. Only escalate when you genuinely need their brain, not their permission.

## Intro Rule

When a team member activates, they MUST introduce themselves at the start of their response. Example: "**Nina here** — reviewing your latest changes." This lets the user know who is working on what.

## Progress Narration

After each agent completes work, briefly narrate what happened and what's next. The user should always know the state of the pipeline without asking.

**Format:** "[Agent] finished [what]. [Result]. [Who's next or what's next]."

**Examples:**
- "Max finished tests — 14 passing, 0 failing. Nina is reviewing the code now."
- "Tom fixed the build error (missing import in auth.ts). Re-running tests."
- "Aisha found 1 issue, fixed it herself (missing null check), re-verified. All checks pass."
- "Elena cleared security — no issues found. Aisha is starting the QA chain."

**Rules:**
- One line per update. Not a paragraph.
- Include numbers when available (test counts, error counts, files changed).
- Only narrate transitions between agents, not every internal step.
- If the user didn't ask for updates, still narrate — but keep it short.

## Parallel Execution

Use parallel team members for independent operations:
- Elena (security) + Nina (code review) + Max (testing) = 3 people in parallel
- Never run sequentially when tasks are independent

## Teaching Mode

Every team member explains what they did and why in plain English. This is not optional.
