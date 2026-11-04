# Agency Onboarding, Cowork + Google Ads MCP + the MAA Skill Suite

A setup checklist for an agency adopting this suite in **Claude Cowork**, with
their own Google Ads MCC. No hosted/managed-agent component is involved: the
stack is just (1) the read-only Google Ads MCP as the data source, (2) these
six skills installed as a plugin, and (3) a synced folder for outputs and
client continuity. Budget ~half a day for the one-time setup, then ~10 minutes
per client.

---

## Phase 0, Credentials (yours, not ours)

Everything authenticates against **your own** Google Ads MCC and Google Cloud
project. Nothing is shared with the publisher of this repo. You need:

1. **Google Ads developer token**, from API Center in your MCC settings.
   Basic access or higher for live client accounts (test tokens only reach
   test accounts).
2. **A Google Cloud project** with the Google Ads API enabled.
3. **OAuth client ID/secret** from that project. Authorize with a user on your
   MCC so every child account is reachable.

⚠️ **Two things that look like "the MCP stopped working."** First, publish
the OAuth app to production; an app left in "Testing" status issues refresh
tokens that expire after 7 days. Second, and the one that actually caused our
daily reconnects from July to September 2026: deploy Cloud Run with the
Firestore token store, or the server forgets every token on restart. Both are
in `shared/frameworks/mcp-setup-guide.md`, Option 2.

## Phase 1, Stand up the Google Ads MCP

Full detail in `shared/frameworks/mcp-setup-guide.md`. For Cowork specifically:

- **Recommended: deploy to Cloud Run and add it as a custom connector**
  (Settings → Connectors → Add custom connector → your Cloud Run URL). Cowork
  sessions run in the cloud, so a remote URL connector works everywhere, any
  team member, any device, scheduled runs included.
- **Alternative: run it locally** via the desktop app's MCP config. Works, but
  the connector is only available while that desktop is online, which rules
  out unattended scheduled runs.

**Joining a server someone else already deployed** (most team members): you
do not build anything. Get the server's `/mcp` URL from whoever runs it, add
it in Cowork (Settings → Connectors → Add custom connector), and sign in with
your own Google account. That account must be a user on the MCC; the server
only proxies your identity, it grants nothing. Each person's sign-in is
stored separately, so one person reconnecting never affects another.

Verify (5 min): ask Claude *"What customers do I have access to?"*, you
should get your MCC's account list back. Then spot-check one campaign summary
pull against the Google Ads UI (cost arrives in micros; divide by 1,000,000).

## Phase 2, Install the skill suite

```
/plugin marketplace add Local-Service-Spotlight/google-ads-maa-skills
/plugin install google-ads-maa-skills
```

Or install the packaged release zip from the repo's Releases page. Confirm the
six skills appear (analyzer, copy-optimizer, change-scripts, lp-auditor,
script, client-view). The suite is MIT-licensed; forking your own copy is fine
and lets you pin versions, just pull upstream updates deliberately.

## Phase 3, Workspace and output configuration

1. Connect a synced folder to Cowork sessions (Google Drive, Dropbox, or a
   local folder via the desktop app) and tell Claude once per session, or put
   it in a memory/instructions file, where deliverables go. That location is
   `{OUTPUT_DIR}` (see `CONFIGURATION.md`).
2. Pick a continuity mode per `CONFIGURATION.md`: the simple
   `{Client}_MAA-Narrative.md` file (zero structure, fine for most agencies
   starting out) or a structured per-client knowledge base. Start simple; you
   can graduate later.
3. If your agency has house style rules (report header, formatting, naming),
   write them into a single overlay file in your workspace and tell Claude it
   overrides the skill defaults. Don't fork the skills to change style.

## Phase 4, Per-client onboarding (~1 hour, one run)

Run the onboarding skill once per client:

> Onboard {client}, CID {digits}, website {url}. Primary contact {name, role}.
> Vault folder {OUTPUT_DIR}/{client}/. Draft only.

It verifies access, scrapes 24 months of account history (all statuses, so
inherited paused inventory is recorded as inherited), investigates the site
and every landing page, builds the service-area test from the site and the
90-day geo data, packages the questions only the client can answer, and
writes the client folder: `compiled/ledger.md`, `data-sources.md`,
`maa-metric-spec.md`, `lp-map.md`, `trend.csv` (13 weeks backfilled),
`account-history.md`, and an onboarding brief for you.

