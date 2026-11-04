# Google Ads MAA Skills

A connected set of Claude skills for managing Google Ads for **local service
businesses** using the MAA framework: **Metrics → Analysis → Action**. One skill
diagnoses and quarterbacks; the others execute the fixes it calls for.

These skills produce tight, actionable working documents, not 20-page audit
reports. The methodology comes from Dennis Yu's BlitzMetrics approach to local
service marketing.

## Getting started in 5 minutes

1. **Install** the skills one of the three ways below (the quickest is to copy
   the folders in `skills/` into `~/.claude/skills/`).
2. **Try it with data you already have.** Export three reports from a Google Ads
   account (campaign summary, keyword report with Quality Score, search terms),
   then ask Claude: *"Do an MAA for this account"* and attach them. The analyzer
   handles the rest and pulls in the other skills as needed.
3. **Read the example** in `shared/examples/` to see what good output looks like.
4. **Connect live data (recommended).** Set up the Google Ads MCP
   (`shared/frameworks/mcp-setup-guide.md`) and the analyzer pulls every dataset
   straight from the API, correctly-labeled Quality Score components, no email
   plumbing, and it can pull follow-up data mid-analysis when its own findings
   raise a question.
5. **Or automate via email.** Ask for *"a Google Ads script that emails me the
   weekly MAA data"* and the `google-ads-script` skill builds the pipeline so
   future MAAs run from an automatic weekly email. This is also the recommended
   backup for scheduled runs while MCP authorization persistence is unsolved.

For the complete operating loop, connecting the data source, running the
analysis, reviewing, and rendering the client report, see **WALKTHROUGH.md**.

## What's in the box

| Skill | What it does |
|---|---|
| **google-ads-onboarding** | The front door. One run per new client: access check, 24-month account scrape, website and landing-page investigation, geo footprint, client intake ask, and the first ledger. |
| **google-ads-daily-check** | The morning pass. One run across the roster: disapprovals and policy, serving stops, budget caps, silent conversion actions, dead landing pages. Exceptions only. Changes nothing, optimizes nothing. |
| **google-ads-analyzer** | The quarterback. Ingests campaign/keyword/search-term/ad data and produces a concise MAA. Diagnoses the account and dispatches the skills below. |
| **google-ads-reviewer** | The hardener. In a fresh context, re-derives every number and applies the evidence gates to every finding, returning KEEP / DEMOTE / DROP / REPULL verdicts; format grading second. Never rewrites, never adds analysis. |
| **google-ads-copy-optimizer** | Writes specific RSA headlines/descriptions and extension improvements to fix Ad Relevance, Expected CTR, and ad strength. |
| **google-ads-change-scripts** | Turns MAA action items into one-time Google Ads Scripts (negatives, pauses, RSA updates, restructures). Every script defaults to dry-run. |
| **google-ads-lp-auditor** | Audits a landing page against the ads pointing to it, intent alignment, conversion friction, Landing Page Experience signals. |
| **google-ads-script** | Builds production Google Ads Scripts for data pipelines and ongoing automation (the weekly MAA data feed, BigQuery sync, monitoring). |
| **google-ads-client-view** | The final render step. Turns a finished MAA into a client-facing report (clean markdown + a paste-ready, Basecamp-tuned HTML) with a plain-language summary and two 13-week trend charts. Adds presentation, never new analysis. |

