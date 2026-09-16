# Lasa Coding Skills

A collection of 10 reusable skills for AI coding agents. The skills help turn an idea into a specification, an implementable issue, and a verified implementation while preserving traceability through to the final documentation.

The instructions are currently written primarily in Italian and are distributed in the `SKILL.md` format, which is compatible with agents that support the Agent Skills ecosystem. The skills may be translated into English in the future.

## Available skills

| Skill | Purpose |
| --- | --- |
| [`capture-intent`](skills/capture-intent/SKILL.md) | Turns rough notes and requirements into a stable Markdown intent without inventing requirements. |
| [`intent-to-spec`](skills/intent-to-spec/SKILL.md) | Turns an intent into a requirements and design specification that can be integrated into the codebase. |
| [`spec-to-issue`](skills/spec-to-issue/SKILL.md) | Prepares a GitHub issue proposal from a specification while preserving requirements and traceability. |
| [`implement-issue`](skills/implement-issue/SKILL.md) | Implements an issue in the existing codebase using a TDD workflow and verifies the acceptance criteria. |
| [`review-implementation`](skills/review-implementation/SKILL.md) | Reviews an implementation against the issue, specification, criteria, and codebase standards. |
| [`close-issue`](skills/close-issue/SKILL.md) | Prepares an aggregated Conventional Commit and issue-closing comments without performing remote operations. |
| [`branch-to-docs`](skills/branch-to-docs/SKILL.md) | Documents the result of a branch and, after confirmation, prepares a changelog compared with `main`. |
| [`keep-a-changelog-from-diff`](skills/keep-a-changelog-from-diff/SKILL.md) | Produces or updates an Italian changelog from diffs, commits, or release ranges. |
| [`validate-traceability`](skills/validate-traceability/SKILL.md) | Checks the consistency of the intent → specification → issue → implementation → documentation chain. |
| [`whats-next`](skills/whats-next/SKILL.md) | Interprets what has already been completed and identifies one next skill, including any prerequisites or blockers. |

## Recommended workflow

```text
capture-intent
      ↓
intent-to-spec
      ↓
spec-to-issue
      ↓
implement-issue
      ↓
review-implementation
      ↓
close-issue
      ↓
branch-to-docs
      ↓
validate-traceability
```

`whats-next` is an orientation skill: it can be used at any point in the workflow to identify the next step without automatically executing the suggested skill.

The workflow includes activities outside the skills themselves: manually publishing the issue on GitHub, creating the commit after implementation, and closing the issue after the result has been reviewed. Skills that produce documents or reports explicitly declare their paths and do not automatically modify issues, commits, or remotes.

## Prerequisites and conventions

- The skills have no runtime dependencies: they are Markdown instructions.
- Skills that inspect GitHub issues require GitHub CLI (`gh`) to be configured and authenticated.
- `review-implementation` and `branch-to-docs` work on an explicit Git scope; some checks require a local `main` branch.
- The workflow uses path conventions such as `docs/intents`, `docs/specs`, `docs/issues`, `docs/adr`, `dev/implementation`, `docs/documentation`, and `docs/changelog`.
- Before installing or running a skill, always read the relevant `SKILL.md` and review any commands the agent may execute.

## Disclaimer

These skills were created to support the author's personal workflow. They do not represent a universal process and may not be suitable for every team, codebase, agent, project, or organizational context.

Before using them, always read and adapt them to your conventions, tools, and security requirements. Pay particular attention to Git/GitHub commands, file paths, permissions, and the data the agent may read or modify. Use of the skills remains the user's responsibility: an agent's output should be reviewed by a person before it is applied, published, or used to make decisions.

The skills are provided without guarantees of correctness, completeness, currency, or fitness for a particular purpose. They do not replace code review, security assessment, legal advice, or other professional evaluation where needed.

## License

This project is distributed under the [BSD Zero Clause License (0BSD)](LICENSE). It is a permissive license with no requirement to retain attribution or the license text in redistributions, while excluding warranties and liability.