Forward the intake ask to the client. Put the answers into the ledger
(definitions, target CPL, service area, exclusions, seasonality). The whole
account sits in a two-cycle quarantine, so the first two weekly MAAs report
and watch; only foundation breaks become Actions.

## Phase 5, Weekly cadence

Use Cowork **scheduled tasks** for the weekly run. Each firing starts a fresh
session, so the prompt must be standalone. Schedule it after your data drops
(if you run the email fallback, after the export lands). This is the standard
prompt; substitute the client block and keep the stages verbatim:

> Weekly Google Ads MAA for {client}. CID {digits}, MCC {digits}. Client
> folder: {OUTPUT_DIR}/{client}/. Draft only. Never post, never email, never
> render the client view until I approve in chat.
>
> STAGE 0, READ BEFORE ANY DATA: compiled/ledger.md in full, then the
> knowledge base, the metric spec, the shared overlay files, and the most
> recent dated MAA. If the ledger is missing, stop and report.
>
> STAGE 0b, PULL: the full pack per gaql-query-pack.md "Pull discipline",
> 7D and 30D, all statuses for inventory, paginated with counts recorded, raw
> response saved. Email fallback only if the MCP is down; never splice.
>
> STAGE 1, DRAFT under shared/frameworks/evidence-gates.md, with the claim
> register in Generation notes. Context questions are answered from the
> ledger or added to its Open ASKs, never asked in chat.
>
> STAGE 2, HARDEN: dispatch google-ads-reviewer as a subagent with only the
> frozen draft, the raw pack, the ledger and the spec. Apply its verdicts
> once. One round.
>
> STAGE 3, RECONCILE against the ledger: numbers, watch-list age, quarantine,
> cross-references, never-surface list, render norms.
>
> STAGE 4, WRAP: append the ledger and the trend row. Do not render. Finish
> with an eight-line summary: leads and CPL vs last cycle, Actions proposed,
> items demoted or dropped, open ASKs for me, data faults hit.

**Daily check.** One scheduled task for the whole roster, not one per client.
Schedule it after the accounts' day closes (early morning local). Prompt:

> Run google-ads-daily-check across every account in
> {OUTPUT_DIR}/_shared/daily-check-roster.md. Read only sections 2, 4 and 7
> of each ledger. Exceptions only. Write each account's block to its delivery
> target, append ledger lines per the skill, and finish with the one-line
> summary. Change nothing. Do not post to any client.

Two operating rules worth enforcing from day one:

- **The MCP is read-only and stays that way.** Account changes ship as
  dry-run-first Google Ads Scripts via `google-ads-change-scripts`, reviewed
  by a human before running live. When you run one, the ledger change log
  gets a row; that row is the record, not `change_event`.
- **Every deliverable is a draft.** The suite never posts to the client, your
  PM tool, or email on its own.

What "5% oversight" looks like once the ledger has two cycles in it: read the
eight-line summary, answer any ASK, approve the render, post. About five
minutes per client per week.

## Troubleshooting quick hits

| Symptom | Likely cause | Fix |
|---|---|---|
| MCP tools missing, or "failed query" with no Reconnect button | Server lost its tokens (in-memory store, restarted) | Remove and re-add the connector today; deploy with the Firestore token store so it stops (setup guide, Option 2) |
| Connector prompts for Google sign-in again after ~7 days | OAuth app still in "Testing" status | Publish the OAuth app to production (Phase 0), reauthorize once |
| "Unrecognized field" GAQL errors | API version drift | Use `get_resource_metadata` for the resource; update `gaql-query-pack.md` |
| Analyzer asks context questions in chat on a weekly run | No ledger, or the ledger is missing the definition | Add the fact as a ledger line; the next run reads it |
| Draft recommends pausing something on one week of data | Gates not applied, or the hardener was skipped | Check the Generation notes for the verdict ledger; re-run Stage 2 |
| Zero-spend clutter in reports | Dormant campaigns included | They should be omitted entirely, see the data contract |
| LSA campaign showing up in the MAA | LSA surfaced by the MCP | Out of scope by rule; omit (separate deliverable if wanted) |
| QS components look wrong | Swap applied to MCP data | Never swap MCP data; the corrector is email-path only |
