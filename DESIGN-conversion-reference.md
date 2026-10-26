# sas-ref: SAS Conversion Reference - Design

Status: **DRAFT for operator review - no code written, nothing banked yet.**
Working defaults throughout; redirect any time.

Author: the-pgh-cid lane (waystation). Date: 2026-10-26.

---

## 1. Purpose and positioning

Today `sas-ref` is a *data corpus*: a provenance-pinned snapshot of the 56
Rosetta Code SAS solutions, plus three SAS reference PDFs, the Parquet format
files, and a minimal migration scaffold (`sas_migration-main/`) with an empty
`.mcp.json` placeholder.

The goal is to make `sas-ref` a **full SAS conversion reference source**: one
queryable, provenance-pinned body of knowledge that an agent (or a human) can
use to convert SAS to an open target - statements, data set options, formats,
functions, macros, procedures, plus the landmines, the coverage map, and the
corpus of worked examples. It exposes that knowledge through a **knowledge +
skills MCP** so any agent session can look up "how does SAS do X, what's the
Python/R target, what's the silent difference, is it gated, and where's the
evidence" without re-deriving it.

### Relationship to the other repos (the important constraint)

Per the operator's ruling, **sas-ref holds *copies*, not the canonical home**,
of the machine-readable knowledge:

- **SASchi-Go** (`bunta/saschi-go`, public lane) remains the *canonical*
  home of the rulebook, capability manifest, and semantics reference, and the
  *gate host* that proves them.
- **sas-campaign** (`cid/sas-campaign`, professional waystation) is the
  re-voiced mirror that ships into the enterprise.
- **sas-ref** is the *reference source*: it carries **copies** of that knowledge
  plus the corpus and the harvested SAS documentation, and it is the layer that
  turns them into a queryable MCP.

This keeps the two existing repos as they are ("at least for now"), and it
means sas-ref is itself a **sister-repo target**: the same sas-ref shape will be
re-created under `buntas-tofu/` as well. So the design must be **reproducible
from a copy + a sync pointer**, not dependent on private state, and must carry
its own sanitization floor (see §9) so the buntas-tofu sister is neutral too.

**Implication:** the rulebook/capability/semantics files in sas-ref are
**sync artifacts**. They are not hand-edited in sas-ref. A sync job copies them
from the canonical repo, stamps the source commit + version + copy time, and a
CI check verifies the copy matches the stated source commit. Editing them in
sas-ref is a drift smell.

---

## 2. Design principles

1. **Provenance-pinned, reproducible.** Every file records *where it came from*
   (URL, revision/commit, retrieval time, license). A file is reproducible and
   auditable against its source. This is already the corpus's standard
   (`TASKS.csv`: `file, task, url, revision, retrieved`); it extends to every
   layer.
2. **Copies carry a source stamp.** Synced knowledge carries `source_repo`,
   `source_commit`, `version`, `copied_at` in a sidecar. Never silently diverge;
   a stamp lets the drift check do its job.
3. **Data, not software, at the knowledge layer.** The KB is JSON/CSV/markdown
   that agents read. The only code is the harvesters and the MCP server.
4. **Deterministic, gated harvest.** Refresh scripts are idempotent
   (`--check` exits non-zero on drift and writes nothing; a plain run refreshes
   and reverts unmarked local edits to generated files). Matches the existing
   `fetch_rosetta_sas.py` contract.
5. **Coverage is measured, not claimed.** The capability manifest is the
   denominator. An entry is either *assessed* (GATED / RULE / POLICY /
   HUMAN-REVIEW / DOCTRINE / OUT-OF-SCOPE) or UNASSESSED. "Null universe" means
   *not enumerated yet*, not *empty*.
6. **Neutral, sanitized output.** sas-ref ships under the-pgh-cid and gets a
   buntas-tofu sister, so the tree itself must carry no host/infra/persona
   identity (the same floor we just applied to sas-campaign). Provenance
   points at public sources, not the estate.
7. **The landmine is the product.** The most valuable content is not "how to
   write a SET" but "SAS truncates differing char widths silently" - the
   `silent_difference_checks`. The MCP surfaces these first-class.