How they connect is described in `shared/frameworks/methodology.md` ("the
quarterback model"). A full **worked example** is in `shared/examples/`, one anonymized client
(Summit Dumpster Rental) for the week of 2026-06-26: a real MAA and its real
production client-view render (markdown + Basecamp HTML + trend charts), plus
sample ad-copy and change-script outputs. See that folder's README.

## Publishing it (for the maintainer)

See **PUBLISHING.md** for step-by-step instructions to put this on GitHub.

## Install

**Requirements:** none for the core skills (they're Markdown). Optional extras:
Python 3.x with `matplotlib` for the client-view trend charts
(`pip install -r requirements.txt`), and `pipx` if you run the Google Ads MCP
locally via stdio.

These skills follow the standard Claude skill/plugin layout, so there are three
ways to use them.

### A. As a plugin (Claude Code / Cowork)

This repo is both a plugin and a single-plugin marketplace.

```
# in Claude Code
/plugin marketplace add Local-Service-Spotlight/google-ads-maa-skills
/plugin install google-ads-maa-skills
```

In Cowork, add the plugin from the marketplace file the same way, or install the
packaged `.plugin`/`.zip` from the releases.

### B. Drop the skills in directly

Copy the folders under `skills/` into your skills directory:

- Claude Code project: `.claude/skills/`
- Claude Code user-wide: `~/.claude/skills/`

Each skill is a self-contained folder (`SKILL.md` plus its `references/`).

### C. Just read them

Every `SKILL.md` is plain Markdown. You can read the methodology and use it
manually even without Claude.

## Using it

The intended loop is weekly, and it is ledger-first (see
`shared/frameworks/ledger-template.md` and `evidence-gates.md`):

0. `google-ads-onboarding` runs once per new client and writes the ledger.
1. `google-ads-analyzer` reads the ledger, then pulls the account data, live via the Google Ads MCP
   (primary; see `shared/frameworks/mcp-setup-guide.md` and
   `shared/frameworks/gaql-query-pack.md`), or from the weekly data email
   (fallback; `shared/frameworks/data-pipeline-contract.md`).
2. It writes the MAA under the evidence gates, hands it to `google-ads-reviewer` for hardening, applies the verdicts, and dispatches the copy optimizer / change scripts / LP
   auditor where the data warrants, and on the MCP path, pulling follow-up
   data mid-analysis instead of deferring open questions a week.
3. You review, approve, and run the generated scripts (always dry-run first).
   The MCP is read-only by design; every account change ships as a
   dry-run-first script.
4. `google-ads-client-view` renders the MAA into a client-facing report you can
   paste into Basecamp (or any rich-text tool) for delivery.

**Full step-by-step:** WALKTHROUGH.md covers the end-to-end weekly system, from
cold install through client delivery, including both data-source setups.

## Roadmap

- **Persistent MCP authorization: solved 2026-09-29.** The daily reauth was
  the server keeping OAuth tokens in memory and losing them on every Cloud Run
  restart. The Cloud Run sequence in `shared/frameworks/mcp-setup-guide.md`
  now deploys with a Firestore token store; a forced restart followed by a
  working query with no reconnect is the acceptance test. Keep the email
  pipeline wired as the fallback anyway; it costs nothing.
- **Deeper mid-cycle data pulls.** The analyzer already answers its own
  questions with follow-up MCP pulls; extending that pattern to more sources
  (page speed, keyword research, analytics) is ongoing.

You can also run any skill ad hoc, "review this landing page," "write headlines
for this ad group," "build a script that does X." Triggers are listed in each
skill's description.

Output location is configurable, see **CONFIGURATION.md**.

## Works with or without BlitzBase

These skills are fully **standalone**. The MAA methodology, dispatch logic, and
data-pipeline contract are bundled under `shared/frameworks/`, so nothing
external is required.

For week-over-week continuity there are two optional, auto-detected modes (see
CONFIGURATION.md): a simple per-client narrative file, or, if you run a
BlitzBase-style knowledge base, a `compiled/` wiki of living status docs that
the analyzer reads and maintains, with a vault-level overlay file able to
override any generic skill convention. Either is an optional enhancement, not a
dependency.

## License

MIT, see `LICENSE`.

## Run records and the definitive article

Every real execution of the analyzer writes a run record (meta article) in
`docs/meta/`, one execution ID each. The definitive article for the analyzer
is https://localservicespotlight.com/google-ads-maa-agent/ ; revised in place for
the 1.5.0 process on 2026-09-15; the source of the live body is
`docs/definitive-article/google-ads-analyzer.md`. The
Task Library home is https://local-service-spotlight.github.io/task-library/ .
