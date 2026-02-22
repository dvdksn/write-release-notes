# Release Notes Style Guide

Follow these conventions when writing and updating release notes.

## File structure

Use level-2 headings (`##`) for sections. Organize entries under the
following sections, in this order:

1. **Intro** — A brief summary paragraph at the top of the file (no heading).
   Summarize the theme of the release in one or two sentences. Update
   this when adding new entries to keep it current.
2. **Breaking changes** — Changes that require user action.
3. **What's new** — New features and capabilities.
4. **Improvements** — Enhancements to existing features.
5. **Bug fixes** — Resolved issues and corrections.

Only include sections that have entries. Don't create empty sections.

## Writing entries

Each entry is a Markdown list item. Use the following format:

```markdown
- Verb-first description of the change. [#123](repo-url/pull/123)
```

Rules:

- Start with a present-tense verb (Add, Fix, Improve, Update, Remove).
- Write in user-facing language, not technical jargon.
- Keep descriptions concise: one sentence, ideally under 120 characters.
- Link the PR number at the end of the entry.
- Use the repository URL provided in the configuration for links.
- Don't include the PR author in the entry itself.

## Sourcing entry content

When deciding what to write for an entry, use these sources in order
of preference:

1. **Release notes section in the PR body** — Many projects include a
   section like "Human readable description for the release notes" or
   "Release note" in their PR template. If present, use this as the
   basis for the entry. Rewrite it to match the style guide format
   (verb-first, concise), but preserve the author's intent.
2. **PR title and body** — If there's no release notes section, use
   the PR title and description to write the entry.
3. **PR diff** — If the title and body are too vague, use
   `gh pr view` or `gh pr diff` to understand what changed.

## Categorization

Use PR labels to help determine which section an entry belongs in. For example:

- Labels indicating a new feature or enhancement (e.g. `feature`,
  `enhancement`, `kind/feature`) → **What's new**
- Labels indicating an improvement or refactor (e.g. `improvement`,
  `performance`, `kind/refactor`) → **Improvements**
- Labels indicating a bug fix (e.g. `bug`, `fix`, `kind/bug`) →
  **Bug fixes**
- Labels indicating a breaking change (e.g. `breaking`,
  `breaking-change`, `impact/breaking`) → **Breaking changes**

If a PR has no labels or the labels don't clearly map to a section,
infer the category from the PR title and content. When in doubt, use
**Improvements**.

## Breaking changes

If migration steps are needed to handle a breaking change, note them as
indented sub-items:

```markdown
## Breaking changes

- Rename `config.oldKey` to `config.newKey`. [#789](repo-url/pull/789)
  - Update your configuration file: change `oldKey:` to `newKey:`.
```

## Contributors

When contributor credits are enabled, add a **Contributors** section at
the end of the file. List external contributors (non-team-members) who
authored merged PRs:

```markdown
## Contributors

Thank you to the following contributors:

@username1, @username2
```

Only add new contributors — don't remove existing ones. If the section
already exists, merge new names into the existing list.

## What to include

- User-facing changes (features, fixes, improvements).
- API changes, even if internal, when they affect integrations.
- Performance improvements with measurable impact.
- Breaking changes, always.

## What to omit

- Internal refactoring with no user-visible effect.
- Dependency bumps (unless they fix a known issue or add a feature).
- CI/CD changes.
- Documentation-only changes.
- PRs with excluded labels (handled by the action's filter).

## Formatting conventions

- Use standard Markdown (CommonMark).
- One blank line between sections.
- No trailing whitespace.
- End the file with a newline.
- Keep entries in chronological order within each section (newest last).
