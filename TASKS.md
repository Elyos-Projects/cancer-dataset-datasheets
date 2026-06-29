# TASKS — cancer-dataset-datasheets

> Status: Draft · Version: 0.1.0 · Last updated: 2026-06-28 · Owner: TBD (maintainer) · Lane: donated

> **Binding cancer guardrails apply to every task below:** open-access / aggregate / de-identified data
> only; controlled-access (dbGaP, EGA, ICGC DACO, individual-level biobanks) and any identifiable patient
> data are OUT OF SCOPE; COSMIC/OncoKB are non-commercial/custom → flag, never treat as open; no medical
> advice (patient-facing = `riskTier: high`, oncologist + advocate reviewed, "not medical advice" banner);
> provenance on every assertion. See `PLAN.md` → *Data, licensing & compliance*.

## How these tasks map to Elyos

Each task becomes an Elyos **Task JSON** validated against `packages/schema/src/schemas.ts`:

- `id` — stable slug ID from the tables (e.g. `cancer-dataset-datasheets-template-003`).
- `title` — the table's Title.
- `project` — `cancer-dataset-datasheets`.
- `type` — one of `code | research | writing | data | design-spec | maintenance` (per table).
- `lane` — `donated` for all tasks here (no funded escrow). A funded task would add `fundedBudgetUsd`.
- `priority` — `high | medium | low`.
- `domain` — array, e.g. `["cancer-research","open-science","bioinformatics","data-documentation"]`.
- `riskTier` — `low | medium | high`. Access-tier/identifiability/license-judgement and per-dataset
  cancer tasks are `medium`; **patient-facing tasks are `high`**.
- `urgent` — boolean; `false` for all current tasks.
- `deliverable` — `pr | dataset | document | translation`. We **never** deliver `dataset` (data is out of
  scope); code → `pr`, docs/metadata → `document`, translations → `translation`.
- `tokenEstimate` — `small | medium | large` (Size column).
- `status` — `open | in-progress | review | delivered | done`; all start `open`.
- `context`, `objective`, `acceptanceCriteria[]`, `resources[]`, `output` — per task.
- `requestor` — **TO BE SECURED** until a steward/maintainer is confirmed.
- `verifiedNeed` — **`false`** until a named steward/maintainer/lab agrees to accept contributions (the
  general need is real; per-task delivery need is unproven).
- `outputLicense` — `CC-BY-4.0` for documentation/metadata; `MIT` for code (validators/scanner/generator).

---

## Milestone M0 — Foundation & cold-start

| ID | Title | Type | Size | Risk | Deliverable | Depends on | Reviewer |
| --- | --- | --- | --- | --- | --- | --- | --- |
| cancer-dataset-datasheets-reviewer-001 | Name/secure the License + Genomic-Privacy reviewer (blocking gate role) | research | small | medium | document | — | Maintainer |
| cancer-dataset-datasheets-template-003 | Cancer-dataset datasheet template + canonical metadata model | writing | small | low | document | — | Technical |
| cancer-dataset-datasheets-gate-004 | Access-tier + identifiability + license gate checklist (blocking) | design-spec | small | medium | document | — | License+Privacy |
| cancer-dataset-datasheets-license-matrix-005 | Cancer-source licensing & access matrix (TCGA/GDC, GEO, cBioPortal, DepMap, COSMIC, OncoKB, ICGC, CPTAC, SEER) | research | medium | medium | document | — | License+Privacy |
| cancer-dataset-datasheets-policy-006 | NC/custom-license + redistribution policy (COSMIC/OncoKB handling) | design-spec | small | medium | document | license-matrix-005 | License+Privacy |
| cancer-dataset-datasheets-croissant-007 | Croissant ML metadata generator + spec validator | code | medium | low | pr | template-003 | Technical |
| cancer-dataset-datasheets-provenance-008 | Provenance + license-text snapshot capture tool | code | small | low | pr | template-003 | Technical |
| cancer-dataset-datasheets-outreach-009 | Steward/partner outreach + open-access candidate shortlist | research | small | low | document | license-matrix-005 | Maintainer |
| cancer-dataset-datasheets-pilot-010 | End-to-end datasheet for one open-access pilot dataset | data | medium | medium | document | template-003, gate-004, license-matrix-005, policy-006, croissant-007, provenance-008, outreach-009, reviewer-001 | License+Privacy, Technical |

