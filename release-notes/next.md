---
since: v0.0.3
---

This release adds automated release notes generation via GitHub Actions, with persistent memory so the agent respects edits made by humans.

## What's new

- Add persistent memory using a SQLite database cached between runs, so the agent won't re-add entries that a human has intentionally removed. [#1](https://github.com/dvdksn/write-release-notes/pull/1)
- Add a nightly GitHub Actions workflow that automatically updates the release notes file and commits the result. [#2](https://github.com/dvdksn/write-release-notes/pull/2)

## Contributors

Thank you to the following contributors:

@dvdksn
