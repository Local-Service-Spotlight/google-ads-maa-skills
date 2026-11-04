# Changelog

## 1.6.0, 2026-10-19 (synced to the public repo 2026-11-02)

Copy optimizer rebuilt around sources. Driven by a live dumpster account where
1.5.0 copy advertised a 10-yard size the business does not carry and a review
count that later went stale, both still serving months on, and by the
operator's review that the skill "didn't try to pull from the website or tie to
any particular data points."

- `skills/google-ads-copy-optimizer/SKILL.md` rewritten: ledger first; a dated
  fact sheet built from the client's website and Google Business Profile; the
  landing page's own language recorded per final URL; a fact check of every
  live ad line (Supported / Contradicted / Unsupported) before any writing;
  90-day search terms, earlier copy tests and asset ratings read before
  proposing; every proposed line cites its source; one test per ad group with
  a decides-on metric and a read date under the evidence gates. Contradicted
  claims are corrections, foundation-level and exempt from gates 1 and 2.
- New `shared/frameworks/copy-facts-template.md` (the fact sheet).
- New `skills/google-ads-copy-optimizer/references/copy-patterns.md`: the old
  angle framework with every concrete claim replaced by a placeholder filled
  from the fact sheet.
- Analyzer and methodology dispatch tables: two new triggers (a contradicted
  live claim; assets rated LOW two cycles running).
- `shared/examples/SummitDumpster_AdCopy_2026-04-24.md`: banner marking it as a
  pre-1.6.0 sample with a known factual error.

## 1.5.0, 2026-09-06 (analyzer SKILL.md revised 2026-09-15 for the Task Library review)

**2026-09-15, second pass, from the first two executions.** Added
`docs/meta/GAA-2026-09-11-C1A.md`, the meta article for one real scheduled
run (2026-09-11, client anonymized), and a retired combined record in
`docs/meta/archive/` covering the 2026-09-08 back-test and the four-account
cycle. Added `docs/definitive-article/google-ads-analyzer.md`, the in-place
revision of the live definitive article
https://localservicespotlight.com/google-ads-maa-agent/ for the five-stage
process (same URL; publish pending). Analyzer `definitive_article` now carries
that URL; `## Example(s)` links the run article. Added `docs/images/` with the
five-stage pipeline diagram, the run diagram for GAA-2026-09-11-C1A and the
14-week calls-per-click chart (matplotlib, source data from the run's call
records). Both articles graded A on the house rubric after three revision
rounds and staged as WordPress drafts (77077 revision, 77076 meta article). Recipe changes
with evidence in the run record: hardener output pasted verbatim (verdict
table, BLOCK/WARN/STYLE counts, grade before and after) as a Definition-of-done
box; derived blocks in the raw pack computed from rows, never typed; run start
and end timestamps recorded; weekly-log Action count read off the file;
answered chat instructions become dated ledger rules. GAQL pack: historical
quality score alongside current state; cap discipline for negatives, search
terms and change_event; asset-level disapproval query (11b); auction-insight
permission, impression-share sentinels and bid placeholders as known faults.
Reviewer step (b) re-sums any derived block. Ledger template section 9 states
the read-off-the-file rule. `status` stays `needs-work` until the article is
live and a reviewer certifies it.

Task Library review response (`reviews/google-ads-analyzer-2026-09-15.md`
in Local-Service-Spotlight/task-library, which graded the bytes on `main` at
`e55015f`, the pre-1.5.0 analyzer): house-format frontmatter (`category`,
`stage`, `definitive_article: GAP`, `status: needs-work`); grade 3.7 plain
opening with first action and the handoff; `## Inputs` as a contract with
access separate from files, prerequisites with checked outputs, and a single
missing-access rule (stop and hand off); `## Steps`; `## Definition of done`
with measurable boxes, a saved-output read-back, and named receiving tasks;
`## Example(s)` linking the real anonymized 1.1.0 run and naming the missing
1.5.0 example; `## Definitive article & links` with the GAP stated honestly;
`## Routing lane and tools` (lane `judgment`); `## Sources and labels`
table that labels every threshold as Google-published, house heuristic, or
unverified. The read-only boundary now appears in the first screen of
Inputs. The full 1.5.0 procedure is unchanged below the new sections.