8. **Advisory, not authoritative.** The MCP *explains, drafts, interprets*.
   Nothing it returns is a verification. A conversion is accepted because a
   gate ran and the numbers matched (in sas-campaign/SASchi-Go), not because
   the reference said so.

---

## 3. Four-layer architecture

```
┌─────────────────────────────────────────────────────────────────────────┐
│  LAYER 4: SKILLS (workflow)                                            │
│  SKILL.md files: analyze → plan → convert → validate → document.        │
│  Teach an agent HOW to drive the MCP + apply the rules + stay advisory. │
├─────────────────────────────────────────────────────────────────────────┤
│  LAYER 3: MCP SERVER  (mcp/)                                           │
│  Python FastMCP stdio server. Reads L1+L2, exposes tools/resources/     │
│  prompts. Wired via .mcp.json into the migration scaffold / any client. │
├─────────────────────────────────────────────────────────────────────────┤
│  LAYER 2: HARVEST PIPELINE  (tools/)                                   │
│  Provenance-pinned ingestors: Rosetta (exists), SAS 9.4 reference docs   │
│  (new), + slots for other public sources. Deterministic, --check.       │
├─────────────────────────────────────────────────────────────────────────┤
│  LAYER 1: KNOWLEDGE BASE  (kb/)                                        │
│  kb/rulebook/  kb/capability/  kb/semantics/   (copies, synced)         │
│  kb/corpus/    (Rosetta, provenance-pinned)                             │
│  kb/reference/ (harvested SAS 9.4 docs, indexed)                        │
│  kb/convert/   (target guides: duckdb/polars/r)                         │
│  kb/census/    (macro surface census)                                   │
└─────────────────────────────────────────────────────────────────────────┘
```

Each layer is independently consumable: L1 is readable by humans and by any
tool; L2 keeps it fresh; L3 makes it queryable; L4 makes it usable in a
workflow.

---

## 4. Knowledge base layout and schemas

```
sas-ref/
├── kb/
│   ├── rulebook/
│   │   ├── sasconversionrulebook.json      # SYNCED copy (canonical: SASchi-Go)
│   │   └── SOURCE.json                     # provenance stamp (§4.6)
│   ├── capability/
│   │   ├── capability-manifest.json        # SYNCED copy
│   │   └── SOURCE.json
│   ├── semantics/
│   │   ├── sas_semantics.py                # SYNCED copy (the reference impl)
│   │   └── sas_semantics_reference.txt     # SYNCED copy (kb01 plain-text mirror)
│   ├── corpus/                             # the 56 Rosetta tasks (re-homed here)
│   │   ├── *.sas
│   │   ├── TASKS.csv
│   │   └── TASKS.md
│   ├── reference/                          # HARVESTED SAS 9.4 docs (new)
│   │   ├── index.json                      # entry index across all docs
│   │   ├── pdfs/                           # the pinned source PDFs
│   │   └── extracted/<docset>/             # pdftotext output, per-doc
│   │       ├── <docset>.txt
│   │       └── entries/                    # one .md/.json per entry (page-pinned)
│   ├── convert/                            # target guides (from sas_migration-main)
│   │   ├── sas_to_duckdb.md
│   │   ├── sas_to_polars.md
│   │   └── sas_to_r.md
│   └── census/
│       ├── macro_surface_census.csv
│       └── macro_surface_census.md
├── mcp/
│   ├── server.py                           # FastMCP stdio server
│   ├── sources.py                          # kb loader + resolvers
│   ├── pyproject.toml
│   └── README.md
├── tools/
│   ├── fetch_rosetta_sas.py                # (exists)
│   ├── sync_knowledge.py                   # NEW: pull copies from canonical repo
│   ├── fetch_sas_reference.py              # NEW: harvest + index the 9.4 PDFs
│   └── check_corpus_index.py               # (exists)
├── .mcp.json                               # MCP client config (wired)
├── CLAUDE.md                               # workflow entry (updated)
├── skills/                                 # LAYER 4 (SKILL.md files)
└── ...
```

