# Internal CRM and ERP for a three-founder company

A case study from [Kortem](https://kortem.es). Built with my two co-founders. The repository is private, so this covers what it does, the decisions behind it, and what I built.

## The problem

The company ran on spreadsheets and forms: a partners' cash ledger in Excel plus a form to log movements, separate Excel files for quotes, and nothing tying clients, meetings and follow-ups together. The CRM and ERP replaces them with one private app for the three partners.

## What it does

| Area | What it covers |
|---|---|
| Dashboard | Cash on hand, monthly sales, next distribution, open quotes, meetings, clients needing follow-up |
| Cash ledger | The whole partners' ledger: summary, split per partner, movement entry |
| Clients | Pipeline from lead to client with follow-up reminders; creation and CSV/Excel import |
| Calendar | Meetings, calls and tasks, weekly view, `.ics` feed for Google Calendar and iPhone |
| Quotes | Editor by parts (hours x rate or fixed price), discounts, monthly items, clauses, statuses, PDF proposal, "send to client" |
| Mail | Company mailbox over IMAP, lead segments, reply from the app, push notifications |
| Domains | Expiry, registrar and where each domain points |

## Stack

TypeScript · Next.js · React · managed Postgres and Auth · Tailwind · Vitest · Playwright (with accessibility checks) · Web Push

## Decisions

| Decision | Why |
|---|---|
| Sign-up is closed; the three partners are created by hand | It is an internal tool. Nobody can register on their own. |
| First access by email link, then each person sets their own password | Easy first login, normal login afterwards, and the link remains available. |
| Login messages never reveal whether an email belongs to a partner | No account enumeration. |
| Password rules are enforced by the auth layer, not only the interface | The rule can't be bypassed from the client. |
| The email link opens a confirmation page with an "Enter" button, and the token is redeemed by a POST | Mail clients that pre-open links to scan them no longer burn the link before the person does. |
| Link works on any device | The first version only worked in the browser that requested it, which failed when a link was requested on a computer and opened on a phone. |
| A demo mode with invented data | Screens can be designed without a database. |
| Real data never enters the repo; spreadsheets are blocked by `.gitignore`; seed data uses only reserved example addresses | Business data stays out of Git. |
| Import scripts are idempotent | Re-running an import never duplicates rows. |
| CI gate on every PR: type check, lint, tests, build; PR naming check | Each person merges their own PR once CI is green, with no gatekeeper. |

## What I built

From my merged pull requests in this repo:

- **Login and access:** the move from first-link access to own passwords, and the fix that made links work on any device and survive mail scanners.
- **Deployment:** the Dockerfile for the company VPS.
- **Data:** idempotent import of the cash ledger; client creation and CSV/Excel import; manual domain management.
- **Notifications and email:** push notifications for new mail and for what each partner does; the "send quote to client" flow that opens the composer with the proposal ready.
- **Interface:** profile photos, interface sounds and animations.
- **Team process:** the PR that merges the partners' open PRs and the contributing rule on CI-green merges.

Sales prospecting and much of the mail work were built mainly by my co-founders.