The ledger-first release. Driven by an audit of five live weekly runs on two
accounts, which counted about fifty human interventions per week of runs:
roughly 40% supplying facts the operator held in memory, 25% knocking down
recommendations built on one week of data, 15% correcting false findings from
capped or filtered queries, 15% re-explaining render style, and the rest the
analyzer contradicting itself across passes. The tool was slow because the
first draft had no memory and no guardrails, not because it lacked agents.

New:
- `shared/frameworks/ledger-template.md`: one compact file per client
  (definitions, standing rules, change log, watch list, recent-change
  quarantine, open ASKs, render spec, weekly log, known data faults). The
  analyzer reads it before any data and appends it after every cycle. It
  replaces `change_event` as the record of what changed.
- `shared/frameworks/agent-receipt.md`: every agent-authored post opens with
  an identification line and closes with the house receipt line (owning desk,
  agent, model, action, human review). Applied by the daily check and the
  client view.
- `shared/frameworks/evidence-gates.md`: ten mechanical rules deciding whether
  a finding may become an Action, each tied to a real miss. Applied by the
  analyzer while drafting and by the reviewer in a fresh context. Foundation
  breaks are exempt.
- `skills/google-ads-daily-check/`: exceptions-only morning pass across a
  roster of accounts (disapprovals and policy, serving stops, budget caps and
  spend far outside budget, conversion actions gone silent, final URLs that
  stop resolving). Suppresses anything the ledger change log says was
  deliberate. Reports foundation breaks with an owner; everything else goes
  to the ledger watch list for the weekly run. No optimization by design.
  Query pack in `references/daily-queries.md`; roster format in the SKILL.
- `skills/google-ads-onboarding/`: one run per new client. Access check,
  24-month scrape with all statuses (inherited paused inventory recorded as
  inherited), website and landing-page investigation with a dispatched tag
  audit, three-way geo footprint (stated vs targeted vs actual spend), a
  packaged client intake, and the first ledger with the account quarantined
  for two cycles. Query pack in `references/onboarding-queries.md`.

Changed:
- `google-ads-analyzer`: five-stage weekly pipeline (read, pull, draft,
  harden, reconcile, wrap). Ledger read first. Full pack pulled before any
  prose. Gates and a claim register as hard rules above Step 3. Weekly runs
  answer context questions from the ledger or add them to its Open ASKs;
  they no longer ask in chat. Auto-dispatches the reviewer. Stops before the
  render on scheduled runs and ends with an eight-line summary.
- `google-ads-reviewer`: now the hardener. Re-derives every number, applies
  the gates to every finding and Action, returns one verdict each (KEEP,
  DEMOTE TO WATCH, DROP, REPULL) with the gate and the contradicting number.
  Format grading runs second, capped at two STYLE findings. One revision
  round, down from two. The wiring the 1.4.0 notes left pending is done.
- `google-ads-client-view`: the "full MAA reproduced verbatim" rule is gone.
  The Full analysis section is rewritten in client language per the ledger's
  render spec, with a default strip list and plain labels; target length about
  a third of the MAA. A trace check (every client-view sentence quoted back to
  its MAA line) is a required step; it caught six invented claims on a live
  render. Cross-references re-checked after internal items are dropped.
- `shared/frameworks/gaql-query-pack.md`: "Pull discipline" section
  (pagination with counts, no status filter on inventory, no absence claims on
  capped sets, verify campaign names, save raw first). `campaign_budget`,
  network split, device split, `user_location_view`, three-level negatives
  and `call_view` added to the standard pack. `change_event` documented as
  unreliable; the ledger change log is authoritative.
- `ONBOARDING-AGENCY.md`: per-client onboarding now runs the onboarding
  skill; the weekly scheduled-task prompt is the standard five-stage prompt.
- `README.md`, `CONFIGURATION.md`, `shared/frameworks/methodology.md` updated
  for the ledger, the gates and the pipeline.

