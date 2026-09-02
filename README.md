# Mobile Development Cycle Agents

This repository contains the Codex custom-agent profiles used by the native mobile development
cycle. The profiles are intentionally thin runtime adapters: they establish one lifecycle identity,
load one complete role skill, configure the role's sandbox and MCP access, and return control to the
root orchestrator.

## Authority and responsibility

The four layers have distinct owners:

| Layer | Owns |
| --- | --- |
| `MobileDevCycleAgents` TOML profiles | Lifecycle identity, sandbox mode, MCP configuration, and the one required role skill. |
| `MobileDevCycleSkills` | Reusable Swift/Kotlin architecture, implementation, review, state-machine, UI, concurrency, testing, evidence, and proportionality guidance. |
| Project `AGENTS.md` and sidecars | Product invariants, repository graph and owners, complexity classification, model/reasoning policy, local handoff schema, and repository-specific validation. |
| Root agent | Classification, deterministic role dispatch, lifecycle sequencing, handoff validation, correction routing, and user-facing completion. |

Do not move reusable engineering doctrine into these TOML files. Do not move project-specific policy
or model selection into them. A profile may contain a compact role anchor, but its corresponding
skill remains the complete reusable role contract.

## Profiles

| Profile | Required skill | Role |
| --- | --- | --- |
| `SwiftArchitect.toml` | `swift-architect` | Designs settled Swift architecture without implementing production code. |
| `SwiftDeveloper.toml` | `swift-developer` | Implements and validates a settled Swift design. |
| `SwiftReviewer.toml` | `swift-reviewer` | Independently reviews Swift changes and routes findings. |
| `KotlinArchitect.toml` | `kotlin-architect` | Designs settled Kotlin/Android architecture without implementing production code. |
| `KotlinDeveloper.toml` | `kotlin-developer` | Implements and validates a settled Kotlin/Android design. |
| `KotlinReviewer.toml` | `kotlin-reviewer` | Independently reviews Kotlin/Android changes and routes findings. |

Architect, Developer, and Reviewer profiles never select or spawn the next lifecycle role. The root
agent owns that transition and reuses the same role agent for corrections or re-review when the
project lifecycle requires it.

## Installation

Place or symlink the required TOML files into `~/.codex/agents/`, preserving their filenames. The
project lifecycle refers to the profile names without the `.toml` suffix, for example
`SwiftArchitect` or `KotlinReviewer`.

The profiles intentionally omit model and reasoning-effort settings. Each project lifecycle selects
those values from its own complexity policy when it invokes a role. Keep the profile's existing
sandbox and MCP configuration unless the runtime capability of that role deliberately changes.

## Maintenance rules

- Change reusable role behavior in `MobileDevCycleSkills`, then keep only a short role anchor here.
- Change sandbox, MCP, or custom-agent runtime behavior in this repository.
- Change product, repository, lifecycle, handoff, model, or validation policy in the affected
  project repository.
- Do not add a shared overlay skill merely to avoid a small amount of repetition between independently
  loadable role skills.
- After a profile change, verify that it loads exactly one corresponding role skill and still omits
  model selection and downstream orchestration.
