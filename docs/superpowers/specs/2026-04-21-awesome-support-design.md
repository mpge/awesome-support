# awesome-support — Design

**Repo:** `mpge/awesome-support`
**Date:** 2026-04-21
**Status:** Approved design, pending implementation plan

## Purpose

A curated, multi-category awesome-list covering the full customer and IT support software ecosystem. Unlike most awesome-lists — which skew toward open-source only — this list explicitly includes commercial SaaS products alongside free and open-source tools, because in the support-software space the category leaders are mostly paid products and omitting them makes the list less useful for practitioners.

Inspired by [fatihok/awesome-support](https://github.com/fatihok/awesome-support) but broader in scope and more inclusive of paid tooling.

## Success criteria

- Repository exists at `github.com/mpge/awesome-support`, public.
- README lists at least 5 tools in each of the 23 categories at launch (~115+ entries total).
- Each entry is tagged so readers can see pricing model and deployment model at a glance without clicking through.
- Contribution process is documented and friction-free (issue template + PR template).
- Follows [awesome-list conventions](https://github.com/sindresorhus/awesome/blob/main/awesome.md) sufficiently to be eligible for listing on the main `awesome` index later.

## Non-goals

- Not a review site — no star ratings, no opinion pieces. Descriptions are factual and neutral.
- Not exhaustive on day one — seed content is a credible starting point; the long tail grows via PRs.
- Not a comparison tool — no feature matrices beyond the per-category highlights table.

## Structure

```
awesome-support/
├── README.md                          main curated list
├── CONTRIBUTING.md                    inclusion criteria + submission process
├── LICENSE                            CC0 1.0 Universal (awesome-list convention)
├── .github/
│   ├── ISSUE_TEMPLATE/
│   │   └── suggest-tool.yml           structured form for tool suggestions
│   └── pull_request_template.md       PR checklist for contributors
├── .gitignore
└── docs/
    └── superpowers/specs/
        └── 2026-04-21-awesome-support-design.md   this file
```

## README layout

1. **Header** — title, awesome badge, license badge, 2-3 sentence intro.
2. **Legend** — tag definitions.
3. **Table of contents** — generated, links to each category.
4. **Categories** (23 sections, see below). Each category contains:
   - One-sentence intro describing what the category covers.
   - **Notable tools** — markdown table with 3-5 most-used tools. Columns: Name · Description · Pricing · Open Source · Self-Hostable.
   - **All tools** — alphabetized bullet list with inline tag badges.
5. **Related lists** — links to adjacent awesome-lists.
6. **Contributing** — short section pointing to CONTRIBUTING.md.
7. **License** — CC0 notice.

## Entry format

**Highlights table row:**
```
| [Zendesk](https://zendesk.com) | Omnichannel ticketing platform. | Paid | No | No |
```

**Bullet entry:**
```
- [Zammad](https://zammad.org) — Web-based open-source helpdesk and customer support system. `Free` `Open Source` `Self-Hosted`
```

## Tag vocabulary

| Tag | Meaning |
|---|---|
| `Free` | Fully free with no paid tier that gates core functionality. |
| `Freemium` | Free tier exists but real use requires paying. |
| `Paid` | No meaningful free tier; trial only. |
| `Open Source` | Source available under an OSI-approved license. |
| `Self-Hosted` | Can be deployed on your own infrastructure. |
| `SaaS` | Vendor-hosted only. |
| `Enterprise` | Primarily sold to large organizations; typically sales-led, no self-serve pricing. |

Tags are additive: a tool can be `Open Source` + `Self-Hosted` + `Free`, or `Freemium` + `SaaS`, etc.

## Categories (23)

Organized into loose clusters for the TOC, but each is an independent section.

**Core channels & ticketing**
1. Ticketing & Shared Inbox
2. Live Chat & Messaging
3. Call Center / CCaaS
4. Social Media Support

**Self-service**
5. Knowledge Base & Documentation
6. Self-Service & Community Forums
7. Customer Portals
8. Status Pages

**Automation & AI**
9. AI Agents (autonomous support)
10. Chatbots & Conversational AI
11. Automation & Integration (support-glue)

**Proactive & in-product**
12. In-App Messaging & Product Tours
13. Customer Feedback / Voice of Customer
14. Customer Success Platforms

**Operations**
15. ITSM / Internal Service Desk
16. Workforce Management (support-specific)
17. Support Analytics & Reporting
18. Survey & CSAT

**Specialized**
19. Remote Support & Screen Sharing
20. Co-Browse
21. Developer Support Platforms
22. Translation & Localization for Support
23. Email Deliverability (support-relevant uses)

## Inclusion criteria (for CONTRIBUTING.md)

A tool qualifies if **all** of these hold:

- Primarily purpose-built for a support/CX/IT-support use case (not a generic tool that happens to be used for support).
- Actively maintained — last release, commit, or public product update within the past 18 months.
- Has a public website or repo with enough information to fill out the entry fields.
- Not duplicated in another entry under a different name.

Rejected: abandoned projects, pure white-label resellers of another listed tool, and tools that are only tangentially related to support.

## Contribution flow

1. Contributor opens an issue using the **Suggest a Tool** template (structured fields: name, URL, category, pricing model, OSS/self-hostable, one-line description).
2. Maintainer (you) reviews for fit against inclusion criteria.
3. If accepted, contributor (or maintainer) opens a PR adding the entry in alphabetical position, respecting the tag vocabulary.
4. PR template includes a checklist: correct category, alphabetical position, tags applied, description under ~15 words, link works.

## Seed content

At launch, each category is populated with 5-10 well-known tools drawn from general industry knowledge. Mix of commercial leaders and notable OSS projects. Not exhaustive — the goal is that a reader landing on any category sees a credible starting set and understands what belongs there.

## Creation & publishing

1. Scaffold locally at `C:\Users\work\awesome-support`.
2. Write all files listed under "Structure" above.
3. Initial commit on `main` branch.
4. Verify `mpge` GitHub org exists and user is authenticated via `gh auth status`.
5. `gh repo create mpge/awesome-support --public --source=. --push`.
6. Verify repo is live and README renders correctly.

## Open decisions deferred to implementation

- Exact seed tools per category (pick during implementation; constrained by inclusion criteria).
- Whether to add a "Related lists" section linking to adjacent awesome-lists (probably yes — convention).
- Whether to include an awesome-lint CI check (convention, but adds setup overhead — decide during implementation).