### 4.1 Rulebook (synced) - schema as it exists

Canonical: `SASchi-Go/docs/sasconversionrulebook.json`. The copy keeps the
exact shape (do not re-shape in sas-ref):

- `meta` - title, purpose, version, date, `python_stack`, `r_stack`
  (pinned target versions), `equivalence_classes`
  (`EXACT`, `EQUIVALENT-WITH-SETTINGS`, `APPROXIMATE`,
  `NO-DIRECT-EQUIVALENT`), `tolerance_tiers`
  (e.g. `closed_form 1e-10`, `iterative_mle_se 1e-05`, `stochastic` =
  validate distributions never draws).
- `contract` - 5 invariants (one blessed target per language per rule;
  `required_settings` are mandatory not advisory; `forbidden_patterns` must
  never appear; `NO-DIRECT-EQUIVALENT` ⇒ STOP and emit a human-review ticket;
  every translated unit ships its `validation_test` into the golden-master
  harness).
- `global_policies` - 9.
- `rules` - **67** entries. Families by prefix: **DS 32, ST 16, MC 9, SV 6,
  SQL 3, MX 1**.
- `runtime_capabilities` - plan version + evidence + per-capability notes.

**Rule entry fields** (the query surface):

```
rule_id                 # "DS-001"
sas_pattern             # "SET a b; (concatenation)"
scope                   # "data-step"
python_target           # {package: "pandas", pattern: "pd.concat([a,b], ignore_index=True)"}
r_target                # {package: "dplyr",  pattern: "bind_rows(a, b)"}
alternates_when         # "large data: polars ...; duckdb UNION ALL BY NAME"
required_settings[]     # mandatory, not advisory
forbidden_patterns[]    # must never appear in generated code
equivalence_class       # EXACT | EQUIVALENT-WITH-SETTINGS | APPROXIMATE | NO-DIRECT-EQUIVALENT
silent_difference_checks[]  # THE landmines - surfaced first-class
validation_test         # how the gate proves it
confidence              # high | medium | low
sources[]               # "S-01" → annotated source inventory
```

### 4.2 Capability manifest (synced) - schema as it exists

Canonical: `SASchi-Go/docs/capability-manifest.json`.

- `reference_frame` - `product: "SAS 9.4"`; `documents[]` each with
  `{id, name, universe, enumerated}`. Enumerated so far: **stmt 108**,
  **dso 40**; the rest declared with `universe: null` (not yet enumerated -
  *null is not zero*).
- `dispositions` - 7: `GATED, RULE, POLICY, HUMAN-REVIEW, DOCTRINE,
  OUT-OF-SCOPE, UNASSESSED`.
- `entries` - **148** (108 + 40). Each:

```
entry                 # "ARRAY Statement"
document              # "stmt" | "dso" | ...
category              # "Action" | "CAS" | ...
type                  # "Executable" | "Declarative"
valid_in              # "DATA step"
disposition           # one of the 7
rule_ids[]            # → rulebook rules that cover it
gates[]               # → the gate names that prove it
reference_page        # page in the reference PDF
note                  # optional
```

- `rollup` - per document: `universe, assessed, gated, rule, human_review,
  out_of_scope, unassessed`. (Current: stmt 108/32 assessed, 23 gated; dso
  40/29 assessed, 2 gated, 7 human-review, 20 out-of-scope.)
- `provenance` - who/what measured the entry list (measured) vs. who made the
  disposition calls (judgment), per document.

### 4.3 Semantics (synced)

- `sas_semantics.py` - the reference implementation of the SAS surface the
  rulebook and gates rely on (the landmines pinned in code).
- `sas_semantics_reference.txt` - the plain-text kb01 mirror (human/diff
  friendly). Kept in lockstep with the .py by the canonical repo; the copy
  stays in lockstep by being a copy.

### 4.4 Corpus (re-homed)

The existing 56 `.sas` files + `TASKS.csv` move under `kb/corpus/` (or are
symlinked; physical move keeps the harvest self-contained). Unchanged
content; the harvest contract (`fetch_rosetta_sas.py --check`) is preserved.

