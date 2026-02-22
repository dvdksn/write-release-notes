---
since: 601c1c1
---

This release adds a GitHub Actions workflow for automated release notes updates and improves the agent with persistent memory so it respects human edits across runs.

## What's new

- Add a GitHub Actions workflow for running the release notes agent on a nightly schedule or on demand. [#2](https://github.com/dvdksn/write-release-notes/pull/2)
- Add persistent memory using a cached SQLite database so the agent remembers human edits between runs and avoids re-adding intentionally removed entries. [#1](https://github.com/dvdksn/write-release-notes/pull/1)

## Contributors

Thank you to the following contributors:

@dvdksn
