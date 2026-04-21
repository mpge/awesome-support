# Contributing

Thanks for helping grow Awesome Support. Please read this whole document before opening a pull request.

## Inclusion criteria

A tool qualifies if **all** of these hold:

- It is primarily purpose-built for a support, customer experience, or IT-support use case. Generic tools that happen to be used for support do not qualify.
- It is actively maintained: a public release, commit, or product update within the last 18 months.
- It has a public website or repository with enough information to complete the entry fields.
- It is not already listed under a different name.

We exclude abandoned projects, white-label resellers of an already-listed tool, and tools only tangentially related to support.

## How to suggest a tool

1. Open an issue using the **Suggest a Tool** template and fill in every field.
2. A maintainer will review the suggestion against the criteria above.
3. If accepted, open a pull request that adds the entry. You can also open the PR yourself at the same time as the issue.

## Entry format

Every entry must match this exact pattern:

    - [Name](https://example.com) - One-line description (pricing; deployment).

Rules:

- Description starts with a capital letter and ends with a period.
- Description is one sentence, roughly 10-20 words.
- `pricing` is one of: `free`, `freemium`, `paid`, `open source`. You may combine them, for example `free; open source`.
- `deployment` is one of: `SaaS`, `self-hosted`, or both separated by `/`.
- Entries are alphabetical within their section (case-insensitive).
- No emoji, no inline HTML, no badge images.

## Pull request checklist

- [ ] Entry is in the correct category section.
- [ ] Entry is in alphabetical position.
- [ ] Entry matches the required format exactly.
- [ ] Table of Contents is updated if you added a new section.
- [ ] `npx awesome-lint` passes locally.

## Running the linter locally

    npx awesome-lint

This requires Node.js 18 or later. The same command runs in CI on every push and pull request.