### 4.5 Reference layer (new, harvested)

The SAS 9.4 reference set is **public, no login**:

```
https://documentation.sas.com/api/docsets/<docset>/9.4/content/<docset>.pdf
```

Known docsets to plan for:

| docset          | document (9.4)                              |
|-----------------|---------------------------------------------|
| `lestmtsref`    | DATA Step Statements: Reference             |
| `leddsoptsref`* | Data Set Options: Reference                 |
| `leforinforref` | Formats and Informats: Reference            |
| `lefunctionsref`| Functions and CALL Routines: Reference      |
| `lesysoptsref`  | System Options: Reference                   |
| `lecompobjref`  | Component Objects: Reference                |

*\*confirm the exact docset id at harvest time; the DSO doc set's slug must
be verified before pinning.* (The capability manifest's `reference_frame`
already records which are enumerated.)

The harvester (`tools/fetch_sas_reference.py`) does, per docset, the **same
extraction recipe that built the capability manifest**:

1. `pdftotext -layout <doc>.pdf <doc>.txt` (stdlib `json`/`re` do the rest;
   pymupdf not required).
2. The document's own **CONTENTS listing** is the authoritative entry list
   (dot leaders become runs of spaces → match `(?:\.\s+){3,}` before the
   page number). Where a CONTENTS lists only chapters (the Data Set Options
   case), the dictionary headings ARE the list and the by-category index
   table is the cross-check.
3. The **DICTIONARY section** gives per-entry metadata: `Valid in:`,
   `Categories:`/`Category:`, `Type:` (Executable/Declarative),
   `Restriction:`. Detect the field block rather than keying on one
   terminator word (it varies: `Statement` / `Routine` / `Data Set Option` /
   `Option`).
4. Per-entry **page extraction**: `pdftotext -layout -f N -l N`; find the
   exact heading line; map physical→logical page via the running-header
   offset; write one entry file (`.md` with a JSON frontmatter block) pinned
   to its page.

Output: `kb/reference/extracted/<docset>/<docset>.txt` +
`entries/<slug>.md`, and a consolidated `kb/reference/index.json`
(`entry → {docset, page, category, type, valid_in, file}`) that the MCP
queries. This is what makes the reference layer *indexable* rather than a
pile of PDFs.

### 4.6 Source stamp (new convention)

Every **synced** directory carries a `SOURCE.json`:

```json
{
  "layer": "rulebook",
  "source_repo": "SASchi-Go",
  "source_commit": "<40-hex>",
  "version": "1.0",
  "copied_at": "2026-10-26T00:00:00Z",
  "files": { "sasconversionrulebook.json": "<sha256>" }
}
```

`tools/sync_knowledge.py` writes it; a CI job recomputes the file hashes and
asserts they match, and (if the canonical repo is reachable) that the commit
still exists. **Editing a synced file in sas-ref is a drift smell** the check
will surface.

---

## 5. Harvest pipeline

All harvesters share one contract:

- **`--check`** - verify current files against the source; exit **1** on
  drift, write nothing.
- **plain run** - refresh; keep retrieval stamps for unchanged files; revert
  **unmarked** local edits to **generated** files by design (marked/hand files
  are never touched).
- **Deterministic** - same source, same output; no wall-clock or RNG in the
  generated content.
- **Provenance** - record URL + revision/commit + retrieval time + license per
  file.

| harvester                  | source                              | status     |
|----------------------------|-------------------------------------|------------|
| `fetch_rosetta_sas.py`     | rosettacode.org SAS category (56)   | exists     |
| `fetch_sas_reference.py`   | documentation.sas.com 9.4 docsets   | **new**    |
| `sync_knowledge.py`        | canonical rulebook/capability/sem   | **new**    |
| (future) `fetch_<src>.py`  | other public sources, e.g. SUGI/SGF | slot       |

`sync_knowledge.py` specifics: it does **not** scrape - it pulls the three
canonical files from the **canonical repo** (a local path or a git remote the
operator points it at), writes them under `kb/`, and stamps `SOURCE.json`.
This is the "copy, not canonical home" mechanism, and it is what makes the
buntas-tofu sister reproducible (same script, different source pointer).

