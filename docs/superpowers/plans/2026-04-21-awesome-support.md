# awesome-support Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Publish `github.com/mpge/awesome-support` — a curated, awesome-lint-compliant list of support software (paid + free + OSS) across 23 categories.

**Architecture:** Static documentation repository. The deliverable is a `README.md` that passes `awesome-lint` plus the standard awesome-list supporting files (LICENSE, CONTRIBUTING.md, issue/PR templates) and a CI workflow that runs `awesome-lint` on every push and PR.

**Tech Stack:** Markdown, Git, GitHub, GitHub Actions, Node.js. Local commands use `pnpm` (already installed in this environment; `npm` is not on the Git Bash PATH). CI on Ubuntu uses `npx`.

**Working directory:** `C:\Users\work\awesome-support` (already initialized as a git repo on branch `main` with two commits containing the design spec).

**Note on "testing":** There is no runtime code. The acceptance test for every task that touches `README.md` is `npx awesome-lint` exiting with status 0. Run it after each content change; treat a lint failure the same as a failing unit test — fix before moving on.

**Note on awesome-lint constraints (IMPORTANT — read before Task 2):**
- Every list entry must match exactly: `- [Name](url) - Description.`
- Description must start with a capital letter and end with a period.
- No emoji in section headings or entries.
- No inline HTML badges inside list entries (the design's `Free` / `Paid` / `Open Source` tag idea must be expressed as plain-text suffixes inside the description, not as badge images or backticks).
- Entries must be alphabetical within each section (case-insensitive).
- Section headings must be in the Contents TOC and vice versa.
- The top-level heading must be `# Awesome Support` followed immediately by the awesome badge.
- Per the spec's deferred decision: the "highlights table" idea is **dropped** because awesome-lint rejects non-list content inside category sections. Pricing/OSS info lives inside the description text instead.

**Final description format:** `- [Name](url) - One-line description (pricing; deployment).`
Example: `- [Zendesk](https://www.zendesk.com) - Omnichannel ticketing and customer service platform (paid; SaaS).`

---

## File Structure

Files this plan creates, in order of creation:

| Path | Purpose |
|------|---------|
| `.gitignore` | Ignore `node_modules/` (dev-only dep for lint) and OS junk. |
| `LICENSE` | CC0 1.0 Universal full text. |
| `CONTRIBUTING.md` | Inclusion criteria, submission flow, entry format rules. |
| `.github/pull_request_template.md` | PR checklist for contributors. |
| `.github/ISSUE_TEMPLATE/suggest-tool.yml` | Structured issue form for tool suggestions. |
| `.github/workflows/lint.yml` | GitHub Actions workflow running `awesome-lint` on push/PR. |
| `README.md` | The curated list itself — grows across Tasks 2 and 5–10. |

All paths are relative to `C:\Users\work\awesome-support`.

---

## Task 1: Scaffold non-README repo files

**Files:**
- Create: `.gitignore`
- Create: `LICENSE`
- Create: `CONTRIBUTING.md`
- Create: `.github/pull_request_template.md`
- Create: `.github/ISSUE_TEMPLATE/suggest-tool.yml`
- Create: `.github/workflows/lint.yml`

- [ ] **Step 1: Create `.gitignore`**

```
node_modules/
.DS_Store
Thumbs.db
*.log
```

- [ ] **Step 2: Create `LICENSE` with CC0 1.0 Universal full text**

Paste the full CC0 1.0 text from https://creativecommons.org/publicdomain/zero/1.0/legalcode.txt. Do not shorten it — awesome-list convention and legal clarity both require the full legal text. The first line must be `Creative Commons Legal Code` and the last section is `CC0 1.0 Universal`.

- [ ] **Step 3: Create `CONTRIBUTING.md`**

```markdown
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
```

- [ ] **Step 4: Create `.github/pull_request_template.md`**

```markdown
## What does this PR do?

<!-- Brief summary: new tool, fix, new category, etc. -->

## Checklist

- [ ] I have read CONTRIBUTING.md.
- [ ] Each new entry matches the required format: `- [Name](url) - Description (pricing; deployment).`
- [ ] Entries are in alphabetical order within their section.
- [ ] If I added a new section, I updated the Contents table of contents.
- [ ] I ran `npx awesome-lint` locally and it passed.
- [ ] The tool meets the inclusion criteria in CONTRIBUTING.md.
```

- [ ] **Step 5: Create `.github/ISSUE_TEMPLATE/suggest-tool.yml`**

```yaml
name: Suggest a Tool
description: Suggest a support tool to add to the list.
title: "[Suggestion] <tool name>"
labels: ["suggestion"]
body:
  - type: input
    id: name
    attributes:
      label: Tool name
    validations:
      required: true
  - type: input
    id: url
    attributes:
      label: Official URL
      placeholder: https://example.com
    validations:
      required: true
  - type: dropdown
    id: category
    attributes:
      label: Proposed category
      options:
        - Ticketing & Shared Inbox
        - Live Chat & Messaging
        - Call Center / CCaaS
        - Social Media Support
        - Knowledge Base & Documentation
        - Self-Service & Community Forums
        - Customer Portals
        - Status Pages
        - AI Agents
        - Chatbots & Conversational AI
        - Automation & Integration
        - In-App Messaging & Product Tours
        - Customer Feedback / Voice of Customer
        - Customer Success Platforms
        - ITSM / Internal Service Desk
        - Workforce Management
        - Support Analytics & Reporting
        - Survey & CSAT
        - Remote Support & Screen Sharing
        - Co-Browse
        - Developer Support Platforms
        - Translation & Localization
        - Email Deliverability
    validations:
      required: true
  - type: dropdown
    id: pricing
    attributes:
      label: Pricing model
      options:
        - Free
        - Freemium
        - Paid
        - Open Source
    validations:
      required: true
  - type: dropdown
    id: deployment
    attributes:
      label: Deployment model
      options:
        - SaaS
        - Self-hosted
        - Both
    validations:
      required: true
  - type: textarea
    id: description
    attributes:
      label: One-line description
      description: One sentence, 10-20 words. Starts with a capital letter and ends with a period.
    validations:
      required: true
  - type: checkboxes
    id: criteria
    attributes:
      label: Inclusion criteria
      options:
        - label: The tool is primarily purpose-built for support / CX / IT-support.
          required: true
        - label: The tool is actively maintained (release, commit, or update in the last 18 months).
          required: true
        - label: The tool is not already listed under another name.
          required: true
```

- [ ] **Step 6: Create `.github/workflows/lint.yml`**

```yaml
name: Lint

on:
  push:
    branches: [main]
  pull_request:

jobs:
  awesome-lint:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - uses: actions/setup-node@v4
        with:
          node-version: 20
      - run: npx --yes awesome-lint
```

- [ ] **Step 7: Commit**

```bash
git add .gitignore LICENSE CONTRIBUTING.md .github/
git commit -m "Scaffold repo: license, contributing guide, templates, lint CI"
```

---

## Task 2: Create README skeleton that passes awesome-lint

**Files:**
- Create: `README.md`

This task produces an awesome-lint-compliant README with all 23 section headings present but empty under each heading (a single placeholder entry per section so `awesome-lint` is happy). Subsequent tasks replace the placeholder entries with real content.

- [ ] **Step 1: Write the README skeleton**

Write `README.md` with this exact content:

```markdown
# Awesome Support [![Awesome](https://awesome.re/badge.svg)](https://github.com/sindresorhus/awesome)

> A curated list of customer support, customer experience, and IT-support software.

Covers 23 categories across ticketing, live chat, knowledge bases, AI agents, ITSM, and more. Unlike most awesome-lists, commercial SaaS products are explicitly included alongside free and open-source tools, because in this space the category leaders are mostly paid.

## Contents

- [Ticketing and Shared Inbox](#ticketing-and-shared-inbox)
- [Live Chat and Messaging](#live-chat-and-messaging)
- [Call Center and CCaaS](#call-center-and-ccaas)
- [Social Media Support](#social-media-support)
- [Knowledge Base and Documentation](#knowledge-base-and-documentation)
- [Self-Service and Community Forums](#self-service-and-community-forums)
- [Customer Portals](#customer-portals)
- [Status Pages](#status-pages)
- [AI Agents](#ai-agents)
- [Chatbots and Conversational AI](#chatbots-and-conversational-ai)
- [Automation and Integration](#automation-and-integration)
- [In-App Messaging and Product Tours](#in-app-messaging-and-product-tours)
- [Customer Feedback and Voice of Customer](#customer-feedback-and-voice-of-customer)
- [Customer Success Platforms](#customer-success-platforms)
- [ITSM and Internal Service Desk](#itsm-and-internal-service-desk)
- [Workforce Management](#workforce-management)
- [Support Analytics and Reporting](#support-analytics-and-reporting)
- [Survey and CSAT](#survey-and-csat)
- [Remote Support and Screen Sharing](#remote-support-and-screen-sharing)
- [Co-Browse](#co-browse)
- [Developer Support Platforms](#developer-support-platforms)
- [Translation and Localization](#translation-and-localization)
- [Email Deliverability](#email-deliverability)
- [Related](#related)
- [Contributing](#contributing)

## Ticketing and Shared Inbox

- [Placeholder](https://example.com) - Temporary entry replaced in Task 4 (paid; SaaS).

## Live Chat and Messaging

- [Placeholder](https://example.com) - Temporary entry replaced in Task 4 (paid; SaaS).

## Call Center and CCaaS

- [Placeholder](https://example.com) - Temporary entry replaced in Task 4 (paid; SaaS).

## Social Media Support

- [Placeholder](https://example.com) - Temporary entry replaced in Task 4 (paid; SaaS).

## Knowledge Base and Documentation

- [Placeholder](https://example.com) - Temporary entry replaced in Task 5 (paid; SaaS).

## Self-Service and Community Forums

- [Placeholder](https://example.com) - Temporary entry replaced in Task 5 (paid; SaaS).

## Customer Portals

- [Placeholder](https://example.com) - Temporary entry replaced in Task 5 (paid; SaaS).

## Status Pages

- [Placeholder](https://example.com) - Temporary entry replaced in Task 5 (paid; SaaS).

## AI Agents

- [Placeholder](https://example.com) - Temporary entry replaced in Task 6 (paid; SaaS).

## Chatbots and Conversational AI

- [Placeholder](https://example.com) - Temporary entry replaced in Task 6 (paid; SaaS).

## Automation and Integration

- [Placeholder](https://example.com) - Temporary entry replaced in Task 6 (paid; SaaS).

## In-App Messaging and Product Tours

- [Placeholder](https://example.com) - Temporary entry replaced in Task 7 (paid; SaaS).

## Customer Feedback and Voice of Customer

- [Placeholder](https://example.com) - Temporary entry replaced in Task 7 (paid; SaaS).

## Customer Success Platforms

- [Placeholder](https://example.com) - Temporary entry replaced in Task 7 (paid; SaaS).

## ITSM and Internal Service Desk

- [Placeholder](https://example.com) - Temporary entry replaced in Task 8 (paid; SaaS).

## Workforce Management

- [Placeholder](https://example.com) - Temporary entry replaced in Task 8 (paid; SaaS).

## Support Analytics and Reporting

- [Placeholder](https://example.com) - Temporary entry replaced in Task 8 (paid; SaaS).

## Survey and CSAT

- [Placeholder](https://example.com) - Temporary entry replaced in Task 8 (paid; SaaS).

## Remote Support and Screen Sharing

- [Placeholder](https://example.com) - Temporary entry replaced in Task 9 (paid; SaaS).

## Co-Browse

- [Placeholder](https://example.com) - Temporary entry replaced in Task 9 (paid; SaaS).

## Developer Support Platforms

- [Placeholder](https://example.com) - Temporary entry replaced in Task 9 (paid; SaaS).

## Translation and Localization

- [Placeholder](https://example.com) - Temporary entry replaced in Task 9 (paid; SaaS).

## Email Deliverability

- [Placeholder](https://example.com) - Temporary entry replaced in Task 9 (paid; SaaS).

## Related

- [awesome-support](https://github.com/fatihok/awesome-support) - The original awesome-support list; more OSS-focused than this one.
- [awesome-selfhosted](https://github.com/awesome-selfhosted/awesome-selfhosted) - Broader self-hosted software list with a ticketing section.
- [awesome-customer-success](https://github.com/xopxe/awesome-customer-success) - Focused specifically on customer success.

## Contributing

Contributions are welcome. Read the [contribution guidelines](CONTRIBUTING.md) first.

## License

[![CC0](https://licensebuttons.net/p/zero/1.0/88x31.png)](https://creativecommons.org/publicdomain/zero/1.0/)

To the extent possible under law, [mpge](https://github.com/mpge) has waived all copyright and related or neighboring rights to this work.
```

- [ ] **Step 2: Run the linter to verify the skeleton passes**

```bash
cd /c/Users/work/awesome-support && pnpm dlx awesome-lint
```

Expected: exit code 0, output ends with `No problems found!` (or similar).

If lint complains about any of the Related links being dead (redirect, 404), replace the specific link with a working alternative and re-run. Do not disable lint rules.

- [ ] **Step 3: Commit**

```bash
git add README.md
git commit -m "Add awesome-lint-compliant README skeleton"
```

---

## Task 3: Configure awesome-lint as a local dev dependency

Using `pnpm dlx awesome-lint` works, but creating a `package.json` pins the version and makes local runs faster after the first install.

**Files:**
- Create: `package.json`

- [ ] **Step 1: Create `package.json`**

```json
{
  "name": "awesome-support",
  "private": true,
  "scripts": {
    "lint": "awesome-lint"
  },
  "devDependencies": {
    "awesome-lint": "^0.19.0"
  }
}
```

- [ ] **Step 2: Install and verify lint still passes**

```bash
pnpm install
ppnpm run lint
```

Expected: `pnpm install` completes (produces `pnpm-lock.yaml` and `node_modules/`); `ppnpm run lint` exits 0.

- [ ] **Step 3: Commit**

```bash
git add package.json pnpm-lock.yaml
git commit -m "Pin awesome-lint via package.json"
```

Note: `node_modules/` is already ignored from Task 1.

---

## Category population (Tasks 4–9)

Each of the following tasks replaces the Task-2 placeholder in a group of categories with alphabetized, lint-compliant seed entries. After each task, run `pnpm run lint` and commit only if it passes.

**Universal per-task workflow:**

1. Open `README.md`.
2. For each category in the task, delete the placeholder line and paste the new block below the category heading.
3. Run `pnpm run lint`. If it fails, read the error, fix the specific entry it points to (usually alphabetization, missing period, or lowercase start).
4. Commit with the suggested message.

---

## Task 4: Populate core-channel categories (Ticketing, Live Chat, Call Center, Social Media)

**Files:**
- Modify: `README.md`

- [ ] **Step 1: Replace the Ticketing and Shared Inbox placeholder**

Under `## Ticketing and Shared Inbox` replace the single placeholder line with:

```markdown
- [Freshdesk](https://freshdesk.com) - Cloud-based ticketing and omnichannel support suite from Freshworks (freemium; SaaS).
- [FreeScout](https://freescout.net) - Self-hosted Help Scout alternative with a shared-inbox workflow (free; open source; self-hosted).
- [Front](https://front.com) - Shared-inbox platform for customer-facing team email, SMS, and chat (paid; SaaS).
- [Help Scout](https://www.helpscout.com) - Shared inbox, knowledge base, and live chat aimed at small and mid-sized teams (paid; SaaS).
- [Hiver](https://hiverhq.com) - Turns Gmail into a shared helpdesk without leaving the inbox (paid; SaaS).
- [HubSpot Service Hub](https://www.hubspot.com/products/service) - Ticketing, knowledge base, and SLAs built on the HubSpot CRM (freemium; SaaS).
- [Intercom](https://www.intercom.com) - Messenger-first customer support and engagement platform with AI agents (paid; SaaS).
- [Kayako](https://kayako.com) - Unified ticketing across email, chat, and social with a customer-journey view (paid; SaaS).
- [osTicket](https://osticket.com) - Long-running open-source ticketing system for small teams (free; open source; self-hosted).
- [Trengo](https://trengo.com) - Shared inbox unifying WhatsApp, email, chat, and voice channels (paid; SaaS).
- [Zammad](https://zammad.org) - Modern open-source web-based helpdesk and customer-support system (free; open source; self-hosted).
- [Zendesk](https://www.zendesk.com) - Omnichannel ticketing and customer-service platform; the category incumbent (paid; SaaS).
```

- [ ] **Step 2: Replace the Live Chat and Messaging placeholder**

```markdown
- [Chatwoot](https://www.chatwoot.com) - Open-source customer-engagement suite with live chat, inbox, and bots (freemium; open source; self-hosted / SaaS).
- [Crisp](https://crisp.chat) - Live chat, shared inbox, and chatbot builder aimed at SMBs (freemium; SaaS).
- [Drift](https://www.drift.com) - Conversational marketing and sales chat, now part of Salesloft (paid; SaaS).
- [LiveChat](https://www.livechat.com) - Long-standing live-chat and helpdesk platform (paid; SaaS).
- [Olark](https://www.olark.com) - Simple live chat focused on ease of setup and accessibility features (paid; SaaS).
- [Tawk.to](https://www.tawk.to) - Free live chat and messaging with paid add-ons for AI and hired agents (free; SaaS).
- [Tidio](https://www.tidio.com) - Live chat plus chatbot and AI agents for small online stores (freemium; SaaS).
- [Zoho SalesIQ](https://www.zoho.com/salesiq/) - Live chat, visitor tracking, and chatbots in the Zoho suite (freemium; SaaS).
```

- [ ] **Step 3: Replace the Call Center and CCaaS placeholder**

```markdown
- [8x8 Contact Center](https://www.8x8.com/products/contact-center) - Cloud contact center with integrated UCaaS (paid; SaaS).
- [Aircall](https://aircall.io) - Cloud phone system with CRM integrations aimed at SMB support and sales teams (paid; SaaS).
- [Amazon Connect](https://aws.amazon.com/connect/) - Pay-as-you-go cloud contact center built on AWS (paid; SaaS).
- [Dialpad](https://www.dialpad.com) - AI-powered business phone and contact center (paid; SaaS).
- [Five9](https://www.five9.com) - Enterprise cloud contact center with digital and AI channels (paid; SaaS).
- [Genesys Cloud](https://www.genesys.com/genesys-cloud) - Enterprise CCaaS with routing, WFM, and AI (paid; SaaS).
- [NICE CXone](https://www.nice.com/products/cxone) - Enterprise contact center platform with analytics and WFO (paid; SaaS).
- [Talkdesk](https://www.talkdesk.com) - Cloud contact center with industry-specific AI agents (paid; SaaS).
```

- [ ] **Step 4: Replace the Social Media Support placeholder**

```markdown
- [Agorapulse](https://www.agorapulse.com) - Social media inbox, publishing, and reporting for mid-sized teams (paid; SaaS).
- [Brand24](https://brand24.com) - Social listening and brand-mention monitoring (paid; SaaS).
- [Hootsuite](https://www.hootsuite.com) - Multi-network social publishing and engagement inbox (paid; SaaS).
- [Khoros Care](https://khoros.com/platform/care) - Enterprise social customer-care platform (paid; SaaS).
- [Mention](https://mention.com) - Real-time brand and social monitoring tool (paid; SaaS).
- [Sprinklr Service](https://www.sprinklr.com/products/customer-service/) - Enterprise unified-CX platform with heavy social focus (paid; SaaS).
- [Sprout Social](https://sproutsocial.com) - Social engagement, publishing, and care platform (paid; SaaS).
```

- [ ] **Step 5: Run lint**

```bash
pnpm run lint
```

Expected: exit 0.

- [ ] **Step 6: Commit**

```bash
git add README.md
git commit -m "Seed core-channel categories (ticketing, chat, CCaaS, social)"
```

---

## Task 5: Populate self-service categories (Knowledge Base, Forums, Portals, Status Pages)

**Files:**
- Modify: `README.md`

- [ ] **Step 1: Replace the Knowledge Base and Documentation placeholder**

```markdown
- [BookStack](https://www.bookstackapp.com) - Self-hosted wiki-style platform organized into books and chapters (free; open source; self-hosted).
- [Confluence](https://www.atlassian.com/software/confluence) - Atlassian team workspace widely used for internal KBs (paid; SaaS / self-hosted).
- [Document360](https://document360.com) - Dedicated knowledge-base platform with analytics and AI search (paid; SaaS).
- [GitBook](https://www.gitbook.com) - Modern docs platform popular with developer-first teams (freemium; SaaS).
- [HelpDocs](https://www.helpdocs.io) - Lightweight knowledge-base tool focused on writing experience (paid; SaaS).
- [MadCap Flare](https://www.madcapsoftware.com/products/flare/) - Enterprise technical-authoring and single-sourcing platform (paid; self-hosted).
- [ReadMe](https://readme.com) - Developer-focused documentation with interactive API explorers (paid; SaaS).
- [Slab](https://slab.com) - Knowledge base focused on modern editing and integrations (paid; SaaS).
- [Stonly](https://stonly.com) - Interactive step-by-step guides and decision trees (paid; SaaS).
- [Wiki.js](https://js.wiki) - Modern open-source wiki engine (free; open source; self-hosted).
```

- [ ] **Step 2: Replace the Self-Service and Community Forums placeholder**

```markdown
- [Circle](https://circle.so) - Community platform increasingly used for customer communities (paid; SaaS).
- [Discourse](https://www.discourse.org) - Leading open-source discussion platform for customer communities (freemium; open source; self-hosted / SaaS).
- [Flarum](https://flarum.org) - Lightweight open-source forum software (free; open source; self-hosted).
- [Gainsight Customer Communities](https://www.gainsight.com/customer-communities/) - Community platform formerly known as inSided (paid; SaaS).
- [Higher Logic Vanilla](https://www.higherlogic.com/vanilla/) - Enterprise community platform formerly Vanilla Forums (paid; SaaS / self-hosted).
- [Khoros Communities](https://khoros.com/platform/communities) - Enterprise community platform (paid; SaaS).
- [NodeBB](https://nodebb.org) - Modern open-source forum software built on Node.js (free; open source; self-hosted).
```

- [ ] **Step 3: Replace the Customer Portals placeholder**

```markdown
- [Freshworks Customer Portal](https://www.freshworks.com/freshdesk/customer-portal/) - Branded self-service portal included with Freshdesk (paid; SaaS).
- [HubSpot Customer Portal](https://www.hubspot.com/products/service/customer-portal) - Portal tied to HubSpot tickets and knowledge base (paid; SaaS).
- [Salesforce Experience Cloud](https://www.salesforce.com/products/experience-cloud/) - Enterprise customer, partner, and employee portal platform (paid; SaaS).
- [Vtiger Customer Portal](https://www.vtiger.com/customer-portal-software/) - Self-service portal inside the Vtiger CRM (freemium; SaaS / self-hosted).
- [Zendesk Help Center](https://www.zendesk.com/service/help-center/) - Branded customer portal bundled with Zendesk Suite (paid; SaaS).
- [Zoho Customer Portal](https://www.zoho.com/desk/help-desk-customer-portal.html) - Customer self-service portal inside Zoho Desk (freemium; SaaS).
```

- [ ] **Step 4: Replace the Status Pages placeholder**

```markdown
- [BetterStack Status](https://betterstack.com/status-page) - Status pages bundled with incident management and uptime monitoring (freemium; SaaS).
- [Cachet](https://cachethq.io) - Self-hosted open-source status page system (free; open source; self-hosted).
- [Instatus](https://instatus.com) - Fast, minimalist hosted status pages (freemium; SaaS).
- [Oh Dear](https://ohdear.app) - Website monitoring bundled with hosted status pages (paid; SaaS).
- [Statusgator](https://statusgator.com) - Aggregates status pages from other vendors you depend on (freemium; SaaS).
- [Statuspage](https://www.atlassian.com/software/statuspage) - Atlassian hosted status pages; widely used incumbent (paid; SaaS).
- [UptimeRobot Status Pages](https://uptimerobot.com/status-page/) - Hosted status pages bundled with UptimeRobot monitoring (freemium; SaaS).
```

- [ ] **Step 5: Run lint and commit**

```bash
pnpm run lint
git add README.md
git commit -m "Seed self-service categories (KB, forums, portals, status)"
```

---

## Task 6: Populate automation and AI categories (AI Agents, Chatbots, Automation)

**Files:**
- Modify: `README.md`

- [ ] **Step 1: Replace the AI Agents placeholder**

```markdown
- [Ada](https://www.ada.cx) - Enterprise AI agent platform for automated customer resolutions (paid; SaaS).
- [Boost.ai](https://www.boost.ai) - Enterprise conversational AI for customer service and self-service (paid; SaaS).
- [Cognigy](https://www.cognigy.com) - Enterprise conversational AI platform with voice and chat AI agents (paid; SaaS).
- [Decagon](https://decagon.ai) - AI agents for customer support with tight enterprise integrations (paid; SaaS).
- [Fin by Intercom](https://www.intercom.com/fin) - AI agent built on top of Intercom's support data (paid; SaaS).
- [Forethought](https://forethought.ai) - AI agent and triage platform for support teams (paid; SaaS).
- [Netomi](https://www.netomi.com) - AI-first customer experience platform for resolution automation (paid; SaaS).
- [Sierra](https://sierra.ai) - Enterprise AI agent platform focused on conversational customer experiences (paid; SaaS).
- [Ultimate (Zendesk AI Agents)](https://www.zendesk.com/service/ai/ai-agents/) - AI agent platform inside Zendesk (paid; SaaS).
```

- [ ] **Step 2: Replace the Chatbots and Conversational AI placeholder**

```markdown
- [Botpress](https://botpress.com) - Developer-focused chatbot platform with an open-source core (freemium; open source; self-hosted / SaaS).
- [Chatfuel](https://chatfuel.com) - No-code chatbot builder for Instagram, WhatsApp, and the web (freemium; SaaS).
- [Dialogflow](https://cloud.google.com/dialogflow) - Google's conversational AI service for voice and chat agents (freemium; SaaS).
- [IBM watsonx Assistant](https://www.ibm.com/products/watsonx-assistant) - Enterprise conversational AI formerly IBM Watson Assistant (paid; SaaS).
- [Kore.ai](https://kore.ai) - Enterprise conversational and generative AI platform (paid; SaaS).
- [Landbot](https://landbot.io) - Visual no-code chatbot builder for web and WhatsApp (freemium; SaaS).
- [LivePerson](https://www.liveperson.com) - Enterprise conversational cloud with messaging and voice AI (paid; SaaS).
- [ManyChat](https://manychat.com) - Chat-marketing and support bots for Instagram, Messenger, and WhatsApp (freemium; SaaS).
- [Rasa](https://rasa.com) - Developer-oriented open-source conversational AI framework (freemium; open source; self-hosted).
```

- [ ] **Step 3: Replace the Automation and Integration placeholder**

```markdown
- [IFTTT](https://ifttt.com) - Lightweight trigger-based integrations across consumer and SMB apps (freemium; SaaS).
- [Make](https://www.make.com) - Visual automation and integration platform, formerly Integromat (freemium; SaaS).
- [n8n](https://n8n.io) - Fair-code workflow automation platform (freemium; open source; self-hosted / SaaS).
- [Pipedream](https://pipedream.com) - Developer-first integration platform with code steps (freemium; SaaS).
- [Power Automate](https://www.microsoft.com/power-platform/products/power-automate) - Microsoft's workflow automation platform (paid; SaaS).
- [Tray.io](https://tray.io) - General-purpose integration and automation platform (paid; SaaS).
- [Workato](https://www.workato.com) - Enterprise iPaaS with strong support-tool recipes (paid; SaaS).
- [Zapier](https://zapier.com) - No-code automation connecting thousands of apps; the category incumbent (freemium; SaaS).
```

- [ ] **Step 4: Run lint and commit**

```bash
pnpm run lint
git add README.md
git commit -m "Seed automation and AI categories"
```

---

## Task 7: Populate proactive/in-product categories (In-App Messaging, Feedback, CS Platforms)

**Files:**
- Modify: `README.md`

- [ ] **Step 1: Replace the In-App Messaging and Product Tours placeholder**

```markdown
- [Appcues](https://www.appcues.com) - In-app onboarding flows, tours, and announcements (paid; SaaS).
- [Chameleon](https://www.chameleon.io) - In-product tours, tooltips, and surveys (paid; SaaS).
- [Pendo](https://www.pendo.io) - Product analytics with in-app guides and feedback (paid; SaaS).
- [Shepherd.js](https://shepherdjs.dev) - Open-source JavaScript library for guided product tours (free; open source; self-hosted).
- [UserGuiding](https://userguiding.com) - No-code onboarding and product tours for SMBs (paid; SaaS).
- [Userflow](https://www.userflow.com) - In-app flows, checklists, and surveys with a developer-friendly SDK (paid; SaaS).
- [Userpilot](https://userpilot.com) - Product experience platform for onboarding and adoption (paid; SaaS).
- [WalkMe](https://www.walkme.com) - Enterprise digital-adoption platform (paid; SaaS).
- [Whatfix](https://whatfix.com) - Digital-adoption platform for enterprise applications (paid; SaaS).
```

- [ ] **Step 2: Replace the Customer Feedback and Voice of Customer placeholder**

```markdown
- [Aha! Ideas](https://www.aha.io/ideas/overview) - Idea management from the Aha! product-management suite (paid; SaaS).
- [Canny](https://canny.io) - Public feature-request boards and roadmap voting (freemium; SaaS).
- [Feature Upvote](https://featureupvote.com) - Simple feature-voting boards for product teams (paid; SaaS).
- [Fider](https://fider.io) - Open-source feedback boards you can self-host (free; open source; self-hosted).
- [Frill](https://frill.co) - Feedback widgets, roadmap, and changelog (paid; SaaS).
- [Nolt](https://nolt.io) - Clean, minimalist feedback and voting boards (paid; SaaS).
- [Productboard](https://www.productboard.com) - Product-management platform with strong feedback capture (paid; SaaS).
- [Savio](https://savio.io) - Customer-feedback tracking tied to CRM and billing data (paid; SaaS).
- [UserVoice](https://www.uservoice.com) - Long-running product feedback and idea-management platform (paid; SaaS).
```

- [ ] **Step 3: Replace the Customer Success Platforms placeholder**

```markdown
- [Catalyst](https://catalyst.io) - Customer-success platform with strong CRM integration (paid; SaaS).
- [ChurnZero](https://churnzero.com) - Customer-success platform focused on SaaS retention and adoption (paid; SaaS).
- [ClientSuccess](https://www.clientsuccess.com) - Mid-market customer-success platform (paid; SaaS).
- [Custify](https://www.custify.com) - Customer-success platform for SaaS with health scores and playbooks (paid; SaaS).
- [Gainsight CS](https://www.gainsight.com/customer-success/) - Leading enterprise customer-success platform (paid; SaaS).
- [Planhat](https://www.planhat.com) - Modern customer-success platform with CRM-like data model (paid; SaaS).
- [Totango](https://www.totango.com) - Customer-success and revenue platform (paid; SaaS).
- [Vitally](https://www.vitally.io) - Customer-success platform designed for B2B SaaS operators (paid; SaaS).
```

- [ ] **Step 4: Run lint and commit**

```bash
pnpm run lint
git add README.md
git commit -m "Seed proactive/in-product categories"
```

---

## Task 8: Populate operations categories (ITSM, WFM, Analytics, CSAT)

**Files:**
- Modify: `README.md`

- [ ] **Step 1: Replace the ITSM and Internal Service Desk placeholder**

```markdown
- [BMC Helix ITSM](https://www.bmc.com/it-solutions/bmc-helix-itsm.html) - Enterprise ITSM platform with strong ITIL coverage (paid; SaaS / self-hosted).
- [Freshservice](https://www.freshworks.com/freshservice/) - Cloud-native IT service desk from Freshworks (paid; SaaS).
- [GLPI](https://glpi-project.org) - Open-source IT asset and service management (free; open source; self-hosted).
- [iTop](https://www.combodo.com/itop) - Open-source ITSM and CMDB platform (free; open source; self-hosted).
- [Jira Service Management](https://www.atlassian.com/software/jira/service-management) - Atlassian's ITSM and employee-service platform (paid; SaaS / self-hosted).
- [ManageEngine ServiceDesk Plus](https://www.manageengine.com/products/service-desk/) - ITIL-aligned ITSM suite from Zoho-owned ManageEngine (paid; SaaS / self-hosted).
- [ServiceNow IT Service Management](https://www.servicenow.com/products/itsm.html) - Market-leading enterprise ITSM platform (paid; SaaS).
- [SolarWinds Service Desk](https://www.solarwinds.com/service-desk) - Mid-market ITSM with asset management (paid; SaaS).
- [SysAid](https://www.sysaid.com) - Mid-market ITSM with built-in automation and AI (paid; SaaS / self-hosted).
- [Znuny](https://www.znuny.org) - Open-source ITSM platform forked from OTRS (free; open source; self-hosted).
```

- [ ] **Step 2: Replace the Workforce Management placeholder**

```markdown
- [Assembled](https://www.assembled.com) - Modern WFM built specifically for support teams (paid; SaaS).
- [Calabrio ONE](https://www.calabrio.com) - Enterprise WFO suite covering WFM, QM, and analytics (paid; SaaS).
- [Injixo](https://www.injixo.com) - Cloud WFM for contact centers (paid; SaaS).
- [NICE Workforce Management](https://www.nice.com/products/workforce-management) - Enterprise WFM, part of NICE CXone (paid; SaaS).
- [Playvox WFM](https://www.playvox.com/workforce-management/) - WFM focused on digital-first support teams (paid; SaaS).
- [Tymeshift](https://tymeshift.com) - Zendesk-native WFM for digital support (paid; SaaS).
- [Verint Workforce Management](https://www.verint.com/workforce-management/) - Enterprise WFM platform (paid; SaaS).
```

- [ ] **Step 3: Replace the Support Analytics and Reporting placeholder**

```markdown
- [Klaus](https://www.klausapp.com) - Conversation review and QA scoring for support teams (paid; SaaS).
- [Looker](https://cloud.google.com/looker) - Enterprise BI platform widely used for support dashboards (paid; SaaS).
- [MaestroQA](https://www.maestroqa.com) - Quality-assurance and coaching platform for support (paid; SaaS).
- [Pathlight](https://www.pathlight.com) - Performance-management and coaching for customer-facing teams (paid; SaaS).
- [Sisense](https://www.sisense.com) - Embeddable analytics platform used for customer-facing support metrics (paid; SaaS).
- [SupportLogic](https://www.supportlogic.com) - Support experience and escalation prediction powered by NLP (paid; SaaS).
- [Tableau](https://www.tableau.com) - Widely used BI tool for building custom support dashboards (paid; SaaS / self-hosted).
```

- [ ] **Step 4: Replace the Survey and CSAT placeholder**

```markdown
- [AskNicely](https://www.asknicely.com) - NPS and CSAT surveys tied to customer workflows (paid; SaaS).
- [Delighted](https://delighted.com) - Simple NPS, CSAT, and CES surveys from Qualtrics (freemium; SaaS).
- [Hotjar](https://www.hotjar.com) - Behavior analytics with on-site feedback and surveys (freemium; SaaS).
- [Medallia](https://www.medallia.com) - Enterprise experience-management platform (paid; SaaS).
- [Qualtrics CustomerXM](https://www.qualtrics.com/customer-experience/) - Enterprise customer-experience management platform (paid; SaaS).
- [Refiner](https://refiner.io) - In-app micro-surveys targeted to SaaS users (paid; SaaS).
- [Simplesat](https://www.simplesat.io) - CSAT embedded in ticket replies for helpdesks (paid; SaaS).
- [SurveyMonkey](https://www.surveymonkey.com) - General-purpose survey platform with CX templates (freemium; SaaS).
- [Typeform](https://www.typeform.com) - Conversational surveys and forms (freemium; SaaS).
```

- [ ] **Step 5: Run lint and commit**

```bash
pnpm run lint
git add README.md
git commit -m "Seed operations categories (ITSM, WFM, analytics, CSAT)"
```

---

## Task 9: Populate specialized categories (Remote Support, Co-Browse, DevRel, i18n, Email)

**Files:**
- Modify: `README.md`

- [ ] **Step 1: Replace the Remote Support and Screen Sharing placeholder**

```markdown
- [AnyDesk](https://anydesk.com) - Fast remote-desktop software with a free tier for personal use (freemium; SaaS / self-hosted).
- [BeyondTrust Remote Support](https://www.beyondtrust.com/remote-support) - Enterprise remote support with strong security controls (paid; SaaS / self-hosted).
- [GoTo Resolve](https://www.goto.com/it-management/resolve) - Remote support and endpoint management, formerly GoToAssist (paid; SaaS).
- [ISL Online](https://www.islonline.com) - Remote-support platform with on-prem deployment option (paid; SaaS / self-hosted).
- [LogMeIn Rescue](https://www.logmeinrescue.com) - Enterprise remote-support toolkit for help desks (paid; SaaS).
- [RustDesk](https://rustdesk.com) - Open-source remote-desktop software you can self-host (free; open source; self-hosted).
- [Splashtop SOS](https://www.splashtop.com/sos) - On-demand remote support for IT and helpdesk teams (paid; SaaS).
- [TeamViewer](https://www.teamviewer.com) - Widely used remote-access and support platform (freemium; SaaS / self-hosted).
- [Zoho Assist](https://www.zoho.com/assist/) - Remote support and unattended access from Zoho (freemium; SaaS).
```

- [ ] **Step 2: Replace the Co-Browse placeholder**

```markdown
- [Cobrowse.io](https://cobrowse.io) - Embeddable co-browse and screen-share SDK for web and mobile (paid; SaaS / self-hosted).
- [Fullview](https://www.fullview.io) - Co-browse and session replay for B2B SaaS support (paid; SaaS).
- [Glance](https://www.glance.net) - Enterprise co-browse, screen share, and video (paid; SaaS).
- [Surfly](https://www.surfly.com) - Universal co-browse that works on any website without install (paid; SaaS).
- [Upscope](https://upscope.com) - No-download co-browse for SaaS support teams (paid; SaaS).
```

- [ ] **Step 3: Replace the Developer Support Platforms placeholder**

```markdown
- [DevRev](https://devrev.ai) - Unified platform bridging support and developer workflows (paid; SaaS).
- [Featurebase](https://featurebase.app) - Feedback, changelog, and help center tuned for developer tools (freemium; SaaS).
- [Linen](https://www.linen.dev) - Community chat (Slack/Discord) mirrored to Google-indexable pages (freemium; SaaS).
- [Plain](https://www.plain.com) - Developer-first support platform built on a modern API (paid; SaaS).
- [Pylon](https://usepylon.com) - Modern B2B support platform with Slack-based customer channels (paid; SaaS).
- [Stack Overflow for Teams](https://stackoverflow.com/teams) - Private Stack Overflow for internal and customer-facing knowledge (freemium; SaaS).
```

- [ ] **Step 4: Replace the Translation and Localization placeholder**

```markdown
- [Crowdin](https://crowdin.com) - Localization management for docs, apps, and support content (freemium; SaaS).
- [Lilt](https://lilt.com) - AI-first translation platform with human-in-the-loop workflows (paid; SaaS).
- [Lokalise](https://lokalise.com) - Translation management platform with strong dev integrations (paid; SaaS).
- [Phrase](https://phrase.com) - Enterprise localization suite, formerly Phrase Strings and Memsource (paid; SaaS).
- [Smartling](https://www.smartling.com) - Enterprise translation-management platform (paid; SaaS).
- [Transifex](https://www.transifex.com) - Localization platform with focus on continuous translation (paid; SaaS).
- [Unbabel](https://unbabel.com) - AI-plus-human translation embedded into support tickets and chat (paid; SaaS).
- [Weblate](https://weblate.org) - Open-source translation-management platform (freemium; open source; self-hosted / SaaS).
```

- [ ] **Step 5: Replace the Email Deliverability placeholder**

```markdown
- [Amazon SES](https://aws.amazon.com/ses/) - Low-cost transactional email service on AWS (paid; SaaS).
- [Haraka](https://haraka.github.io) - High-performance open-source SMTP server written in Node.js (free; open source; self-hosted).
- [Mailgun](https://www.mailgun.com) - Transactional email service with deliverability tooling (freemium; SaaS).
- [Postal](https://postalserver.io) - Self-hosted mail delivery platform (free; open source; self-hosted).
- [Postmark](https://postmarkapp.com) - Transactional email focused on deliverability and speed (paid; SaaS).
- [SendGrid](https://sendgrid.com) - Transactional and marketing email from Twilio (freemium; SaaS).
- [SparkPost](https://www.sparkpost.com) - Transactional email platform now part of MessageBird (paid; SaaS).
```

- [ ] **Step 6: Run lint and commit**

```bash
pnpm run lint
git add README.md
git commit -m "Seed specialized categories (remote, co-browse, devrel, i18n, email)"
```

---

## Task 10: Final polish and full-repo lint verification

**Files:**
- Modify: `README.md` (polish pass only)

- [ ] **Step 1: Run awesome-lint and fix anything it catches**

```bash
pnpm run lint
```

If it reports issues, address them one at a time. Common fixes:
- **"List items should not contain uppercase at the start of description"** → check for a missing period at the end of the previous sentence.
- **"List items should be alphabetically sorted"** → reorder. Note that awesome-lint compares case-insensitively and ignores leading articles.
- **"Link X is broken"** → replace with the working URL (test in a browser first).
- **"Duplicate list items"** → dedupe.

- [ ] **Step 2: Manually eyeball the README in a Markdown preview**

Open the README in VS Code preview or a GitHub preview. Confirm:
- All 23 category sections render.
- TOC links all jump to real anchors.
- No placeholder entries remain (search for "Placeholder" and "Temporary").

Run:
```bash
grep -n -i placeholder README.md || echo "no placeholders"
grep -n -i "temporary entry" README.md || echo "no temporary entries"
```

Expected: both print `no placeholders` / `no temporary entries`.

- [ ] **Step 3: Final lint pass**

```bash
pnpm run lint
```

Expected: exit 0.

- [ ] **Step 4: If any fixes were applied, commit**

```bash
git add README.md
git commit -m "Final lint-clean polish"
```

If nothing changed, skip the commit.

---

## Task 11: Publish to GitHub as mpge/awesome-support

**Prerequisites:**
- `gh` CLI is installed and authenticated as `mpge` (`gh auth status` should print `Logged in to github.com as mpge`).
- If not authenticated, run `gh auth login` in an interactive terminal before starting this task.

- [ ] **Step 1: Verify gh auth**

```bash
gh auth status
```

Expected: output includes `Logged in to github.com as mpge`. If it says any other user, stop and resolve before proceeding.

- [ ] **Step 2: Create the GitHub repo and push**

From `C:\Users\work\awesome-support`:

```bash
gh repo create mpge/awesome-support \
  --public \
  --source=. \
  --remote=origin \
  --push \
  --description "A curated list of customer and IT support software — paid, free, and open source."
```

Expected: command prints the URL `https://github.com/mpge/awesome-support`.

- [ ] **Step 3: Verify the CI lint workflow runs and passes**

```bash
gh run list --limit 1
```

Expected: one workflow run for `Lint` with status `completed` and conclusion `success`. If it is still `in_progress`, wait 30-60 seconds and re-run. If the conclusion is `failure`, inspect with:

```bash
gh run view --log-failed
```

Fix anything the CI linter catches that the local linter missed (rare, but can happen if Node versions differ), push the fix, and re-verify.

- [ ] **Step 4: Add repo topics so the list is discoverable**

```bash
gh repo edit mpge/awesome-support --add-topic awesome --add-topic awesome-list --add-topic support --add-topic customer-support --add-topic helpdesk --add-topic itsm --add-topic customer-experience
```

- [ ] **Step 5: Verify end state**

```bash
gh repo view mpge/awesome-support --web
```

Expected: browser opens to a public repo showing:
- Green CI check on the latest commit.
- README rendering with awesome badge at the top.
- All 23 categories populated.
- CC0 license auto-detected by GitHub.
- Repo topics visible in the sidebar.

---

## Post-publish: optional next steps (not part of this plan)

- Apply for listing on the main [awesome](https://github.com/sindresorhus/awesome) index. Criteria: project must be ~30 days old, pass `awesome-lint`, have meaningful content, and follow all conventions. Open a PR on sindresorhus/awesome when ready.
- Configure branch protection on `main` requiring the `Lint` check to pass before merging PRs.
- Add a repository social preview image.
