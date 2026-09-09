# 🧭 Enough (enough)

> The model is enough. The wheels are enough. All it takes is a little guidance.

[中文版](README.md)

Enough is a Skill for AI coding assistants (Codex, Claude Code, OpenCode, and others). It uses risk-based routing, reuse-first decisions, and evidence-driven delivery to get a software task to “enough and shippable” — without coding on impulse or turning every small edit into a heavy process.

## Why does this exist?

If you regularly ask AI to build features, you have probably hit one of these:

**1. Wasted effort.** The AI writes a pile of code, then you find an existing project or module already solves the problem.

**2. Wrong direction, discovered too late.** The AI starts before clarifying the constraints that actually change the scope.

**3. Everything at once, nothing works.** The AI spreads across ten modules; on integration day, the main flow does not work at all.

**4. “Build succeeded” is not “it works”.** The AI says “all tests pass,” but only compilation or mocks passed; the real path crashes.

**5. Release incidents.** Secrets in code, temp files in the repository, debug settings left on — discovered after shipping.

**6. The same bug, forever.** The AI keeps guessing at one issue, fixing one break and causing another without stopping to find the cause.

**7. Process too heavy.** A typo gets research, design, and a full regression suite; time goes to ceremony instead of results.

**8. Tests for tests’ sake.** Brittle tests are added for “coverage” or mandatory TDD without proving a real risk.

Enough turns the judgment of a reliable engineer into a workflow an AI can follow: pause when needed, act when needed, and stop when the evidence is sufficient.

## Core philosophy

The model is capable enough, and the open-source ecosystem — including what is already in your project — is rich enough. Enough does not teach the AI how to write every line. At failure-prone points it asks the AI to **inspect existing capability first, choose a path by risk, search and design only when warranted, and accept work with proportionate evidence.**

| Question | Enough’s default |
| --- | --- |
| What comes first? | Inspect the current project’s modules, dependencies, interfaces, tests, and decisions |
| When do we search outside? | Only for a new project, an important dependency/capability choice, or a real adopt-vs-build tradeoff |
| When is design approval needed? | For complex work, new projects, important models/contracts, or security and data-boundary changes |
| How do we verify? | Separately decide what needs proof, whether current evidence is enough, whether a permanent test is worth it, and whether TDD helps |
| When do we stop? | When approved acceptance items have sufficient evidence, no blocker remains, and limits are disclosed |

## How it works

### Start with three risk tracks

Not every task follows the same sequence. Enough chooses a path from failure impact, blast radius, reversibility, uncertainty, and external-state impact; file type and task length alone do not decide it.

| Track | Suitable work | What happens | What is not the default |
| --- | --- | --- | --- |
| 🐇 **FAST** | Local, low-impact, reversible changes with clear acceptance | Inspect the change and context, implement, then run the nearest native check or smoke test | External evaluation, formal design, permanent new tests, a full review chain |
| 🏗️ **NORMAL** | Ordinary features or fixes with clear boundaries and controlled risk | State short acceptance and credible failure modes, reuse existing evidence, complete the affected real path, then independently accept it | Strict TDD, per-function tests, fixed dual review, full-stack regression |
| 🚨 **DEEP** | High-impact or hard-to-reverse work, uncertain key contracts, concurrency, auth/privacy, migrations, or external state | Strengthen design, exceptional-case evidence, controlled environments, and recovery for the specific risk | Unauthorized real operations or mechanically enabling every “advanced” step |

### Reuse first — not search for search’s sake

1. **Inspect what already exists.** Reuse modules, interfaces, dependencies, tests, and prior decisions that fit the current project.
2. **Search externally only for real selection.** For a new project, important new dependency, new custom capability, or a genuine mature-solution-vs-build choice, make a GitHub reuse decision: adopt, adopt and extend, borrow, or build.
3. **Do not block routine work on search.** Copy, small fixes, and local extensions of an existing implementation do not stall because GitHub is unavailable. If real selection research is incomplete, disclose that limit rather than presenting custom work as verified.

### Design, implementation, and acceptance happen when needed

Complex work first defines goals, non-goals, acceptance, boundaries, and failure behavior, then gets approval. Small, clear work can confirm its goal and acceptance in chat. For a real cross-boundary system, prove a minimal vertical slice first; do not manufacture a “whole-system path” for a small task.

The main agent owns risk, authorization, interface consistency, integration, and acceptance. A subagent’s “done” is not delivery: the main agent independently inspects the artifact and runs the selected verification. In delegated or cross-role work, subagents inherit the established strategy.

### Tests are not one fixed gate

Enough makes three separate decisions:

1. **Is current evidence enough?** Use tests, type/static checks, builds, startup, or real scenarios to prove observable results, invariants, and credible failures. Do not add a test when evidence is already sufficient.
2. **Is a permanent test worth adding?** Add one only when there is repeatable real risk, a stable observation boundary, an independent expectation, and ongoing regression value. Otherwise use a temporary diagnostic, smoke test, integration run, or repeatable manual step.
3. **Should tests come first?** Use TDD only when the user requests it, or when the contract is clear, counterexamples are stable, and test-first work brings a concrete constraint benefit. A behavior test added after implementation can still be valid.