---

## 6. MCP server spec

**Runtime:** Python, **FastMCP**, **stdio** transport. One server reads the KB
and exposes it. Stdio because it is wired into the migration scaffold and any
local agent client via `.mcp.json`; it needs no network and no auth.

`mcp/.mcp.json` (client config) - replaces the empty placeholder:

```json
{
  "mcpServers": {
    "sas-ref": {
      "command": "python",
      "args": ["-m", "sas_ref.mcp.server"],
      "cwd": "<repo root>",
      "env": { "SAS_REF_KB": "kb" }
    }
  }
}
```

### 6.1 Tools

Tools are the *actions* - targeted lookups and searches that return a
compact, decision-ready answer (rule + landmines + disposition + evidence,
not a raw dump).

| tool | args | returns |
|------|------|---------|
| `lookup_rule` | `rule_id` | the full rule: pattern, python/r target, alternates, required_settings, forbidden_patterns, equivalence_class, **silent_difference_checks**, validation_test, confidence, sources |
| `search_rules` | `query`, `family?` (DS/ST/MC/SV/SQL/MX), `equivalence_class?`, `target?` (python/r), `limit?` | ranked list of `{rule_id, sas_pattern, scope, equivalence_class, confidence}` with a one-line why-it-matched |
| `get_landmines` | `rule_id` \| `family` \| `query` | just the `silent_difference_checks`, deduped - "what will silently disagree" |
| `lookup_capability` | `entry`, `document?` | the capability entry: disposition, rule_ids, gates, page, note |
| `capability_status` | `document?` | the rollup for a doc (or all): universe / assessed / gated / rule / human_review / out_of_scope / unassessed |
| `search_reference` | `query`, `docset?`, `limit?` | matching reference entries from `kb/reference/index.json` with page + file pointer |
| `read_reference` | `docset`, `entry` (or `page`) | the pinned page/entry text from the extracted docs |
| `search_corpus` | `query`, `limit?` | Rosetta tasks matching (title / filename / body), from TASKS.csv + files |
| `get_conversion_guide` | `target` (duckdb/polars/r) | the `kb/convert/sas_to_<target>.md` guide |
| `conversion_contract` | - | the rulebook `contract` + `global_policies` + `tolerance_tiers` (the invariants a converter must honor) |

Every tool result carries a small **provenance footer** (source_commit +
version for synced layers; url+revision for harvested), so an answer is
traceable to its source.

### 6.2 Resources