- `google-ads-change-scripts`: every script carries a pre-filled ledger row in
  its header; when the operator confirms a live run, the skill writes the row,
  the quarantine entry and any watch-list change, verifies via the API, and
  replies with the rows only. "log a change: ..." trigger for UI edits. The
  analyzer records any account state the change log does not explain as an
  "unlogged change" ASK rather than analyzing it.
- `google-ads-onboarding`: Phase G also writes a filled weekly scheduled-task
  prompt (`references/weekly-task-template.md`) so a new client's task is a
  paste, not a hand-typed prompt.
- `google-ads-daily-check` thresholds recalibrated after the first live run
  (two accounts, 2026-09-08): tracking check is rate-based with a 14-spend-day
  stop rule so a clean stop is caught even with an empty 30-day window, and
  REMOVED actions are ignored; serving check adds a volume-independent
  "was serving, now zero for 7/14 days" rule; budget-lost bar 25% on three
  consecutive days and the underspend test uses a median; campaign policy
  flags require an ENABLED disapproved ad and LIMITED/BUDGET_CONSTRAINED is
  not a serving exception; landing-page host comparison is on registrable
  domain so www to apex is not a finding.
- `google-ads-daily-check`, second recalibration after the 2026-09-09 run:
  rules that catch low-volume breaks now say "no volume floor" in their
  own name (2(b), 4(a)) after two were misread as having one; 2(b) needs 20
  impressions across the 28-day precondition so single-impression brand ad
  groups stop tripping RED; check 4 scoped to actions with
  include_in_conversions_metric true and reads all_conversions; scheduled
  dark days are detected and excluded before check 2; impression share is
  recomputed daily because Google's figures settle for 24 to 48 hours; the
  raw file carries a named key per check with explicit empty arrays so a
  check that did not run is never read as clean; the roster is re-read after
  the last account and a slug in both tables is not run; the interactive
  Basecamp staging path is removed (Lexical editor, shortcut hazard) and the
  paste-ready block is the only delivery; MCP field fixes (campaign start and
  end dates not selectable, IS NOT NULL rejected on final_urls, URL checks
  through the browser).
- `google-ads-daily-check`: ledger-defined conversion action groups are
  evaluated as one series in check 4 (a call asset reporting under one action
  while a sibling receives merged rows produced a false stop-rule RED); an
  "under audit" ledger state is carried without severity so an explained
  item is not re-alarmed daily; conversion rows by segments.date are noted as
  click-dated.
- `google-ads-daily-check`: RED items get a paste-ready Basecamp block (and,
  on interactive runs, are staged in the thread's comment box, never
  submitted). Roster gains a Basecamp internal thread column.

Not changed: copy optimizer, LP auditor, script builder, the grading rubric's
dimensions (the reviewer still uses them, after the gates).

## 1.4.0, 2026-08-12

Adds the review stage: one agent writes the MAA, a different agent grades it.
Follows the two-agent generate/review pattern already proven on the GA4 side,
where reports self-graded A- consistently while cold graders scored the same
reports B, B+ and C-.

New:
- `skills/google-ads-reviewer/`, independent grader. Runs in a fresh context
  against the frozen MAA, the source data, and the client's spec. Emits a grade
  plus a punch list where every finding carries a tier (BLOCK/WARN/STYLE), the
  quoted offending line, the contradicting evidence, and a return path.
- `shared/frameworks/maa-grading-rubric.md`, the grading standard. Ten
  dimensions, an auto-fail list, and severity tiers.
- `skills/google-ads-reviewer/references/review-output-format.md`, the punch
  list schema and a worked example.

Design notes:
- **Precedence is the load-bearing rule.** The rubric is the lowest layer of
  authority, below the client's metric spec and the deployment's overlay files.
  A deviation the client's spec calls for is correct output, not a defect. This
  was written in response to a real case: an account whose spec places auction
  insights and Quality Score components inside the Metrics section, which the
  generic rubric would otherwise flag as a structural failure every week.
- **Three return paths, not one.** REWRITE (analyzer fixes from data in hand),
  REPULL (analyzer pulls more data in the same cycle), ASK (only a human can
  answer). ASK findings never route back to the analyzer, because an agent asked
  to close a human-knowledge gap produces a confident guess.