Mocks, automated tests, successful builds, and real-environment verification are different evidence strengths; none substitutes for the others by name alone.

### High risk is not automatic execution authority

External writes, security, privacy, money, deployment, accounts, production data, and industrial devices are DEEP. Enough requires a clear target, authorization, irreversible impact, and remedy, then chooses proportionate safeguards such as least privilege, dry runs, state queries, controlled canaries, recovery, and exceptional-case verification. That is not permission to make a real call, release, or deployment.

## What problems does it solve?

| Pain point | How Enough addresses it | Mechanism |
| --- | --- | --- |
| Wasted effort | Inspect existing capability first; compare mature options only when real selection is needed | Reuse first |
| Wrong direction late | Approve only complex decisions that truly change scope | Design when needed |
| Everything at once, nothing works | Prove a minimal vertical slice when a real cross-boundary path exists | Core contracts and integration first |
| Fake tests, fake completion | Distinguish mocks, tests, builds, and real runs; reconcile each acceptance item | Evidence driven |
| Release incidents | Apply controlled verification and recovery to the specific DEEP risk | Risk-proportionate safeguards |
| The same bug forever | After two failures on one path, classify the root cause before retrying | Stop guessing |
| Process too heavy | FAST stays local; NORMAL and DEEP use only proportionate actions | FAST / NORMAL / DEEP |
| Tests for tests’ sake | Permanent tests and TDD are separate value decisions | Three verification decisions |

## Real cases: what it prevented

**A user wanted an RSS reader and wrote zero lines of code.**
A Windows-local RSS reader with multi-source subscriptions, keyword filtering, scheduled refresh, and one-click install is a real product-selection question. Enough’s reuse decision found that RSS Guard covered the core needs, so it recommended trying the mature project instead of building merely to write code. See the [RSS Guard reuse decision](examples/reuse-decision-rss-guard.md).

In another anonymized case, an “advanced journal” became a native Android, local-only app with system authentication. The reuse decision changed from “extend an existing project” to “borrow the data model and build natively.” Only then did work proceed through a minimal vertical slice and milestones; migration-test and documentation-write gaps remained explicitly visible in the acceptance record. See the [Mind Journal end-to-end case](examples/mind-journal-end-to-end.md).

More input → output examples are in [examples/](examples/). Materials marked v0.1 are retained as historical process records; current behavior is defined by this page and `skills/enough/SKILL.md`.

## Quick start

**One-command install (official GitHub CLI)**:

```bash
gh skill install zhizhixia/enough enough --agent codex --scope user
```

**Universal installer (skills.sh)**:

```bash
npx skills add zhizhixia/enough --yes
```

**Manual install (zero dependencies)**: copy `skills/enough/` into your client’s skills directory, keeping `SKILL.md`, `references/`, and `agents/`. For Codex, for example, use `~/.codex/skills/enough/`. Then restart the client or refresh its skills list as required.

> The standalone repository provides Enough’s core workflow and bundled references. It **does not** automatically install Hermes’s global natural-language routing rules or companion Skills such as brainstorming, verification-planning, and CodeGraph. Clients also differ in automatic triggering: when auto-routing is available, describe the software task naturally; when an explicit call is needed, use `Use $enough to proceed with risk-based, reuse-first, evidence-driven delivery: <task>`, or that client’s equivalent explicit Skill syntax.

If an optional companion capability is missing, Enough uses an equivalent simplified step instead of abandoning the task.

## Who is it for?

- Developers using AI for code who want it to be more reliable.
- Maintainers of existing projects who do not want every request to reinvent a wheel.
- Anyone working with real accounts, data, migrations, or external state who cannot accept “close enough.”
- Beginners who want good habits without a heavyweight process for every edit.

## How it compares to other methodologies

Enough is “risk-proportionate workflow guidance,” not a preset-gate system or a full project-management framework.

| Dimension | **Enough** | Superpowers | OpenSpec / Spec Kit |
| --- | --- | --- | --- |
| Positioning | Risk routing, reuse, and evidence decisions | Full methodology + skill system | Spec-driven development framework |
| Light work | FAST runs only the nearest necessary checks | Depends on installed workflows | Usually still has a spec process |
| External search | Only for real selection | Depends on the workflow | Not a core capability |
| Design approval | Only for complex design or key boundaries | Depends on the workflow | Specification centered |
| Tests | Evidence, permanent tests, and TDD decided separately | Depends on the workflow | Depends on project conventions |
| Dependencies | Plain Markdown; usable standalone | Host-tool skill ecosystem | CLI plus specification directory |
| Automatic trigger | Depends on the host client and installation | Depends on the host tool | CLI-command driven |

**Choose Enough when** you are an individual or small team across multiple clients and want the AI to judge risk and existing capability before delivering with enough evidence.

**Choose the alternatives when** you need team-scale specification management and cross-repository collaboration (OpenSpec / Spec Kit), or a complete methodology suite out of the box (Superpowers). They do not conflict; Enough can cooperate with existing capabilities but does not auto-install the rest of that ecosystem.

## License

MIT License. See [LICENSE](LICENSE).