**Acceptance criteria — key tasks**

- **template-003 (template + canonical model)**
  - [ ] Canonical metadata model documents every field in PLAN (source, accession, `accessTier`,
        `license{...,permitsDerivatives,nonCommercial,citedClause}`, `provenance{...,requiredCitation,
        gdsPolicyNote}`, `identifiability{individualLevel,germlinePresent,reIdentificationRisk,...}`,
        `ontology`, `fields[]`, `provenanceCitations[]`, `patientFacing`, `completenessScore`).
  - [ ] Markdown template covers the Datasheets-for-Datasets questionnaire, provenance, license record,
        access-tier + identifiability assessment, data dictionary, known-issues, worked (synthetic) examples.
  - [ ] Encodes the **hard invariant**: any record with `accessTier != open` or
        `identifiability.individualLevel/germlinePresent == true` is rejected (cannot be authored).
  - [ ] States the deliverable is documentation, not data; output licensed CC-BY-4.0; requires a
        provenance citation per assertion.

- **gate-004 (access-tier + identifiability + license gate)**
  - [ ] Access-tier check runs **first**: PASS only if open-access; any dbGaP/EGA/DACO/controlled/biobank
        path → EXCLUDE. Authentication-required file = EXCLUDE signal.
  - [ ] Identifiability check: germline/individual-level scanner result, k≥5 quasi-identifier check, geo
        precision, linkage risk; any hit → EXCLUDE/halt. We never de-identify ourselves.
  - [ ] License check: objective criterion — PASS only if `permitsDerivatives: true` from a cited
        clause/URL; COSMIC/OncoKB/NC/custom → FLAG/escalate per `policy-006`; unclear → EXCLUDE.
  - [ ] Inspection follows the bounded protocol (open-tier only, row cap, ephemeral, no committed samples).
  - [ ] Produces a committed PASS/FLAG/EXCLUDE artifact per dataset recording which checks ran and what
        fired; ships a worked PASS example **and** a worked EXCLUDE example (a COSMIC entry).

- **license-matrix-005 (cancer-source licensing & access matrix)**
  - [ ] Each source (TCGA/GDC, GEO, cBioPortal, DepMap, COSMIC, OncoKB, ICGC/PCAWG, CPTAC, SEER) has a
        disposition with a **cited license URL + clause** and its open vs. controlled tier delineated.
  - [ ] COSMIC and OncoKB recorded as non-commercial/custom → flag/exclude from do-first; controlled
        tiers (dbGaP/EGA/DACO) recorded as out of scope.
  - [ ] cBioPortal flagged as **per-study** verification (terms vary); DepMap flagged as per-release.
  - [ ] Matrix versioned and scheduled for re-verification each milestone.

- **pilot-010 (pilot dataset, end-to-end)**
  - [ ] Pilot is unambiguously **open-access** + permissive (e.g. a DepMap CC-BY public release file or a
        TCGA open-tier expression matrix via GDC) and chosen for a realistic acceptance path (informal
        steward channel or self-serve Zenodo DOI).
  - [ ] Passed `gate-004` (open tier confirmed; no individual-level/germline/re-identifiable content;
        license permits derivatives with cited clause) with the artifact committed.
  - [ ] Complete data dictionary, access-tier + identifiability assessment, Datasheet, and valid Croissant
        metadata produced; **every assertion carries a provenance citation**; completeness ≥ 90/100.
  - [ ] Provenance recorded (repository, accession, retrieval date, release/freeze, attribution, required
        dataset citation, GDS note; license snapshot = committed copy + SHA-256 + Wayback URL).
  - [ ] Documentation **accepted** via informal channel or Zenodo DOI with the Steward's acceptance
        artifact (`outcomes/<dataset-id>.json`) recorded — or **submitted** with the blocker surfaced.