- **Restraint is a stated goal.** The likely failure mode is too many findings,
  not too few. STYLE findings are capped, padding is forbidden, and zero
  findings is a valid result.
- Coverage is declared on every review, so a structural-only pass is never
  mistaken for a verified one.

Not yet wired: the analyzer does not auto-submit for review, and the client-view
render is not gated on a verdict. The reviewer is invoked directly while it is
being calibrated. Those two changes land once back-testing shows the grades
match human judgment.

## 1.3.0, 2026-07-23

Fit-and-finish release driven by a cold-start review: a fresh session with no
prior context attempted to guide setup from the docs alone. Everything that
tripped it is fixed here.

Docs, blocking fixes:
- `README.md`: install command had a literal `<your-github-username>`
  placeholder; now reads `Goodrich-Dev/google-ads-maa-skills`. Added a
  Requirements line (Python 3.x + matplotlib for charts, pipx for local stdio
  MCP) and updated the continuity paragraph to describe both auto-detected
  modes.

Docs, setup depth (`shared/frameworks/mcp-setup-guide.md`):
- Prerequisites expanded into a "Before you start" walkthrough: where the
  developer token lives, a dedicated test-vs-Basic access callout (apply on
  day one; manual approval lag), enabling the Google Ads API in GCP, OAuth
  client type per deployment (Desktop for stdio, Web for Cloud Run), and
  publishing the OAuth consent screen to Production moved up from "Known
  limitation" into setup, doing it up front prevents the 7-day refresh-token
  expiry instead of treating it as a later fix.
- Cloud Run option now includes the concrete `gcloud` sequence, with the
  two-pass deploy called out explicitly (`GOOGLE_ADS_MCP_BASE_URL` is assigned
  by Cloud Run after the first deploy, so deploy → read URL → update).

Troubleshooting (`WALKTHROUGH.md`):
- New first entry: what a test-access token failing against a live account
  looks like, and the fix (request Basic access).
- Reauthorization entry now distinguishes lapsed authorization from
  misconfiguration, including the Testing-status OAuth app masquerading as
  constant lapses.

Hygiene:
- Added `.gitignore` (output/, credentials patterns, Python and OS noise),
  `{OUTPUT_DIR}` defaults to `./output`, which previously landed client MAAs in
  the repo working tree with nothing preventing a commit.
- Added `requirements.txt` (matplotlib, optional).
- Added `shared/scripts/README.md` documenting `correct_qs_columns.py` usage
  and the never-on-MCP-data warning.

## 1.2.0, 2026-07-23

Continuity system upgraded from a single narrative file to an optional
structured knowledge base, based on a live first-run test against a brand-new
client account (no prior setup) that surfaced where the skills and a structured
vault disagree.

Continuity:
- `CONFIGURATION.md`: "BlitzBase / vault continuity" section rewritten as
  **two auto-detected continuity modes**, simple mode (the existing
  `{Client}_MAA-Narrative.md`, unchanged, still the zero-setup default) and
  **knowledge-base mode** (a `compiled/` wiki of living status docs:
  `maa-metric-spec.md`, `paid-status.md`, `active-issues.md`,
  `data-sources.md`, `trend.csv`, plus `raw/maa-reports/` as the delta and
  voice source). When `compiled/` exists the analyzer reads and maintains it
  instead of the narrative file.
- First-run bootstrap rule: if `maa-metric-spec.md` is missing in
  knowledge-base mode, the analyzer creates it before writing the report,
  from the most recent archived MAA, or from the first run's own structure for
  a brand-new client.
- Overlay contract documented: a vault-level
  `clients/_shared/maa-skills-binding.md` file may override any generic skill
  convention (naming, section ordering, house style). Overlay wins over skill
  defaults. This lets one public skill codebase serve differently-structured
  private vaults without forking.

Analyzer:
- Narrative-file instructions scoped to simple mode only; knowledge-base mode
  reads `client-knowledge-base.md` → `compiled/` → last archived MAA, and the
  wrap-up updates `paid-status.md` / `active-issues.md` / `trend.csv` instead
  of appending a narrative log entry.