Resources are the *readable content*, URI-addressed, for clients that want to
pull a document whole (as opposed to a tool's compact answer).

```
sasref://rulebook                       # the full rulebook JSON
sasref://rulebook/{rule_id}             # one rule
sasref://capability                     # the full manifest
sasref://capability/rollup              # just the rollup
sasref://semantics/reference            # the kb01 plain-text mirror
sasref://convert/{target}               # a target guide
sasref://corpus/{task}                  # one Rosetta .sas
sasref://reference/{docset}/{entry}     # one pinned reference page
sasref://census/macro                   # the macro census
```

### 6.3 Prompts

Prompts are the *workflow templates* that load the right context in one
call - this is the bridge to LAYER 4 (skills) for MCP-native clients.

| prompt | inputs | does |
|--------|--------|------|
| `convert` | `sas_source`, `target` | loads `conversion_contract` + the relevant rules (by detected constructs) + `get_conversion_guide(target)`, and frames the task: analyze → plan → convert → validate, **advisory** (explains/drafts/interprets; never verifies) |
| `landmine_audit` | `sas_source` or `rule_ids` | pulls the `silent_difference_checks` for the constructs present, so a converter checks the silent differences before trusting output |
| `coverage_gap` | `document?` | reports the UNASSESSED / HUMAN-REVIEW entries (the known unknowns) for a doc, so a planner knows where evidence is thin |

### 6.4 The advisory rule (carried into the MCP)

The server never claims a conversion is correct. Tool and prompt outputs are
**reference material**: patterns, targets, landmines, dispositions, evidence
pointers. The gate (in sas-campaign/SASchi-Go) is what makes a conversion
*accepted*. The server says so in its own banner, mirroring the
"assistant is advisory by law" doctrine already in the rulebook's
`runtime_capabilities`.

---

## 7. Skills (LAYER 4)

`skills/` holds `SKILL.md` files that encode the **workflow** on top of the
MCP, for agents that read skills (e.g. Hermes/Claude) rather than only MCP
prompts. Two skills to start:

- **`sas-conversion`** - the analyze → plan → convert → validate → document
  loop: how to detect constructs, which MCP tools to call, how to apply the
  `contract` invariants, how to surface landmines, and the hard line that the
  skill *interprets and drafts* but the *gate verifies*. References the
  MCP tool names explicitly so the skill and the server stay in lockstep.
- **`sas-kb-maintenance`** - how to run the harvesters, read a drift check,
  run `sync_knowledge.py`, and what "a copy that drifted" means and how to
  fix it (re-sync, never hand-edit).

The skills are the "how"; the MCP is the "what." Both stay neutral (§9).

---

## 8. Cross-repo relationship and sync

```
SASchi-Go (canonical, gate host)
   │  rulebook / capability / semantics  (source of truth)
   ▼
tools/sync_knowledge.py  ──copy + SOURCE.json stamp──►  kb/rulebook, kb/capability, kb/semantics
   ▲                                                                  │
   │ (operator points it at the local path or a git remote)          ▼
 sas-ref (the reference source)  ◄────────  MCP server reads the KB
   │                                                                   
   └── re-created under buntas-tofu/ (the sister) - same shape, same
       tools, same skills, different sync pointer + sanitized tree
```

**Drift handling:** sas-ref never edits synced files. When SASchi-Go moves,
`sync_knowledge.py` refreshes the copy and stamps the new commit; CI
re-hashes against `SOURCE.json` and fails on a mismatch. This is what lets the
two sas-ref instances (the-pgh-cid and buntas-tofu) stay in lockstep with the
canonical repo without either being the home.

**The buntas-tofu sister:** identical repo shape. The only deltas are
(1) the `sync_knowledge.py` source pointer, (2) the sanitization pass (the
buntas-tofu tree keeps buntas-tofu identity; the the-pgh-cid tree keeps
professional identity - same floor, different allowed tokens), and (3) the
corpus/reference harvests, which are source-independent and therefore
byte-identical across both sisters.

---

## 9. Governance

- **Provenance gate:** every `kb/` file carries source provenance; CI asserts
  presence (synced: `SOURCE.json` + hash match; harvested: url/revision/time;
  corpus: `TASKS.csv`).
- **Sync gate:** synced layers must match their `SOURCE.json` hashes.
- **Style floor:** the `dmf-lint` floor (no em dashes / ellipses, balanced
  fences, status lines, manifest schema) applies to the new markdown/JSON, same
  as the rest of the estate.
- **Sanitization floor:** a token gate over the committed tree. Allowed
  identity is *per-instance* (the-pgh-cid tree: `the-pgh-cid`, `Matthew
  Haubach`; buntas-tofu tree: `buntas-tofu`); **not** allowed anywhere: host
  names (cloud/fahrenheit/wutai/...), `spira`, `besaid`, tailnet hosts, the
  personal gmail, and cross-lane names. (This is the exact floor sas-campaign
  just passed; it ships as a check, not a habit.)
- **CI workflows:** mirror the existing estate shape - a **governance**
  workflow (dmf-lint + manifest + provenance + sanitization + sync gate) and a
  **verification** workflow (run the MCP server's self-test: load the KB, call
  a few tools, assert shapes; plus the corpus `--check`).

---

## 10. Phased build plan

Proposed order, each phase independently shippable and verified before the
next. (Nothing here is built yet - this is for review.)

1. **Phase 0 - house the KB.** Create `kb/`; move the 56 corpus files +
   `TASKS.csv` to `kb/corpus/`; pull the first synced copies (rulebook,
   capability, semantics) from SASchi-Go with `SOURCE.json` stamps; move the
   three convert guides + the census into `kb/`. *Deliverable: a clean,
   provenance-pinned KB layout. Verify: provenance gate + hashes.*
2. **Phase 1 - the MCP, wired to what we have.** `mcp/` FastMCP server +
   `.mcp.json`, exposing the §6 tools/resources/prompts over the KB as it
   exists (rulebook, capability, corpus, convert, census). *Deliverable: an
   agent can look up a rule + its landmines + its disposition + a target guide
   + a corpus example. Verify: a server self-test + a few live tool calls.*
3. **Phase 2 - the skills.** `skills/sas-conversion` + `sas-kb-maintenance`
   SKILL.md files that drive the MCP. *Verify: a skill-driven conversion
   walkthrough that stays advisory.*
4. **Phase 3 - the 9.4 reference harvest.** `fetch_sas_reference.py`;
   `kb/reference/index.json` + pinned entry pages; wire `search_reference` /
   `read_reference`. *Deliverable: the reference layer carries indexed SAS 9.4
   documentation. Verify: determinism (`--check`), page pinning, and that the
   enumerated docsets reconcile with the capability manifest's `reference_frame`.*
5. **Phase 4 - governance + the sister.** Full CI (governance + verification
   incl. the sanitization + sync gates); stand up the buntas-tofu sister with
   the re-pointed sync source and its own sanitization pass. *Verify: both
   sisters green, corpora byte-identical, each sanitized to its instance.*

Phases 1 and 3 are the two big value levers (the queryable core, then the
external documentation). Phases 0/2/4 make it durable, usable, and reproducible.

---

## 11. Open questions / decisions for you

1. **MCP transport & packaging.** Stdio-only for now, or do you also want a
   **streamable-HTTP** mode so a remote / headless client (e.g. the enterprise
   server) can query sas-ref without a local checkout? (Stdio is the minimal
   proof; HTTP is a later phase, not a blocker.)
2. **`sync_knowledge.py` source pointer.** Should it pull from a **local path**
   (`~/besaid/bunta/saschi-go`) or a **git remote/commit** by default? A remote
   is cleaner for the buntas-tofu sister (no private local state), but a local
   path is zero-network. I'd default to *configurable, remote-preferred.*
3. **Reference docset scope for Phase 3.** Start with the two already
   enumerated (stmt, dso) to reconcile against the capability manifest, or
   harvest the whole public set (formats, functions, system options, component
   objects) up front? I'd start stmt+dso (reconciles with what we already
   assessed) and enumerate the rest as follow-on harvests.