**M0 Definition of Done:** License + Genomic-Privacy reviewer named (blocking role filled before pilot
review); template + canonical model + both gates + licensing matrix + NC/custom policy published; Croissant
generator and provenance/snapshot tool green in CI with golden fixtures; one open-access dataset documented
end-to-end and **accepted** via informal channel or Zenodo DOI (evidence artifact recorded) — or submitted
with the blocker surfaced; ≥ 1 steward-outreach thread opened; **0** privacy/safety errors.

---

## Milestone M1 — Gates hardened + first acceptances

| ID | Title | Type | Size | Risk | Deliverable | Depends on | Reviewer |
| --- | --- | --- | --- | --- | --- | --- | --- |
| cancer-dataset-datasheets-scanner-016 | Germline / identifiability scanner for inspection | code | medium | medium | pr | template-003, gate-004 | License+Privacy, Technical |
| cancer-dataset-datasheets-triage-011 | Triage 5 open-access candidates through both gates | research | medium | medium | document | gate-004, license-matrix-005, policy-006, outreach-009 | License+Privacy |
| cancer-dataset-datasheets-doc-012 | Datasheet for accepted dataset #2 (DepMap public release) | data | medium | medium | document | pilot-010, triage-011 | License+Privacy, Technical |
| cancer-dataset-datasheets-doc-013 | Datasheet for accepted dataset #3 (open GEO series) | data | medium | medium | document | pilot-010, triage-011 | License+Privacy, Technical |
| cancer-dataset-datasheets-partner-014 | Secure first confirmed contribution partner (steward/lab) | research | small | low | document | outreach-009 | Steward |

**Acceptance criteria — key tasks**

- **scanner-016 (germline / identifiability scanner)**
  - [ ] Detects and halts on individual-level genotypes, germline variant calls, raw sequence references,
        person-linked sample identifiers, day-precision DOB/DOD, and k<5 quasi-identifier classes.
  - [ ] Ships committed synthetic golden fixtures that must trip each rule + clean fixtures that must pass,
        exercised in CI (no real inspected data committed).
  - [ ] Code MIT-licensed; `pnpm build && pnpm test && pnpm lint` green; DCO signed-off; no credentials.

- **triage-011 (triage 5 candidates)**
  - [ ] Five open-access datasets evaluated with a committed gate artifact each (access-tier first, then
        identifiability, then license), applying `policy-006`.
  - [ ] Each PASS records open-tier confirmation, identifiability result, license id/URL/snapshot, and
        `permitsDerivatives: true` with cited evidence.
  - [ ] Any controlled/identifiable/NC-or-unclear dataset is EXCLUDED/FLAGGED with the concern surfaced;
        COSMIC/OncoKB never pass.

- **partner-014 (first confirmed partner)**
  - [ ] A named steward/maintainer/lab confirms they will review and accept contributed documentation.
  - [ ] Contribution mechanism documented (PR vs. submitter update vs. archive DOI vs. email).
  - [ ] Tasks for that partner updated to `verifiedNeed: true` with `requestor` set.

**M1 Definition of Done:** both gates applied to ≥ 3 datasets with committed artifacts; germline/
identifiability scanner integrated + green in CI; ≥ 2 datasets accepted onto a portal/repo/archive
(acceptance artifacts recorded); ≥ 1 confirmed partner; license-snapshot capture working (committed copy +
SHA-256 + Wayback); **0** privacy/safety errors (any occurrence halts the milestone).

---

## Milestone M2 — Source coverage & scale (TCGA/GDC · GEO · cBioPortal · DepMap)

