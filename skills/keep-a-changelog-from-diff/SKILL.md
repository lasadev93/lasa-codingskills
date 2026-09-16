---
name: keep-a-changelog-from-diff
description: Create or update changelog entries from git changes using the Keep a Changelog convention. Use when Codex must inspect `git diff`, staged changes, commits, or a release range and write a clear human-readable changelog entry under the standard sections Added, Changed, Deprecated, Removed, Fixed, and Security.
---

# Keep a Changelog From Diff

Inspect the repository changes before writing anything. Prefer `git diff`, `git diff --staged`, `git log`, and targeted file reads to understand the user-visible impact of the change. Do not infer changelog entries from filenames alone.

The changelog must be written in ITALIAN.

Write changelog entries using the Keep a Changelog convention. Produce human-readable release notes, not commit-message summaries. Focus on externally meaningful behavior, product-facing changes, developer-facing breaking changes, and fixes that matter to users or integrators.

Group entries under the standard sections only when they are justified by the diff:

- `Added` for new features or capabilities
- `Changed` for behavior changes, refactors with visible impact, or meaningful UX updates
- `Deprecated` for features or APIs that remain available but are being phased out
- `Removed` for deleted features, endpoints, options, or support
- `Fixed` for bug fixes and regressions
- `Security` for vulnerability fixes, hardening, permission changes, or sensitive validation improvements

Do not create empty sections. Do not force every diff into a section if no user-meaningful change exists. If the diff is purely internal and not changelog-worthy, say so explicitly.

Prefer concrete outcomes over implementation detail. Write “Added support for CSV export in the reports page” rather than “Added export handler and refactored report service.” Mention technical detail only when it changes integration behavior, migration requirements, configuration, or operational expectations.

When a change is breaking, make that explicit in the wording and call out the migration impact. If the repository already has a changelog format, match that format exactly unless the user asks for normalization.

When updating `CHANGELOG.md`, preserve the existing structure, heading levels, link style, and release ordering. Insert content into the correct unreleased or versioned section instead of rewriting unrelated history.

Use this workflow:
- Read the current changelog file if it exists.
- Inspect the relevant diff scope. Use the scope the user implies: working tree, staged changes, a commit, a branch comparison, or a release range.
- Extract only the changes that matter to users, consumers, operators, or developers integrating with the project.
- Map each meaningful change to the most appropriate Keep a Changelog section.
- Write short, specific entries in parallel style. Prefer one sentence per entry.
- If the diff leaves uncertainty about behavior, state the uncertainty and avoid overclaiming.

Apply these writing rules:
- Use past tense only if the repository already does so; otherwise prefer the concise imperative-neutral changelog style common in Keep a Changelog.
- Avoid low-signal wording such as “Updated various files”, “Minor fixes”, “Code cleanup”, or “Improved stuff”.
- Avoid raw commit hashes, PR numbers, stack traces, or internal filenames unless the repository convention explicitly includes them.
- Collapse duplicate low-level changes into one higher-level entry when they serve the same user-facing outcome.
- If several files contribute to one feature, write one changelog line for the feature, not one per file.
- Before finishing, verify that every entry is supported by the diff and that no entry describes work that did not actually happen.
- If needed, use the following decision heuristic:

If the user asks for direct file edits, update the changelog file. If the user asks only for suggested text, return the proposed entry without editing files.