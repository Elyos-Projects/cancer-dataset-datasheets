# PLAN — cancer-dataset-datasheets

> Status: Draft · Version: 0.1.0 · Last updated: 2026-06-28 · Owner: TBD (maintainer) · Lane: donated · Risk tier: medium (patient-facing education, if attempted, is high)

> **BINDING CANCER GUARDRAILS (read first — these override any convenience or throughput goal).**
> This project documents **only open-access / aggregate / de-identified** cancer data. **Controlled-access
> data (dbGaP, EGA, ICGC DACO-gated, individual-level biobanks) and ANY identifiable patient data are
> categorically OUT OF SCOPE** — we neither access, request, mirror, nor describe row-level patient
> records. Per-source license is **verified before any work begins** (TCGA/GDC open-tier and GEO open
> are accepted; **COSMIC and OncoKB are non-commercial / custom-licensed → flag/escalate, do not treat
> as open**). **No medical advice**: any patient-facing content is *education only*, carries a "not
> medical advice" banner, and ships **only** after **oncologist + patient-advocate review** (`riskTier:
> high`). **Provenance on every assertion** — each factual claim in a datasheet cites its source.

## Executive summary

Open cancer datasets — The Cancer Genome Atlas (TCGA, via the NCI Genomic Data Commons / GDC), the
Gene Expression Omnibus (GEO), cBioPortal, and the Cancer Dependency Map (DepMap) — are among the
most valuable public resources in oncology. Yet they are frequently reused with **incomplete, scattered,
or out-of-date documentation**: which access tier a file belongs to, whether the license actually
permits the reuse a researcher has in mind, exactly which patients/samples are included and how they
were processed, what was masked or filtered, and how to attribute the source correctly. These gaps
slow cancer research, cause licensing mistakes (e.g. treating non-commercial COSMIC/OncoKB data as
freely reusable), and — most seriously — create avenues for **re-identification or controlled-data
mishandling** when the access-tier boundary is misunderstood.

This project produces standardized, source-verified **documentation and machine-readable metadata**
for **open-access** cancer datasets: a **Datasheet for Datasets**, a provenance + license record, a
plain-but-precise data dictionary, an access-tier & identifiability assessment, and Croissant ML
metadata — and contributes it back to the dataset's portal, repository, or a citable archive.

**The deliverable is documentation, never the data.** We never republish, mirror, re-host, transform,
re-identify, or aggregate the underlying data; we describe it. Each dataset is a self-contained unit of
work suitable for one donated AI session plus mandatory human review. The hard, non-skippable design
principle is the **two-part gate**: (1) an **access-tier + identifiability gate** that admits only
open-access, aggregate/de-identified data and rejects anything controlled-access, germline/individual-level,
or re-identifiable; and (2) a **per-source license gate** that records an explicit `permitsDerivatives`
decision with cited evidence and flags non-commercial/custom terms rather than guessing.

Risk tier is **medium**. The risks are not primarily technical — they are **patient-safety, privacy,
and legal**: mischaracterizing a license, documenting data that should never have been in scope
(controlled-access or re-identifiable), or letting an unreviewed assertion read as medical advice. The
plan front-loads the access-tier/identifiability/license/PII review as a blocking gate and routes any
patient-facing surface to oncologist + advocate sign-off as `riskTier: high`.

## Problem & beneficiaries

**Who is helped.**
- **Cancer researchers and bioinformaticians** who want to reuse TCGA/GEO/cBioPortal/DepMap data but
  lose time deciphering access tiers, processing pipelines, sample inclusion, and license terms.
- **Patient advocates and advocacy organizations** who need to understand, in plain language, what a
  public cancer dataset does and does not contain (education only — never advice).
- **Data stewards at the source repositories** (GDC, NCBI/GEO, cBioPortal, Broad/DepMap), who benefit
  from better, attributable, standards-aligned documentation of resources they already maintain.
- **Students and reproducibility efforts**, who need trustworthy provenance to re-run analyses.
- **The cancer research commons** broadly: clearer licensing reduces accidental misuse and increases
  correct, attributed reuse.

**The verified need.** The *general* need — that poor/scattered documentation and license ambiguity are
real barriers to safe reuse of open cancer data — is well established in the bioinformatics literature
and in the data-sharing policies of NIH/NCI themselves. However, the **per-dataset, per-partner need is
TO BE SECURED**: we have **not** yet confirmed a named repository, lab, or steward who has agreed to
*accept* contributed documentation. Until a specific steward/maintainer confirms they will review and
accept contributions, individual tasks carry **`verifiedNeed: false`**. This honesty is required because
"delivered, not merged" means the output must be *accepted by a beneficiary*, not merely produced.

**Partner org.** TO BE SECURED. Candidate channels include: a GEO submitter/lab willing to accept an
improved series-level datasheet; the cBioPortal community (GitHub) for study-level metadata; DepMap's
feedback/forum channels; data-curation groups (e.g. ELIXIR, Bioconductor `ExperimentHub` curators); and
self-serve citable archives (Zenodo, with a DOI) where acceptance does not depend on a third party. M0
includes explicit steward-outreach work; **no partner is assumed**.

## Goals and non-goals

**Goals**
- Produce a reusable, standards-aligned **cancer-dataset datasheet template** + canonical metadata model
  (Datasheet-for-Datasets questionnaire, provenance, license record, access-tier + identifiability
  assessment, data dictionary, Croissant ML metadata).
- Maintain an authoritative, source-cited **cancer-source licensing matrix** (TCGA/GDC, GEO, cBioPortal,
  DepMap, plus COSMIC, OncoKB, ICGC/PCAWG, CPTAC, SEER) so every triage applies a fixed rule.
- For each in-scope **open-access** dataset, deliver complete, source-verified documentation that
  measurably improves safe reuse, with **provenance on every assertion**.
- Make the **access-tier + identifiability gate** and the **license gate** non-skippable, auditable,
  committed artifacts.
- Optionally (gated, high-risk), produce **plain-language "what this dataset is" explainers** for patients
  and advocates — education only, "not medical advice," oncologist + advocate reviewed.

**Non-goals**
- We do **not** host, mirror, clean, transform, re-aggregate, or republish any underlying data.
- We do **not** access, request, or describe **controlled-access** data (dbGaP, EGA, ICGC DACO,
  individual-level biobanks) — categorically out of scope.
- We do **not** attempt, document, or facilitate **re-identification**, linkage of de-identified records,
  or anything that increases re-identification risk.
- We do **not** document datasets with unclear/incompatible licenses (incl. treating COSMIC/OncoKB
  non-commercial data as open) — these are flagged/escalated, never best-guessed.
- We do **not** give medical advice, treatment guidance, prognosis, or clinical interpretation. Any
  patient-facing text is education with a "not medical advice" banner and expert sign-off.
- We do **not** produce analytical conclusions, rankings, biomarker claims, or "what the data means"
  interpretation beyond neutral, sourced description.
- We do **not** auto-publish to any portal; a human submits after review.

## Success metrics (outcomes)

Outcome-based and beneficiary-centric. Vanity metrics ("datasheets written") are explicitly excluded —
only *accepted, used, and safe* documentation counts.

| Metric | Baseline | Target (first 6 months) |
| --- | --- | --- |
| Open-access cancer datasets with documentation **accepted onto the source portal/repo/archive** (last-mile delivered) | 0 | 6 accepted |
| Access-tier/identifiability + license gate applied to triaged datasets | n/a | 100% of triaged-in datasets have a committed gate artifact |
| **Privacy/safety errors** (controlled-access or re-identifiable data documented; license misclassified) | n/a | **0** — a single occurrence triggers a full stop + review |
| Reuse signal: documented datasets cited/forked/referenced by a third party | 0 | ≥ 2 with verifiable external reuse |
| Confirmed contribution partners (steward/maintainer/lab accepting contributions) | 0 | ≥ 1 secured |
| Datasheets passing technical + license/privacy review with no rework needed on safety items | n/a | ≥ 90% first-pass on safety items; **0** safety items deferred |
| Patient-facing explainers shipped **without** oncologist + advocate sign-off | n/a | **0** (hard gate) |

**Quantifying "improves reuse" (so DoDs are verifiable).** Each datasheet gets a
**documentation-completeness score (0–100)**: fraction of canonical-metadata fields populated *and
source-verified* — data-dictionary coverage of all documented fields, provenance complete, license
recorded with `permitsDerivatives` + cited clause, access-tier + identifiability assessment complete,
Datasheet sections answered, valid Croissant emitted, **every assertion carrying a provenance citation**.
Target: every delivered datasheet reaches **≥ 90/100** vs. a recorded **before-score** captured at triage
on the dataset as-published. The before/after pair is stored in the dataset's gate/provenance artifact.

**Attribution of outcomes.** A "reuse event" must be externally verifiable (a citation, a portal/repo
acceptance record, a merged PR, a Zenodo DOI referenced elsewhere). Self-reported reuse does not count.

**What "accepted" means, per channel** (the Steward records one canonical acceptance-evidence artifact
per dataset, `outcomes/<dataset-id>.json`: channel, URL/permalink/DOI, timestamp, completeness
before/after):
- **GEO / NCBI:** submitter or curator accepts the improved series documentation (written confirmation
  or an updated series record); evidence = the confirmation reference / record URL.
- **cBioPortal (GitHub):** merged PR to the study/metadata repo; evidence = merge commit URL.
- **DepMap:** maintainer/forum acceptance of the dataset documentation; evidence = the archived thread / issue.
- **Zenodo (self-serve archive):** published metadata record / version DOI; evidence = the DOI.
- **Lab / institutional channel:** explicit written acceptance from the named steward; evidence = archived message.
- "Submitted but unconfirmed" never counts as accepted.

## Scope

**In scope**
- Documentation artifacts for **open-access** cancer datasets: Datasheet-for-Datasets, provenance record,
  license record, **access-tier + identifiability assessment**, data dictionary, known-issues, worked
  examples (synthetic/illustrative only), Croissant ML metadata (JSON-LD).
- The **cancer-source licensing matrix** and the access-tier/identifiability/license gate checklists.
- Small, dependency-light **validation/inspection scripts** that re-check a documented dataset's *schema*
  against its datasheet and emit a quality report — operating only on open-tier, aggregate data under the
  bounded inspection protocol below.
- A **germline / identifiability scanner** used during inspection to halt on any individual-level or
  re-identifiable signal.
- Optionally (gated, `riskTier: high`): plain-language patient/advocate explainers — education only.

**Candidate dataset focus.** The initial pool is the **open-access tiers** of TCGA (GDC open data),
public GEO series (open data only), cBioPortal studies (per-study terms verified), and DepMap public
releases. A source-cited licensing matrix (`license-matrix-005`) is the funnel; the gate is the filter.
COSMIC, OncoKB, and any controlled-access resource are **explicitly excluded from the do-first pool**.

**Out of scope**
- The data itself (no hosting/mirroring/transformation/cleaning/re-aggregation/republishing).
- **All controlled-access data** (dbGaP, EGA, ICGC DACO-gated, individual-level biobanks) — not accessed,
  requested, or described.
- Any **identifiable or re-identifiable patient data**; any germline/individual-level genomic record; any
  attempt to link or re-identify de-identified data.
- Datasets with **non-commercial / custom / unclear** licenses treated as open (COSMIC, OncoKB, etc.) —
  flagged/escalated, never assumed reusable.
- Clinical interpretation, prognosis, treatment guidance, biomarker/efficacy claims, rankings, or
  "what the data means" analysis.
- Automated, unattended publishing to any portal.
- Any task that primarily serves a for-profit entity's private interest.

## Solution approach & architecture

This is a **content/data-documentation project with light software** (template + validators + a
germline/identifiability scanner + Croissant generator). It is **not** a data pipeline that moves data.

**Pipeline (per dataset)**
1. **Access-tier + identifiability gate** — confirm the dataset/file is **open-access** (not dbGaP/EGA/
   DACO/controlled), aggregate or de-identified, with **no germline/individual-level genomic content**.
   PASS only if open-tier + non-re-identifiable. Record decision + which checks ran.
2. **License gate** — apply the cancer-source licensing matrix; record license id + URL + text snapshot,
   and an explicit `permitsDerivatives: true` with a cited clause/URL. COSMIC/OncoKB/NC/custom → FLAG/escalate.
3. **Provenance capture** — source URL, repository, accession/ID, version/release, retrieval date, data
   freeze/cutoff, update cadence, license URL + snapshot, required attribution + required citation (e.g.
   the TCGA/dataset citation), and the GDS/consent-policy note for the source.
4. **Access-tier & identifiability assessment** — documents the tier, the de-identification model the
   *publisher* applied (e.g. GDC open-tier processing, masked somatic mutations), residual-risk notes,
   and an explicit "no controlled/germline/individual-level content documented here" attestation.
5. **Data-dictionary** — field/column name, type, units, allowed values, nullability, ontology codes
   (NCIt / OncoTree / Disease Ontology where applicable), description, caveats — derived by *inspecting*
   schema/aggregate data under the bounded protocol below, never by republishing.
6. **Datasheet** — Datasheets-for-Datasets questionnaire: motivation, composition, collection,
   preprocessing/cleaning, uses, distribution, maintenance — **with provenance on every assertion**.
7. **Croissant metadata** — machine-readable JSON-LD (Croissant ML), with cancer/bio fields where useful.
8. **Validation script** — re-checks the documented *schema* (types/allowed values/row-count of the
   aggregate) and emits a quality report; never extracts or commits real rows.
9. **Review & contribute** — License + Genomic-Privacy reviewer (mandatory) + Technical reviewer; then a
   human submits the contribution. Any patient-facing surface additionally requires oncologist + advocate.

**Canonical metadata model.** One internal object per dataset is the source of truth; all outputs
(Datasheet, Croissant, portal-specific) are *projections* of it. Fields include:
`id`, `title`, `source` (`gdc-tcga | geo | cbioportal | depmap | other`), `accession`, `sourceUrl`,
`accessTier` (`open | controlled` — **only `open` admissible**), `license {id, url, permitsDerivatives:boolean,
nonCommercial:boolean, snapshotRef, citedClause}`, `provenance {repository, accession, retrievedAt,
release, dataFreeze, updateCadence, attribution, requiredCitation, gdsPolicyNote}`,
`identifiability {individualLevel:boolean, germlinePresent:boolean, reIdentificationRisk:string,
publisherDeidentification:string, attestation:string}`, `ontology {disease[], assay[]}`,
`fields[] {name, type, units, allowedValues, nullable, description, caveats, ontologyRef}`,
`knownIssues[]`, `examples[]` (synthetic/illustrative only), `provenanceCitations[]` (one per assertion),
`specVersions {croissant}`, `patientFacing:boolean`, `completenessScore {before, after}`.
**Hard invariant:** any record with `accessTier != open`, `identifiability.individualLevel == true`, or
`identifiability.germlinePresent == true` is rejected by the gate and never produced.

**Tech stack.** TypeScript, ESM, pnpm workspaces (Elyos conventions). Validators, scanner, and Croissant
generator are small Node packages with minimal dependencies. Documentation authored in Markdown +
JSON/JSON-LD. No runtime services; everything runs locally or in CI.

**Pinned spec versions** (recorded in `specVersions`, bumped only via a deliberate task):
- **Datasheets for Datasets** — Gebru et al. (2021) questionnaire.
- **Croissant ML** — v1.0 (MLCommons); validate against the v1.0 JSON-LD context/SHACL.
- **Ontologies** — NCI Thesaurus (NCIt), OncoTree, Disease Ontology, EDAM (assay/format) — versions
  recorded per datasheet.

**Bounded dataset-inspection access protocol** (makes "describe but never store, never re-identify"
enforceable — inspection is the only point we touch data):
- **Open-tier only.** Inspect only confirmed open-access endpoints/files. Never authenticate to, request,
  or download controlled-access data. If a file requires dbGaP/EGA/DACO authorization, **stop**.
- **Schema/aggregate first.** Prefer published schemas, data dictionaries, and aggregate summaries;
  for type/value inference, stream and sample rather than downloading full files.
- **Row cap.** Inspect at most a bounded sample (default **1,000 rows** or first ~5 MB, whichever is
  smaller) of *aggregate/de-identified* data — never a full extract, never individual-level records.
- **Germline / individual-level halt.** Run the identifiability scanner on every inspection; on any
  signal of individual-level, germline, or re-identifiable content, **halt immediately**, discard the
  sample, and route to EXCLUDE/FLAG.
- **Local-only and ephemeral.** Any bytes read live only in the contributor's local scratch for the
  session and are deleted afterward; never written into the repo, CI artifacts, receipts, or logs.
- **No committed samples.** Worked `examples[]` use synthetic or trivially public illustrative rows, or
  cite a value by reference — we never paste real patient/sample rows into committed documentation.

**Key decisions.**
- Canonical-model-first so we never hand-maintain parallel metadata formats.
- Both gates are *blocking* committed checklist artifacts, not informal judgement.
- The access-tier/identifiability gate runs **before** the license gate (privacy/safety dominates legality).
- Adapters/generators are output-only; a human performs the actual portal submission.
- Patient-facing content is a separate, gated track (`riskTier: high`), never bundled into core datasheets.

## Data, licensing & compliance

**THIS IS THE CRITICAL SECTION. It leads with the binding cancer guardrails; everything below is
subordinate to them.**

### Binding guardrails (restated, non-negotiable)
1. **Open-access only.** Only data in a source's documented **open / public tier** is in scope.
   **Controlled-access (dbGaP, EGA, ICGC DACO, individual-level biobanks) is categorically out of scope** —
   not accessed, requested, mirrored, or described.
2. **Aggregate / de-identified only.** No individual-level patient records, **no germline data**, nothing
   re-identifiable. We never attempt or facilitate re-identification or record linkage.
3. **Per-source license verified before any work.** Accepted sources must permit reuse *and derivative
   documentation*. **COSMIC and OncoKB are non-commercial / custom-licensed → FLAG/escalate, never treated
   as open.** Unclear terms → EXCLUDE, never guess.
4. **No medical advice.** Patient-facing content is *education only*, carries a "not medical advice"
   banner, and ships only after **oncologist + patient-advocate** sign-off (`riskTier: high`).
5. **Provenance on every assertion.** Every factual claim in a datasheet cites a source.

### Per-source licensing & access posture (the matrix, summarized — full detail in `license-matrix-005`)

| Source | In-scope tier | Access/license posture | Disposition |
| --- | --- | --- | --- |
| **TCGA via GDC** | GDC **open-access** data (e.g. gene expression, copy-number, masked somatic mutations, de-identified clinical/biospecimen) | Open data: no use restrictions under the **NIH Genomic Data Sharing (GDS)** policy; TCGA publication/citation guidelines apply; **controlled-access (raw sequence, germline) via dbGaP is OUT OF SCOPE** | **ACCEPT** open-tier; document required TCGA citation; never touch controlled tier |
| **GEO (NCBI)** | Public/open series (processed/aggregate) | NCBI public data, generally unrestricted reuse; **per-series check** — some series link raw/individual-level data in SRA/dbGaP (controlled) → exclude those linked files | **ACCEPT** open series; verify each series; exclude any controlled-linked components |
| **cBioPortal** | Studies whose underlying data are open; portal software is open-source | Data redistributed under **each original study's terms** — must be verified per study; some derive from TCGA (open) | **ACCEPT per-study only after terms verified**; otherwise FLAG |
| **DepMap (Broad)** | Public DepMap/CCLE releases | Recent public releases under **CC BY 4.0** (verify the specific release/file; some files carry distinct terms) | **ACCEPT** with release-specific license recorded; verify each release |
| **COSMIC** | — | **Non-commercial license**; commercial reuse requires a paid license; redistribution restricted | **FLAG / EXCLUDE from do-first**; escalate to license policy; never treat as open |
| **OncoKB** | — | **Custom license**, free for research only, redistribution/commercial restricted | **FLAG / EXCLUDE from do-first**; escalate; never treat as open |
| **ICGC / PCAWG** | Open-tier summaries only | Mixed: open + **DACO-controlled** individual-level data | **ACCEPT open-tier only**; controlled tier OUT OF SCOPE |
| **CPTAC (PDC)** | Open proteomics/aggregate | Generally open; verify per dataset | **ACCEPT** open-tier after verification |
| **SEER** | Public **aggregate** incidence/mortality statistics | Aggregate stats public; **SEER research (individual-level) data requires a signed agreement** → out of scope | **ACCEPT aggregate stats only**; individual-level OUT OF SCOPE |

*Versions/dates of each license are captured per dataset; the matrix is re-verified each milestone.*

**Objective "permits derivatives" criterion.** A dataset PASSes the license check only if its license is
accepted by the matrix/policy **and** `license.permitsDerivatives: true` is recorded with a cited
clause/URL evidencing derivative documentation/metadata is allowed. Missing evidence, an unparseable
license, non-commercial terms without a decided policy, or `permitsDerivatives` that cannot be set `true`
from the source text = **FLAG/EXCLUDE**, never default-allow.

**Provenance model.** Every documented dataset records: source repository, accession/ID, source URL,
retrieval timestamp, release/version, data freeze/cutoff, update cadence, license id + URL + a captured
**snapshot** of the license text (committed copy + SHA-256 hash + Wayback save URL), required attribution,
the **required dataset citation** (e.g. TCGA/DepMap citation), and the source's **GDS/consent-policy note**.
Provenance is part of the committed deliverable, and **every assertion in the datasheet carries a
provenance citation** (`provenanceCitations[]`).

**Privacy / PII / identifiability stance (genomics-aware).** This is stronger than generic PII handling
because de-identified genomic data carries documented re-identification risk:
- **Access-tier check first.** Confirm open-tier; reject any controlled-access path immediately.
- **Germline / individual-level scanner.** During inspection, flag any per-individual genotype, germline
  variant calls, raw sequence, sample-level identifiers tied to a person, dates of birth/death at day
  precision, or quasi-identifier combinations (age + sex + rare-diagnosis + geography) below a **k ≥ 5**
  equivalence class. Any hit → EXCLUDE/FLAG and halt inspection.
- **Linkage risk.** Flag datasets trivially linkable to an external person-level key. We never perform
  linkage; we only flag risk so the dataset is excluded.
- **Publisher de-identification, not ours.** We never de-identify, anonymize, or aggregate data ourselves
  (that is transforming data — out of scope). We document the *publisher's* de-identification and reject
  anything inadequately de-identified at source.
The scanner output (which checks ran, what fired) is recorded in the committed gate artifact.

**Attribution & output licensing.** All documentation attributes the original source per its license and
required citation, links to the original, and clearly states the **documentation — not the data** is our
contribution. Documentation/metadata output is licensed **CC-BY-4.0**; validator/scanner/generator **code
is MIT**. Where a source requires a specific citation (TCGA, DepMap), that citation is reproduced verbatim.

## Quality, review & risk gates

**Risk tier: medium** for core documentation (factual accuracy about sensitive cancer data, license
interpretation, and identifiability judgement). **`high`** for any **patient-facing** content.

**Required review before a deed is "done":**
- **License + Genomic-Privacy reviewer** (mandatory, every dataset; non-skippable hard gate): confirms
  open-access tier, no individual-level/germline/re-identifiable content, license permits
  reuse+derivatives, COSMIC/OncoKB-style terms correctly flagged. **No deed ships without this sign-off.**
  This role must be filled **before the M0 pilot is reviewed**.
- **Technical reviewer:** confirms the data dictionary, access-tier assessment, Datasheet, Croissant
  metadata, and validation script are accurate, that **every assertion has a provenance citation**, and
  that CI is green.
- **Oncologist + patient-advocate reviewers** (mandatory for *any* patient-facing surface; escalates the
  task to `riskTier: high`): confirm the content is education only, accurate, non-stigmatizing, carries
  the "not medical advice" banner, and contains no clinical/treatment/prognostic guidance.

**Test fixtures & golden files (so "CI green" means something).** Each tool ships with committed test
assets exercised in CI, using only **synthetic/public** fixtures (never real inspected data):
- **Croissant validator** — golden JSON-LD fixtures (known-valid must pass; malformed must fail) against
  pinned Croissant v1.0.
- **Germline/identifiability scanner** — synthetic fixtures that must trip each rule (individual-level,
  germline, low-k quasi-identifiers, controlled-access markers) and clean fixtures that must pass.
- **Gate checklist** — a worked example PASS artifact and a worked EXCLUDE artifact (e.g. a COSMIC entry)
  committed as references.

**Definition of Shipped.** Documentation + machine-readable metadata **accepted onto the dataset's
portal/repo/archive** (per the per-channel acceptance definitions in Success metrics), with: open-access
tier confirmed, no controlled/identifiable content, a verified license with cited `permitsDerivatives`,
recorded provenance + required citation, **every assertion sourced**, completeness ≥ 90/100, and the
Steward's acceptance-evidence artifact (`outcomes/<dataset-id>.json`) recorded. For patient-facing
content, **oncologist + advocate sign-off is additionally required**. Producing docs is *not* shipped;
recorded acceptance by the beneficiary is.

## Roadmap & milestones

**M0 — Foundation & cold-start (thin)**
- Goal: build the reusable toolkit + both gates + the licensing matrix; secure the blocking reviewer
  role; prove the end-to-end flow on **one clearly-open-access dataset**; begin steward outreach.
- **Cold-start de-risking (pilot selection).** To avoid producing docs nothing accepts, the pilot is
  chosen for (a) unambiguous open-access status and permissive license (e.g. a **DepMap CC-BY public
  release** file, or a **TCGA open-tier expression matrix via GDC**), and (b) a realistic acceptance path
  — an informal steward channel *or* a **self-serve Zenodo metadata DOI** we can publish ourselves — so
  M0 yields a real *accepted* outcome, not a "submitted, pending" one.
- Exit criteria: (1) datasheet template + canonical metadata model published; (2) **cancer-source
  licensing matrix** published with cited evidence; (3) **access-tier + identifiability gate** and
  **license gate** checklists exist and are applied to one dataset; (4) Croissant validator + germline/
  identifiability scanner working in CI with golden fixtures; (5) **License + Genomic-Privacy reviewer
  named** (blocking role filled before pilot review); (6) one open-access dataset documented end-to-end
  and **accepted** via informal channel or Zenodo DOI (acceptance artifact recorded) — or, if no channel
  materializes, **submitted** with the blocker surfaced; (7) ≥ 1 steward-outreach thread opened.

**M1 — Gates hardened + first acceptances**
- Goal: make both gates rigorous and get real deliveries accepted.
- Exit criteria: (1) both gates codified as reviewable artifacts and applied to ≥ 3 datasets;
  (2) ≥ 2 datasets **accepted** onto portal/repo/archive (acceptance artifacts recorded); (3) ≥ 1
  confirmed contribution partner; (4) license-snapshot capture automated where feasible; (5) **0**
  privacy/safety errors (any occurrence halts the milestone).

**M2 — Source coverage & scale (TCGA/GDC · GEO · cBioPortal · DepMap)**
- Goal: prove the template across all four target sources and reduce per-dataset effort.
- Exit criteria: (1) at least one accepted datasheet exists for **each** of TCGA/GDC, GEO, cBioPortal,
  and DepMap (cBioPortal study terms verified per study); (2) ≥ 5 datasets accepted cumulatively;
  (3) median per-dataset effort (AI-session minutes + human-review cycles, from the outcome ledger)
  measurably reduced vs. the recorded M0/M1 baseline; (4) germline/identifiability scanner integrated
  into the standard inspection flow.

**M3 — Reuse outcomes & sustainability**
- Goal: demonstrate real downstream reuse and a maintenance model.
- Exit criteria: (1) ≥ 2 verifiable external reuse events; (2) ≥ 6 datasets accepted cumulatively;
  (3) documented refresh/version-drift process (cancer datasets re-release regularly — e.g. GDC data
  releases, DepMap quarterly) and a steward identified for ongoing liaison.

**M4 — (Optional, gated) Patient/advocate plain-language explainers — `riskTier: high`**
- Goal: turn selected, already-shipped datasheets into education-only plain-language explainers.
- **Hard precondition:** oncologist + patient-advocate reviewers secured and the "not medical advice"
  framing ratified. Not started until M3 quality is proven.
- Exit criteria: (1) ≥ 1 explainer shipped with full oncologist + advocate sign-off and banner;
  (2) **0** explainers shipped without sign-off; (3) advocate-readability check passed.

Dependencies: M1 depends on M0 toolkit + gates; M2 coverage depends on M1's verified gates and matrix;
M3 depends on accepted deliveries from M1–M2; M4 depends on M3 quality + the high-risk reviewer panel.

## Work breakdown

The itemized, schema-mapped backlog lives in `TASKS.md`, organized by the milestones above. Each
milestone has a task table (`ID | Title | Type | Size | Risk | Deliverable | Depends on | Reviewer`),
acceptance criteria for the most important tasks, and a milestone Definition of Done. A backlog of
sized-but-unscheduled tasks and one complete, schema-valid example Task JSON are included. The
per-source candidate funnel is the **cancer-source licensing matrix** (`license-matrix-005`); no dataset
becomes a task until it passes both gates — listing a dataset does not pre-approve it.

## Governance, roles & stakeholders

- **Maintainer (Owner):** TBD — owns the toolkit, matrix, triage, and backlog.
- **License + Genomic-Privacy reviewer:** TBD (name TO BE SECURED) — **mandatory, non-skippable**
  gatekeeper for access-tier, identifiability, and license. Must be filled **before the M0 pilot is
  reviewed** (blocking prerequisite, not a parallel hire). Filled by a named reviewer who can read
  genomic data-sharing policies (NIH GDS, dbGaP/EGA access models) and open/NC licenses (CC/COSMIC/OncoKB
  /custom) and apply the identifiability methodology. May rotate among ≥ 2 qualified reviewers, but at
  least one named, qualified reviewer must exist at all times or triage/documentation halts. Until named,
  all tasks remain `verifiedNeed: false` and no dataset can pass the gate.
- **Technical reviewer(s):** rotation verifying dictionaries, access-tier assessments, Croissant metadata,
  validators, and provenance-citation completeness (CI green).
- **Oncologist reviewer & patient-advocate reviewer:** TO BE SECURED — **mandatory** for any patient-facing
  content (`riskTier: high`); credentialed sign-off required before merge per the good-deed definition.
- **Steward (last-mile owner):** TBD — owns relationships with GEO/cBioPortal/DepMap/archives and confirms
  acceptance (the "delivered" signal). Critical because Definition of Shipped is acceptance, not production.
- **Partner / requestor:** TO BE SECURED — named steward(s)/maintainer(s)/lab(s).

## Dependencies & integrations

- **External standards/specs (pinned):** Datasheets for Datasets (Gebru et al. 2021), Croissant ML v1.0,
  schema.org/Dataset, SPDX license identifiers; ontologies NCIt, OncoTree, Disease Ontology, EDAM.
  Versions recorded in `specVersions` and bumped only via a deliberate task.
- **External sources/portals:** NCI **GDC** (TCGA open tier), **GEO**/NCBI, **cBioPortal** (GitHub),
  **DepMap**/Broad; archives (Zenodo). Integration is *read-only inspection of open tiers* + *output-only
  metadata*; **no controlled-access authentication**, no automated upload.
- **Reference policies:** NIH Genomic Data Sharing (GDS) policy; dbGaP/EGA access models (to know what to
  exclude); source citation/attribution guidelines (TCGA, DepMap).
- **Datasets:** specific open-access cancer datasets — TO BE SELECTED via the matrix + gates; none assumed
  in scope yet.
- **Elyos pieces:** Task JSON schema (`packages/schema`), donated-lane CLI workspace/PR flow
  (`packages/cli`), good-deed definition + refusal guardrails. No funded-lane/runner dependency.

## Risks & mitigations

| Risk | Likelihood | Impact | Mitigation | Owner |
| --- | --- | --- | --- | --- |
| Documenting controlled-access data, or mistaking a controlled file for open | Medium | **Critical** | Access-tier gate runs first and blocks; scanner halts on controlled markers; reviewer confirms tier; full-stop on any occurrence | License+Privacy reviewer |
| Documenting individual-level / germline / re-identifiable data | Low | **Critical** | Germline/identifiability scanner + k≥5 check during inspection; halt+discard on any signal; never de-identify ourselves | License+Privacy reviewer |
| Treating COSMIC/OncoKB (non-commercial/custom) as open | Medium | High | Licensing matrix fixes disposition; gate requires cited `permitsDerivatives`; NC/custom → flag/escalate | License+Privacy reviewer |
| Misclassifying a license generally | Medium | High | Mandatory reviewer; record license URL + text snapshot + cited clause; exclude on doubt | License+Privacy reviewer |
| Patient-facing text read as medical advice | Medium | High | Patient-facing is a separate high-risk track; "not medical advice" banner; oncologist + advocate sign-off mandatory | Oncologist/advocate |
| Unsourced assertion slips into a datasheet | Medium | Medium | Provenance-on-every-assertion invariant; technical review checks `provenanceCitations[]` coverage | Technical reviewer |
| No partner secured → docs produced but never accepted (fails "delivered") | Medium | High | M0 steward outreach + Zenodo self-serve fallback; steward role; `verifiedNeed:false` until secured | Steward |
| Source data re-release makes docs stale (GDC/DepMap refresh often) | Medium | Medium | Record release/freeze + cadence; validation detects schema drift; refresh milestone (M3) | Maintainer |
| cBioPortal study terms vary per study | Medium | Medium | Per-study license verification required; no blanket acceptance | License+Privacy reviewer |
| Croissant/ontology spec drift | Low | Low | Canonical-model-first; pinned spec versions; isolated version-bump task | Maintainer |
| Scope creep into analysis/interpretation | Medium | Medium | Explicit non-goal; reviewers reject any interpretation/ranking | Maintainer |

## Security & privacy

- **Threat surface is small** (no runtime service, no data hosting) but the **data is sensitive** (cancer
  patients), so privacy controls are stricter than for generic open data.
- **Access-control discipline:** contributors must never hold or use dbGaP/EGA/DACO credentials for this
  work; the inspection protocol forbids authenticating to controlled-access endpoints. If a file demands
  authorization, that is itself an EXCLUDE signal.
- **Secrets handling:** validators/scanner/generator require no credentials by default. Any portal token
  needed for a contribution is supplied by the human submitting and never written into logs, receipts,
  or committed files (per Elyos rules).
- **PII / identifiability:** the dominant concern is *upstream* identifiability in candidate datasets,
  handled by the mandatory access-tier + identifiability gate and the germline scanner. We never download,
  store, or process individual-level or germline data; we inspect open-tier schema/aggregate only enough
  to document, and exclude on any signal.
- **Abuse/misuse prevention:** refuse and flag any task steering toward re-identification, linkage of
  de-identified records, laundering controlled-access data as open, surveillance, or producing clinical
  advice. Documentation must remain descriptive, sourced, and education-only.

## Sustainability & maintenance

- **Post-delivery ownership:** the steward maintains source/portal relationships; the maintainer keeps
  the toolkit (scanner, validators, generator, template) and the licensing matrix current with spec,
  policy, and source-release changes.
- **Refresh:** cancer datasets re-release on a cadence (e.g. GDC data releases, DepMap quarterly).
  Recorded release/freeze + cadence flags when a datasheet is due for refresh; validation scripts detect
  schema drift; stale docs become `maintenance` tasks.
- **Outcome tracking:** the steward records acceptance events and external reuse signals against the
  success metrics, reviewed each milestone. The licensing matrix is re-verified each milestone (licenses
  and access policies change).

## Open questions

- Which specific steward(s)/maintainer(s) (GEO submitter, cBioPortal community, DepMap, a curation group)
  will be the first confirmed contribution partner?
- For DepMap, which exact releases/files are CC-BY vs. otherwise-restricted (must be pinned per release)?
- For cBioPortal, what is the per-study license verification workflow, and which studies are unambiguously
  open (TCGA-derived) for the first deliveries?
- Where is the canonical license-text snapshot stored? **Proposed default (to ratify):** committed local
  copy of the license text/page + SHA-256 hash + Wayback save URL (linking the bare URL is insufficient).
- What counts as a sufficiently "verifiable external reuse event" for the outcome metric in a research
  context (citation, Bioconductor/`ExperimentHub` inclusion, a referencing PR)?
- Will the optional M4 patient-facing track be pursued, and can credentialed oncologist + advocate
  reviewers be secured before it starts? (If not, M4 stays unscheduled.)

## References

- Elyos work rules — `C:\code\elyos\CLAUDE.md`
- Good Deed Definition + risk tiers — `C:\code\elyos\docs\good-deed-definition.md`
- Task JSON schema — `C:\code\elyos\packages\schema\src\schemas.ts`
- Portfolio roadmap (Track 8 cancer guardrails) — `C:\code\elyos\planning\ROADMAP.md`
- Sibling project (house style) — `C:\code\elyos\planning\projects\open-data-datasheets\PLAN.md`
- Datasheets for Datasets — Gebru et al. (2018/2021)
- Croissant ML metadata format specification (MLCommons), v1.0
- NCI Genomic Data Commons (GDC) data access policies; TCGA publication/citation guidelines
- NIH Genomic Data Sharing (GDS) policy; dbGaP / EGA controlled-access models (for exclusion)
- DepMap data-use terms; cBioPortal data-usage and per-study terms
- COSMIC license (non-commercial); OncoKB terms of use (custom/non-commercial)
- Ontologies: NCI Thesaurus (NCIt), OncoTree, Disease Ontology, EDAM; SPDX license list

---

## Appendix A — Improvements applied

The following 25 specific improvements were identified during drafting and **have been applied** to this
PLAN (and the companion TASKS.md). Each lists what changed and where.

1. **Access-tier gate added and sequenced first.** A dedicated access-tier check runs *before* the license
   gate (privacy/safety dominates legality) — Solution approach step 1; Data section.
2. **Germline/identifiability scanner as a build artifact**, not a manual note — Scope, Solution approach,
   tooling task `scanner-016`, with golden CI fixtures.
3. **Cancer-source licensing matrix table** with explicit per-source disposition (TCGA/GDC, GEO,
   cBioPortal, DepMap, COSMIC, OncoKB, ICGC, CPTAC, SEER) — Data section + task `license-matrix-005`.
4. **COSMIC and OncoKB explicitly flagged non-commercial/custom** and excluded from the do-first pool in
   three places (guardrails, matrix, Out-of-scope) so they cannot be silently treated as open.
5. **"Provenance on every assertion" made a verifiable invariant** via `provenanceCitations[]` in the
   canonical model and a technical-review acceptance check — not just a slogan.
6. **k ≥ 5 quasi-identifier threshold** specified for re-identification risk (age+sex+rare-diagnosis+geo)
   — Data/identifiability stance.
7. **dbGaP/EGA/DACO named explicitly** as out-of-scope controlled tiers throughout (not just "controlled
   access"), matching the task's binding list.
8. **NIH GDS policy referenced** as the basis for TCGA open-tier reuse and for what defines the controlled
   boundary — Dependencies + References.
9. **Required dataset citation** (e.g. TCGA/DepMap) added to the provenance model and reproduced verbatim
   — Data section, canonical model.
10. **Patient-facing content isolated into a separate, optional, gated milestone (M4) at `riskTier: high`**
    rather than bundled into core datasheets.
11. **Oncologist + patient-advocate reviewer roles** added to Governance and made a hard merge gate for any
    patient-facing surface, per the good-deed `high` tier.
12. **"Not medical advice" banner** mandated for all patient-facing output — guardrails, M4, metrics
    (count of explainers shipped without sign-off = 0).
13. **Zero-tolerance safety metric**: privacy/safety errors target is **0**, and a single occurrence
    triggers a full stop — Success metrics + M1 exit criteria.
14. **Self-serve Zenodo DOI fallback** for cold-start acceptance so M0 can yield a real *accepted* outcome
    without a third party — M0, per-channel acceptance.
15. **Per-channel acceptance definitions** specialized for GEO/cBioPortal/DepMap/Zenodo/lab channels (not
    the generic portal list) — Success metrics.
16. **`verifiedNeed: false` everywhere until a named steward confirms acceptance**, with rationale, in both
    files.
17. **Bounded inspection protocol hardened for genomics**: open-tier-only, germline/individual-level halt,
    no controlled authentication, row cap, ephemeral, no committed samples — Solution approach.
18. **Hard canonical-model invariant**: any record with `accessTier != open` or individual-level/germline
    flags is rejected by construction — Solution approach.
19. **Ontology fields** (NCIt/OncoTree/Disease Ontology/EDAM) added to the canonical model and data
    dictionary so cancer datasheets are semantically richer and machine-mergeable.
20. **cBioPortal per-study license verification** called out as a distinct risk + acceptance requirement
    (terms vary per study) rather than assuming a blanket license.
21. **Source-refresh reality** (GDC releases, DepMap quarterly) baked into Sustainability + M3 + a
    `refresh` maintenance task, so staleness is managed, not discovered.
22. **License + Genomic-Privacy reviewer is a blocking prerequisite** filled *before* the M0 pilot review,
    with rotation rules — Governance + task `reviewer-001`.
23. **Worked EXCLUDE artifact (a COSMIC entry)** required as a committed CI reference so the gate's
    rejection path is tested, not just the happy path — Quality gates + `gate-004`.
24. **Completeness score requires provenance-citation coverage**, tying the 0–100 score to the
    provenance-on-every-assertion rule — Success metrics.
25. **All four target sources must each have ≥ 1 accepted datasheet** as an explicit M2 exit criterion, so
    coverage is proven across TCGA/GEO/cBioPortal/DepMap rather than concentrated in the easiest source.

## Review sign-off

**Reviewed for completeness and correctness on 2026-06-28 (drafting reviewer pass).**

- **Spec compliance:** all 17 required H2 sections from `PLAN_SPEC.md` are present and in order; the
  metadata header matches the global convention (Status/Version/Last updated/Owner/Lane). ✔
- **Cancer guardrails:** open-access-only, controlled-access (dbGaP/EGA/DACO/biobanks) out of scope,
  COSMIC/OncoKB flagged non-commercial/custom, no-medical-advice + oncologist/advocate review at
  `riskTier: high`, and provenance-on-every-assertion all appear in the leading guardrail block **and**
  are operationalized in the gates, metrics, roles, and tasks (not just stated). ✔
- **Correctness checks made during review and fixed:**
  - Confirmed the access-tier gate is sequenced *before* the license gate everywhere it appears (fixed one
    ordering reference in Solution approach).
  - Verified COSMIC/OncoKB never appear in any "accept" path; they appear only as FLAG/EXCLUDE.
  - Verified `verifiedNeed: false` rationale is consistent between PLAN and TASKS.
  - Verified every milestone has measurable exit criteria and that M2 forces coverage across all four
    sources.
  - Verified the example Task JSON in TASKS.md validates against `schemas.ts` (required fields present;
    enums valid; `verifiedNeed: false`; `lane: donated` so no `fundedBudgetUsd` required).
- **Residual risk acknowledged:** the project's correctness depends on securing a qualified
  License + Genomic-Privacy reviewer; until then no dataset passes the gate and all tasks stay
  `verifiedNeed: false`. This is surfaced, not hidden.
- **Outstanding human decisions:** named first partner; DepMap per-release license pinning; whether to
  pursue the optional high-risk M4 patient-facing track. Listed in Open questions.

Sign-off: **Draft approved for circulation** (senior-staff-engineer + TPM drafting review). Not yet
ratified by the Elyos board/community or by the (still-to-be-named) credentialed reviewers.