| ID | Title | Type | Size | Risk | Deliverable | Depends on | Reviewer |
| --- | --- | --- | --- | --- | --- | --- | --- |
| cancer-dataset-datasheets-tcga-017 | Datasheet for a TCGA open-tier dataset (via GDC) | data | medium | medium | document | gate-004, croissant-007, scanner-016 | License+Privacy, Technical |
| cancer-dataset-datasheets-cbio-018 | Datasheet for a cBioPortal study (per-study terms verified) | data | medium | medium | document | gate-004, croissant-007, scanner-016 | License+Privacy, Technical |
| cancer-dataset-datasheets-effort-019 | Effort/throughput instrumentation in the outcome ledger | code | small | low | pr | pilot-010 | Technical |
| cancer-dataset-datasheets-scale-020 | Datasheets for datasets #6–#8 (cover remaining target sources) | data | large | medium | document | tcga-017, cbio-018, doc-012, doc-013, partner-014 | License+Privacy, Technical |

**Acceptance criteria — key tasks**

- **tcga-017 (TCGA open-tier via GDC)**
  - [ ] Gate confirms the file is **GDC open-access** (e.g. gene expression / copy-number / masked somatic
        mutations / de-identified clinical) — **never** controlled-access/dbGaP/germline.
  - [ ] Required TCGA citation + GDS-policy note recorded in provenance; license/attribution per GDC terms.
  - [ ] Complete data dictionary (with NCIt/OncoTree codes where applicable) + Datasheet + valid Croissant;
        completeness ≥ 90/100; provenance citation per assertion.
  - [ ] Accepted upstream (acceptance artifact) — or submitted with blocker surfaced.

- **cbio-018 (cBioPortal study)**
  - [ ] **Per-study license verified** with cited clause (terms vary per study); TCGA-derived/open studies
        preferred; any non-open study EXCLUDED.
  - [ ] Datasheet + Croissant produced; provenance + required citation recorded; completeness ≥ 90/100.
  - [ ] Delivered via a GitHub PR to the study/metadata repo (acceptance = merge commit) or steward channel.