4. **`sas_migration-main/` fate.** It currently lives in the sas-ref root as a
   scaffold. (As of 2026-10-26 it exists only in an unpushed local commit,
   `b618e0d`; it is not yet on remote main. Nothing here is a verdict on it.) Do we (a) fold its useful parts (the convert guides, the Claude
   settings) into `kb/convert/` + `skills/` and retire the scaffold, or (b)
   keep it as-is and just point the new MCP/skills at it? I'd fold the guides
   in and retire the scaffold - one home for the workflow, not two.
5. **Naming.** `sas-ref` as the repo; should the MCP server / package be
   `sas-ref-mcp`, `sas_ref`, or something that reads well for the enterprise
   audience (it's the buntas-tofu sister too)?

---

*This is the design for review. No code written, no commits made, no harvests
run. On your go, Phase 0 is the entry point and every phase ships verified
before the next.*

Housekeeping notes for the reviewer:

- **Pre-existing CI debt:** the sas-ref Governance workflow has been red since
  2026-10-16 (broken link pointers in the vendored
  `parquet-format-README.md`, and later in the `sas_migration-main/`
  scaffold templates). Independent of this doc; this doc itself passes
  the style floor. Fixing it is a separate, small job.
- **Unpushed local work:** local main carried an unpushed commit
  (`b618e0d`, the unzipped scaffold) as of 2026-10-26. This design doc
  was pushed on its own branch; main was left exactly as found.
