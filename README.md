# Write Release Notes

A GitHub Action that uses an AI agent to accumulate release notes from
merged pull requests into a Markdown file.

The action runs on a schedule (or manually), reads the existing release
notes file, identifies merged PRs not yet documented, and uses
[cagent](https://github.com/docker/cagent) to categorize and add them.
You commit the result in a subsequent step.

## Quick start

```yaml
name: Update release notes
on:
  schedule:
    - cron: "0 9 * * 1" # Weekly on Monday
  workflow_dispatch:

permissions:
  contents: write
  pull-requests: read

jobs:
  update-notes:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4

      - uses: dvdksn/write-release-notes@main
        id: release-notes
        with:
          anthropic-api-key: ${{ secrets.ANTHROPIC_API_KEY }}
          team-members: "alice,bob,charlie"

      - name: Commit changes
        run: |
          git config user.name "github-actions[bot]"
          git config user.email "github-actions[bot]@users.noreply.github.com"
          git add ${{ steps.release-notes.outputs.output-file }}
          git diff --staged --quiet || git commit -m "Update release notes"
          git push
```

## Inputs

| Input                  | Default                       | Description                                     |
| ---------------------- | ----------------------------- | ----------------------------------------------- |
| `output-file`          | `release-notes/next.md`       | Path to the release notes file                  |
| `style-guide`          | _(built-in)_                  | Path to a custom style guide file               |
| `additional-prompt`    | _(empty)_                     | Extra instructions for the agent                |
| `include-contributors` | `true`                        | Credit external contributors                    |
| `exclude-labels`       | `skip-release,chore,internal` | Comma-separated labels to skip                  |
| `team-members`         | _(empty)_                     | Comma-separated GitHub usernames (not credited) |
| `anthropic-api-key`    |                               | Anthropic API key                               |
| `openai-api-key`       |                               | OpenAI API key                                  |
| `google-api-key`       |                               | Google AI API key                               |
| `github-token`         | `github.token`                | GitHub token                                    |
| `model`                | _(agent default)_             | Model override (e.g. `anthropic/claude-sonnet-4-6`) |
| `cagent-version`       | `v1.23.4`                     | cagent version                                  |
| `add-prompt-files`     | _(empty)_                     | Additional prompt files (comma-separated)       |

## Outputs

| Output        | Description                            |
| ------------- | -------------------------------------- |
| `exit-code`   | cagent exit code                       |
| `output-file` | Path to the updated release notes file |

## Baseline (`since` front matter)

The release notes file uses YAML front matter to define its scope:

```markdown
---
since: v1.2.0
---
```

The `since` field can be a git tag, commit SHA, or date. The agent
fetches merged PRs after this baseline and only documents those.

If there's no front matter, the agent infers how far back to go from
the existing content, or fetches a reasonable set of recent PRs.

## Custom style guide

By default, the action uses a built-in style guide that organizes
entries into **Breaking changes**, **What's new**, **Improvements**,
and **Bug fixes** sections.

To customize the format, create your own style guide and pass its path:

```yaml
- uses: dvdksn/write-release-notes@main
  with:
    style-guide: .github/release-notes-style.md
    anthropic-api-key: ${{ secrets.ANTHROPIC_API_KEY }}
```

See [`prompts/style-guide.md`](prompts/style-guide.md) for the default
style guide.

## Authentication

The action needs a GitHub token to fetch PRs. By default it uses the
workflow's `github.token`. Set `github-token` to override.

At least one AI provider API key is required (`anthropic-api-key`,
`openai-api-key`, or `google-api-key`).

## How it works

The action runs a [cagent](https://github.com/docker/cagent) agent
that:

1. Reads the existing release notes file (if any)
2. Determines the baseline from the `since` front matter
3. Fetches merged PRs since the baseline via `gh`
4. Determines which PRs are already documented
5. Skips PRs with excluded labels
6. Categorizes new PRs and adds them to the appropriate sections
7. Revises or removes entries when PRs are updated or reverted
8. Uses `gh pr view` / `gh pr diff` to research PRs when needed
9. Writes the updated file

The action does **not** commit or push. Add a commit step after the
action (see the quick start example).

## License

Apache License 2.0. See [LICENSE](LICENSE).