Notes from the first-run test that motivated this release:
- A cold account with no prior setup produced a structurally sound MAA from
  the skill alone (MCP path, QS components correctly read without the swap,
  LSA correctly excluded), confirming the skills stand alone.
- Everything that broke was continuity-layer, not analysis-layer: file naming,
  narrative vs. wiki, house style rules the skill had no way to know. Hence
  the overlay contract rather than more skill body text.

Packaging & distribution:
- Restored the missing `.claude-plugin/` folder (`plugin.json` +
  `marketplace.json`), the documented `/plugin marketplace add` install path
  did not work without it.
- Added `ONBOARDING-AGENCY.md`, a Cowork-focused setup checklist for a
  third-party agency adopting the suite with their own MCC, credentials, and
  Google Ads MCP deployment (Cloud Run connector recommended; no
  managed-agent component). Includes weekly scheduled-run prompt template and
  a troubleshooting table.


## 1.1.0, 2026-07-10

Synced from the live production version. The operating system these skills
mirror runs weekly on real client accounts; improvements found in those runs
flow here. Headline change: **the Google Ads MCP is now the primary data
source**, with the email pipeline as the fallback.

Data pipeline:
- Added `shared/frameworks/mcp-setup-guide.md`, full setup guide for the
  official Google Ads MCP server (prerequisites, local + Cloud Run deployment,
  verification, reauthorization caveat).
- Added `shared/frameworks/gaql-query-pack.md`, the GAQL queries that
  reproduce every MAA dataset live from the API, with unit conversions and the
  QS component field mapping.
- `data-pipeline-contract.md` rewritten around source priority: MCP first
  (read-only, correctly-labeled QS components, no swap), email fallback second,
  manual exports third.
- Added `shared/scripts/correct_qs_columns.py`, deterministic corrector for
  the email path's QS column inversion. The swap is now mechanical, scoped to
  the email path only, with a per-client anchor sanity check.
- LSA campaigns explicitly out of scope (they surface on the MCP path; omit
  them like dormant campaigns).

Analyzer:
- New diagnostic gates from live runs: Structure Before Tactics (dependency
  ordering of recommendations), Verify Structure From the Data (never infer
  targeting from names), Label Inferences as Inferences.
- Action section: one list with inline owner tags (no per-person sub-sections),
  no padding, no non-actions, no method notes in the report body.
- Mid-cycle data pulls: when the analysis raises a question, pull the extra
  data in the same cycle and turn it into a grounded action.

Client view:
- Confident bottom line: the above-the-fold summary never ends on a caveat;
  hedges move to the 🟡 section.
- 🟡 vs 🔴 split formalized; arrangement-type watch items framed as the shared
  plan, not our deficiency.
- "Render" always routes to this skill's templates, never an ad-hoc mockup.
- "What we need from you" carries only genuine asks; resolved asks drop off.

Docs:
- README, WALKTHROUGH (Path A = MCP, Path B = email), and CONFIGURATION updated
  for the two-path data setup; roadmap section added (persistent MCP
  authorization is the known blocker to fully hands-off scheduled runs).

## 1.0.0, 2026-06-30

First public release. Packaged six production Google Ads skills for
distribution:

- google-ads-analyzer (the quarterback: MAA framework)
- google-ads-copy-optimizer
- google-ads-change-scripts
- google-ads-lp-auditor
- google-ads-script
- google-ads-client-view (final render step: client-facing report + trend charts)

Changes made for distribution:
- Removed internal/workspace-specific output paths; output location is now
  configurable (see CONFIGURATION.md).
- Removed references to private vault/knowledge-base paths. The MAA methodology
  and data-pipeline contract are bundled under `shared/frameworks/` so the
  skills work standalone.
- Added a full anonymized worked example under `shared/examples/`: one client
  (Summit Dumpster Rental, week of 2026-06-26) with a real MAA and its real
  production client-view render (markdown + Basecamp HTML + two trend charts +
  the trend.csv input), plus sample ad-copy and change-script outputs. Client,
  people, locations, account IDs, and competitor names changed; numbers and
  reasoning preserved.
- Internal vault-integrated test variants of these skills are intentionally
  excluded from this package.