- **scale-020 (datasets #6–#8)**
  - [ ] Each dataset passes both gates with committed artifacts; collectively ensure **≥ 1 accepted
        datasheet exists for each of TCGA/GDC, GEO, cBioPortal, DepMap** across M0–M2.
  - [ ] Each accepted upstream with acceptance artifact; per-dataset effort logged to show reduction vs.
        the M0/M1 baseline median.

**M2 Definition of Done:** ≥ 1 accepted datasheet for **each** of TCGA/GDC, GEO, cBioPortal, and DepMap;
≥ 5 datasets accepted cumulatively; germline/identifiability scanner in the standard flow; measurable
median per-dataset effort reduction vs. the M0/M1 baseline; **0** privacy/safety errors.

---

## Milestone M3 — Reuse outcomes & sustainability

| ID | Title | Type | Size | Risk | Deliverable | Depends on | Reviewer |
| --- | --- | --- | --- | --- | --- | --- | --- |
| cancer-dataset-datasheets-reuse-021 | Track and verify external reuse events | research | small | low | document | doc-012, doc-013, scale-020 | Steward |
| cancer-dataset-datasheets-refresh-022 | Staleness/version-drift refresh process (GDC/DepMap re-releases) | maintenance | small | low | document | provenance-008, tcga-017 | Maintainer |
| cancer-dataset-datasheets-matrix-recheck-023 | Re-verify the licensing matrix (licenses/policies change) | maintenance | small | medium | document | license-matrix-005 | License+Privacy |

**Acceptance criteria — key tasks**

- **reuse-021 (reuse tracking)**
  - [ ] ≥ 2 verifiable external reuse events recorded (citation / portal acceptance / merged PR / DOI
        reference); each links to externally verifiable evidence (no self-reported reuse).

- **refresh-022 (staleness/refresh)**
  - [ ] Process detects when a documented dataset has drifted from its recorded release/freeze (GDC data
        releases, DepMap quarterly); validation flags schema drift; stale docs become `maintenance` tasks.

**M3 Definition of Done:** ≥ 2 verifiable reuse events; ≥ 6 datasets accepted cumulatively; refresh/
version-drift process documented; licensing matrix re-verified; steward identified for ongoing liaison.

---

## Milestone M4 — (Optional, gated) Patient/advocate plain-language explainers · `riskTier: high`

> **Hard precondition:** oncologist + patient-advocate reviewers secured and the "not medical advice"
> framing ratified. Not started until M3 quality is proven. Education only — never advice.

| ID | Title | Type | Size | Risk | Deliverable | Depends on | Reviewer |
| --- | --- | --- | --- | --- | --- | --- | --- |
| cancer-dataset-datasheets-onco-reviewer-024 | Secure oncologist + patient-advocate reviewer panel | research | small | high | document | — | Maintainer |
| cancer-dataset-datasheets-explainer-025 | Plain-language "what this dataset is" explainer for an accepted datasheet | writing | medium | high | document | onco-reviewer-024, scale-020 | Oncologist, Advocate, Technical |

**Acceptance criteria — key tasks**

- **explainer-025 (patient/advocate explainer)**
  - [ ] Built only from an already-accepted datasheet; education only — **no** clinical/treatment/prognostic
        guidance, no biomarker/efficacy claims, no rankings.
  - [ ] Carries a prominent **"not medical advice"** banner and links to the source datasheet + dataset.
  - [ ] **Oncologist + patient-advocate sign-off recorded** before merge (`riskTier: high`); advocate
        readability check passed; provenance citation per assertion retained.

**M4 Definition of Done:** ≥ 1 explainer shipped with full oncologist + advocate sign-off and banner; **0**
explainers shipped without sign-off.

---

## Backlog / future

| ID | Title | Type | Size | Risk | Deliverable | Notes |
| --- | --- | --- | --- | --- | --- | --- |
| cancer-dataset-datasheets-cptac-026 | Datasheet for an open CPTAC proteomics dataset | data | medium | medium | document | Extends coverage to proteomics; verify open tier |
| cancer-dataset-datasheets-seer-027 | Datasheet for SEER **aggregate** incidence statistics | data | medium | medium | document | Aggregate only; individual-level SEER research data OUT OF SCOPE |
| cancer-dataset-datasheets-zenodo-028 | Zenodo metadata adapter + DOI provenance | code | medium | low | pr | Self-serve archive acceptance channel |
| cancer-dataset-datasheets-i18n-029 | Translate a delivered datasheet (domain reviewer) | translation | small | medium | translation | Widens reuse; needs bilingual domain reviewer |
| cancer-dataset-datasheets-dash-030 | Outcome dashboard for accepted docs + reuse events | code | medium | low | pr | Reads the outcome ledger; supports success metrics |

---

## Example task JSON

```json
{
  "id": "cancer-dataset-datasheets-reviewer-001",
  "title": "Name/secure the License + Genomic-Privacy reviewer (blocking gate role)",
  "project": "cancer-dataset-datasheets",
  "type": "research",
  "lane": "donated",
  "priority": "high",
  "domain": ["cancer-research", "open-science", "bioinformatics", "data-governance"],
  "riskTier": "medium",
  "urgent": false,
  "deliverable": "document",
  "tokenEstimate": "small",
  "status": "open",
  "context": "This project documents only open-access, aggregate/de-identified cancer datasets (TCGA via GDC, GEO, cBioPortal, DepMap). Controlled-access data (dbGaP, EGA, ICGC DACO, individual-level biobanks) and any identifiable patient data are out of scope, and COSMIC/OncoKB are non-commercial/custom-licensed. Because the central safety control is a non-skippable access-tier + identifiability + license gate, a qualified reviewer must own that gate before any dataset is documented. Until this role is filled, no dataset can pass the gate and all tasks remain verifiedNeed:false.",
  "objective": "Identify and secure a named, qualified License + Genomic-Privacy reviewer (with a documented qualification to read NIH GDS / dbGaP-EGA access models and open/non-commercial licenses and to apply the identifiability methodology), and record the appointment and rotation plan in PLAN.md Governance.",
  "acceptanceCriteria": [
    "At least one named reviewer is secured, with a recorded qualification covering genomic data-sharing policy (NIH GDS), controlled- vs open-access tiers (dbGaP/EGA/DACO), and open/non-commercial license interpretation (CC, COSMIC, OncoKB, custom).",
    "The reviewer's responsibilities are documented: access-tier check first, identifiability/germline check, then license check with cited permitsDerivatives; authority to EXCLUDE/FLAG.",
    "A rotation plan ensures at least one qualified reviewer is always available, or triage/documentation halts.",
    "PLAN.md Governance section is updated with the named reviewer (or explicit TO BE SECURED status) and the appointment is dated.",
    "It is explicitly recorded that no dataset passes the gate and all tasks stay verifiedNeed:false until this role is filled."
  ],
  "resources": [
    "C:\\Users\\jason\\AppData\\Local\\Temp\\claude\\C--code-elyos\\5eca0d44-6b8b-4c30-9696-37a524cb249a\\scratchpad\\plans\\cancer-dataset-datasheets\\PLAN.md",
    "C:\\code\\elyos\\docs\\good-deed-definition.md",
    "C:\\code\\elyos\\planning\\ROADMAP.md",
    "NIH Genomic Data Sharing (GDS) policy",
    "NCI Genomic Data Commons (GDC) data access policies"
  ],
  "output": "A short governance document naming (or marking TO BE SECURED) the License + Genomic-Privacy reviewer, their qualification, responsibilities, and rotation plan, committed to the project and reflected in PLAN.md.",
  "requestor": "TO BE SECURED",
  "verifiedNeed": false,
  "outputLicense": "CC-BY-4.0"
}
```

---

## Task count & coverage

- **23 scheduled tasks** across M0–M4 (M0: 9 · M1: 5 · M2: 4 · M3: 3 · M4: 2) + **5 backlog tasks** = **28 total**,
  each now materialized as a schema-valid `tasks/<id>.json` (see *Generated task index* below).
- Type mix: research, writing, design-spec, code, data, maintenance, translation — all `deliverable` ∈
  {`pr`, `document`, `translation`}; **never** `dataset` (data is out of scope).
- All tasks `lane: donated`, `verifiedNeed: false`, `requestor: TO BE SECURED` until a partner is confirmed.
- Patient-facing tasks (`explainer-025`, `onco-reviewer-024`) are `riskTier: high` and gated behind
  oncologist + patient-advocate sign-off; all other cancer-data tasks are `riskTier: medium` or `low`.

---

## Acceptance criteria — remaining tasks

The milestone tables above carry full acceptance bullets for the "key tasks"; the remaining rows now
carry their checkable criteria in `tasks/<id>.json`. Summarized here for reference (authoritative copy
lives in the JSON):

- **policy-006 (NC/custom-license + redistribution policy)** — COSMIC/OncoKB and any NC/custom source are
  flagged/escalated and never treated as open; document-only, never host/mirror/redistribute the data;
  objective FLAG-vs-EXCLUDE rule with worked examples; versioned and cross-referenced from `license-matrix-005`/`gate-004`.
- **croissant-007 (Croissant generator + validator)** — emits valid Croissant ML (JSON-LD) from a datasheet
  conforming to the canonical model; spec validator fails on non-conformance; golden fixtures green in CI; MIT, DCO, no secrets.
- **provenance-008 (provenance + snapshot tool)** — captures repository/accession/retrieval-date/release/citation/GDS
  note; license snapshot = committed copy + SHA-256 + Wayback URL; fixtures green in CI; MIT, DCO, no secrets.
- **outreach-009 (steward outreach + shortlist)** — open-access-only candidate shortlist with acceptance channels;
  ≥ 1 outreach thread (or documented plan with TO-BE-SECURED stewards); no guardrail-violating candidate; `verifiedNeed` stays false until confirmed.
- **doc-012 / doc-013 (DepMap / open GEO datasheets)** — open-tier + license verified; passed `gate-004`; full data
  dictionary + assessment + Datasheet + valid Croissant; completeness ≥ 90/100; provenance + snapshot recorded; accepted via the source channel (artifact) or submitted with blocker surfaced.
- **effort-019 (effort instrumentation)** — outcome ledger records per-dataset effort; reports median + reduction vs.
  M0/M1 baseline; MIT, CI green, DCO, no secrets.
- **matrix-recheck-023 (licensing-matrix re-verify)** — each disposition re-checked vs. current cited clause/URL;
  version + date bumped; changes propagated to `gate-004`/`policy-006`; COSMIC/OncoKB + controlled tiers stay flagged/out-of-scope.
- **onco-reviewer-024 (oncologist + advocate panel)** — named oncologist + patient-advocate secured (or TO BE SECURED);
  sign-off hard gate documented; "not medical advice" framing ratified; PLAN.md Governance dated; no explainer starts until filled.
- **cptac-026 / seer-027 (CPTAC proteomics / SEER aggregate)** — open tier verified (SEER **aggregate only**;
  individual-level OUT OF SCOPE); passed `gate-004`; full datasheet + Croissant; completeness ≥ 90/100; provenance + snapshot + required citation; accepted upstream or submitted with blocker.
- **zenodo-028 (Zenodo adapter + DOI provenance)** — maps canonical model to valid Zenodo deposition metadata
  (documentation only); records version DOI as acceptance evidence; fixtures green in CI; MIT, DCO, no secrets.
- **i18n-029 (translate a delivered datasheet)** — source is an already-delivered datasheet; bilingual domain-reviewer
  sign-off; provenance/license records preserved; **source-compatible license (never relicense copyrighted source as CC-BY)**; no new claims; guardrails retained.
- **dash-030 (outcome dashboard)** — reads the committed outcome ledger / `outcomes/<dataset-id>.json`; surfaces
  accepted-count-per-source / reuse / completeness / effort from verifiable evidence only; MIT, CI green, DCO, no inspected data committed.

## Fan-out notes

- Per-dataset rows (`pilot-010`, `doc-012`, `doc-013`, `tcga-017`, `cbio-018`, `cptac-026`, `seer-027`) are kept as
  **one representative task each** — the concrete datasets are selected at triage and are **not enumerated** in
  PLAN/TASKS, so none are fabricated. They expand to additional `tasks/*.json` only when a specific open-access
  dataset is confirmed through `gate-004`.
- `scale-020` (datasets #6–#8) stays a **single bounded task**; it fans out into per-dataset JSONs only once the
  specific sources to complete TCGA/GDC · GEO · cBioPortal · DepMap coverage are chosen at triage.
- `i18n-029` (translation) stays a **single representative task**; no target language is invented in advance —
  it expands when a bilingual domain reviewer/language is confirmed.
- No dimension in PLAN/TASKS is explicitly enumerated for mechanical fan-out, so the generated set is **one JSON per
  backlog row** (28 total incl. the seed).

## Generated task index

All rows are materialized as schema-valid `tasks/<id>.json` (validated against `packages/schema` taskSchema;
`filename == id`; no duplicates; no extra keys). The pre-existing seed is `cancer-dataset-datasheets-reviewer-001`.

- **M0:** `cancer-dataset-datasheets-reviewer-001` (seed) · `…-template-003` · `…-gate-004` · `…-license-matrix-005` ·
  `…-policy-006` · `…-croissant-007` · `…-provenance-008` · `…-outreach-009` · `…-pilot-010`
- **M1:** `…-scanner-016` · `…-triage-011` · `…-doc-012` · `…-doc-013` · `…-partner-014`
- **M2:** `…-tcga-017` · `…-cbio-018` · `…-effort-019` · `…-scale-020`
- **M3:** `…-reuse-021` · `…-refresh-022` · `…-matrix-recheck-023`
- **M4 (gated, `riskTier: high`):** `…-onco-reviewer-024` · `…-explainer-025`
- **Backlog:** `…-cptac-026` · `…-seer-027` · `…-zenodo-028` · `…-i18n-029` · `…-dash-030`
