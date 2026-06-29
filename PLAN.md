# PLAN — cancer-dataset-datasheets

> Status: Draft · Version: 0.2.0 · Last updated: 2026-06-29 · Owner: TBD (maintainer) · Lane: donated · Risk tier: medium (patient-facing education, if attempted, is high)

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

**Targets are explicitly conditioned on the blocking reviewer being seated.** The "6 accepted in 6 months"
target assumes the **License + Genomic-Privacy reviewer is named** (the hard blocker — until then no dataset
passes the gate and all tasks stay `verifiedNeed: false`) and at least one acceptance channel exists. With
no confirmed steward, the **self-serve Zenodo metadata DOI** is the only channel that makes any early
acceptance achievable; the cumulative targets are aspirational until both the reviewer and a steward are
secured, and are read as conditional rather than committed.

**Quantifying "improves reuse" (so DoDs are verifiable).** Each datasheet gets a
**documentation-completeness score (0–100)**: fraction of canonical-metadata fields populated *and
source-verified* — data-dictionary coverage of all documented fields, provenance complete (incl.
`upstreamVersionDoi`), license recorded with `permitsDerivatives` + `shareAlike` + cited clause, access-tier
+ identifiability assessment complete, Datasheet sections answered, valid Croissant emitted, **every
assertion carrying a provenance citation**. The "every assertion has a citation" rule is **machine-enforced
by a citation-coverage lint** (flags any datasheet sentence lacking a citation anchor; the 90/100 bar is not
reviewer-judged on this dimension), feeding the score. Target: every delivered datasheet reaches **≥ 90/100**
vs. a recorded **before-score**. The before-score is defined **against the source portal's *existing*
metadata** (e.g. GDC structured fields, the GEO MIAME record), **not against nothing** — a dataset with no
prior datasheet does not get a trivially-near-0 before-score that inflates apparent improvement. The
before/after pair is stored in the dataset's gate/provenance artifact.

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

## Competitive landscape & differentiation

No incumbent delivers *verified, license-gated, access-tier-aware, provenance-complete, machine-readable*
datasheets across cancer portals. **Portals own the data and the tier-of-record; standards own the format;
nobody owns the curated, verified bridge.** That bridge is this project.

**Documentation standards / frameworks (templates, not rivals).**
- **Datasheets for Datasets** (Gebru et al., CACM 2021) — the 57-question / 7-section questionnaire we adopt.
  Strength: peer-reviewed, widely accepted vocabulary. Gap: prose-only, no machine-readable output, no
  license/access-tier verification, no genomics/identifiability notion. We use it as the *template*.
- **MLCommons Croissant (1.0 / now 1.1)** — machine-readable JSON-LD over schema.org; **1.1 adds DUO/PROV-O
  permission+provenance layers and MCP**. Strength: the interoperability substrate, indexable by Google
  Dataset Search. Gap: a *format*, not curated/verified content — says nothing about whether a cancer file is
  open vs. dbGaP-controlled. We **emit** Croissant (target 1.1 DUO/PROV-O); it is our output format, not a
  competitor.
- **Data Nutrition Project (Dataset Nutrition Label, 2nd-gen)** — FDA-style at-a-glance label; a model for the
  optional **M4** patient explainers. Gap: generic, no license-gate, limited live coverage.
- **Hugging Face dataset cards** — README + YAML frontmatter, `mlcroissant` export. Strength: huge reach.
  Gap: author-self-reported and frequently empty/unverified, license fields routinely wrong, no
  genomic-privacy gating, and not where cancer-portal data lives.

**Discovery / search (complements, not rivals).**
- **Google Dataset Search** — indexes schema.org/`Dataset` JSON-LD; emitting Croissant makes our datasheets
  findable. Gap: indexes whatever publishers assert, with no verification — a complement.
- **OmicsDI (EMBL-EBI)** — cross-omics metadata harmonization + discovery via a unified REST API. Gap:
  discovery-grade (thin) per-dataset metadata, no license-verification gate, no access-tier/identifiability
  verdict, no Datasheets narrative.

**The cancer data portals themselves (the real incumbents).**
- **NCI GDC / CRDC** — authoritative TCGA host; canonical, harmonized, programmatic API; the **source of truth
  on tier**. Gap: portal-/schema-centric, technical docs; no Datasheets narrative, no Croissant, no
  plain-language layer; tier facts live in policy pages, not per-file machine metadata.
- **cBioPortal** — rich genomic browsing over TCGA/TARGET + many studies; **ODbL default with per-study
  notes**. Weakness: **clinical metadata is documented as "extremely heterogeneous and lacks standardization"
  — different studies use arbitrary terms for identical entities**, and the license is per-study and easy to
  get wrong. This heterogeneity is precisely the gap we fill (harmonize to OncoTree/NCIt).
- **DepMap (Broad)** — CC BY 4.0 on Figshare, **DOI-per-quarterly-release**. Gap: docs are release-notes +
  readmes; no standardized datasheet/Croissant; cross-quarter version drift is a documentation burden nobody
  centrally solves.
- **GEO (NCBI)** — MIAME/MINSEQE minimum metadata, stable accessions. Gap: MIAME is *minimum*; reuse-fitness
  and license are thin/free-text; some series link controlled SRA/dbGaP components — the open-vs-controlled
  boundary risk we target.
- **Kaggle cancer datasets** — accessible but provenance/license frequently broken or mis-stated, re-hosted
  derivatives with lost lineage — an anti-pattern that motivates this project.

**Our differentiators (the moat).**
- **The two-part VERIFIED gate is the moat.** Access-tier + identifiability verified **before** license, both
  as committed, auditable artifacts — a "**this is safely open, and here is the cited proof**" verdict
  (access-tier + identifiability + cited license clause). **No portal, HF, OmicsDI, or Croissant ships this.**
- **Verification, not assertion.** HF cards and Google Dataset Search index self-reported metadata; we deliver
  *source-verified, cited, snapshotted* records with a **zero-tolerance** privacy/safety metric.
- **Cancer-license-matrix-of-record** — a correctly-tiered, clause-cited matrix spanning
  GDC/GEO/cBioPortal(**ODbL**)/DepMap/COSMIC/OncoKB/ICGC/CPTAC/SEER — the artifact everyone needs and nobody
  maintains; it correctly distinguishes CC-BY (DepMap) vs. ODbL share-alike (cBioPortal) vs. GDS-open (GDC)
  vs. NC/custom (COSMIC/OncoKB) vs. per-series GEO checks.
- **Machine-readable permissions via Croissant (push to 1.1 DUO/PROV-O)** — consent/access constraints as
  ontology terms, not prose; uniquely on-mission and future-proofed for agent (MCP) consumption.

**Division of labor with the Elyos siblings** (see the reuse contract under Solution approach): three layers
— **general toolkit** (`open-data-datasheets`, which owns the shared canonical model/Croissant
validator/inspection protocol) → **cancer-wide verified datasheets + gate** (this project, the genomics
*superset*) → **disease-specific discovery** (`ewing-open-data-catalog`, which *consumes/links* these
datasheets). Win condition: one shared codebase, three scopes, **zero validator triplication**.

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
nonCommercial:boolean, shareAlike:boolean, snapshotRef, citedClause}`, `provenance {repository, accession,
retrievedAt, release, upstreamVersionDoi, dataFreeze, updateCadence, attribution, requiredCitation,
gdsPolicyNote}`, `identifiability {individualLevel:boolean, germlinePresent:boolean, reIdentificationRisk:string,
publisherDeidentification:string, attestation:string}`, `ontology {disease[], assay[]}`,
`fields[] {name, type, units, allowedValues, nullable, description, caveats, ontologyRef}`,
`knownIssues[]`, `examples[]` (synthetic/illustrative only), `provenanceCitations[]` (one per assertion),
`specVersions {croissant}`, `patientFacing:boolean`, `completenessScore {before, after}`.

The `license.shareAlike` flag (added in v0.2) captures **ODC-ODbL and other copyleft/share-alike terms** —
this is **cBioPortal's stated default** (unless otherwise noted, cBioPortal data are available under the
**ODC Open Database License (ODbL)** with attribution, and some studies further restrict commercial use).
ODbL is **materially different from CC-BY**: whether share-alike terms permit our **CC-BY-licensed derivative
documentation** is a **License + Genomic-Privacy reviewer ruling, not a guess** (metadata-about-data likely
escapes the ODbL database right, but this must be decided, cited, and recorded before any cBioPortal source
is documented). `provenance.upstreamVersionDoi` (added in v0.2) requires pinning the **exact upstream dataset
DOI / release identifier** (e.g. a DepMap Figshare per-quarter DOI, a GDC data-release number) — not only a
free-text `release` string — so a datasheet names "DepMap 24Q4 (DOI …)", never an ambiguous "DepMap public".
**Hard invariant:** any record with `accessTier != open`, `identifiability.individualLevel == true`, or
`identifiability.germlinePresent == true` is rejected by the gate and never produced.

**Tech stack.** TypeScript, ESM, pnpm workspaces (Elyos conventions). Validators, scanner, and Croissant
generator are small Node packages with minimal dependencies. Documentation authored in Markdown +
JSON/JSON-LD. No runtime services; everything runs locally or in CI.

**Reuse contract with `open-data-datasheets` (this project is the genomics SUPERSET, not a fork).** The
sibling `open-data-datasheets` (general/civic open data) **owns the shared toolkit** — the canonical
metadata model, the Croissant validator (+ golden fixtures), the bounded 1,000-row/5 MB inspection
protocol, and the CC-BY-output/MIT-code split. cancer-dataset-datasheets **reuses those as a dependency and
extends them**; it does **not** re-implement a parallel canonical model, Croissant validator, or inspection
protocol (doing so would leave Elyos maintaining three drifting validators). What this project genuinely
**adds on top** — its reason to be a superset — is the **access-tier + identifiability gate, the germline/
identifiability scanner, the k≥5 check, the cancer-source license matrix (incl. ODbL/share-alike), the
OncoTree/NCIt ontology layer, the upstream-version-DOI pinning, and the oncologist/advocate review track.**
The written reuse manifest (which components are shared dependencies vs. cancer-specific extensions, and who
owns the shared core) is a deliverable of `template-003`/`croissant-007`. Boundary with
`ewing-open-data-catalog`: that is a single-disease **discovery/catalog** layer that **consumes and links**
these datasheets for its vertical; this project **produces the deep per-dataset datasheet+gate+Croissant
artifact** and does **not** re-build a disease catalog. Net: one shared codebase, three scopes
(general toolkit → cancer-wide verified datasheets → disease-specific discovery), **zero validator
triplication**. Any Ewing-sarcoma open dataset is owned here for the datasheet; ewing points at it.

**Pinned spec versions** (recorded in `specVersions`, bumped only via a deliberate task):
- **Datasheets for Datasets** — Gebru et al. (2021) questionnaire.
- **Croissant ML** — **target v1.1 (MLCommons)** for its first-class **DUO (Data Use Ontology) + PROV-O
  permission/provenance layers**, which natively encode the access-tier/consent constraints this project
  exists to assert (e.g. `DUO:0000004` no-restriction, disease-specific, non-commercial) — emitting these as
  ontology terms rather than free-text prose is squarely on-mission and future-proofs for agent (MCP)
  consumption + Google Dataset Search indexing. The generator may pin **v1.0 transitionally** for stability,
  but if so it must **record why** and **schedule the 1.1 bump** (the DUO/PROV-O layer is not optional polish —
  it is the machine-readable form of the gate verdict). Validate against the pinned context/SHACL either way.
- **Ontologies** — **OncoTree is the primary disease vocabulary** (the de-facto cancer standard cBioPortal
  itself uses), with **NCI Thesaurus (NCIt) and Disease Ontology as secondary crosswalks** to remove
  three-vocabulary mapping ambiguity; EDAM for assay/format — versions recorded per datasheet.

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

**Claude API leverage (always human-verified; the gate verdict is never an LLM's to make).** This is a
donated-lane project — a human runs their agent; Claude is an *acceleration and drafting* layer feeding the
mandatory human reviews, never an autonomous decider. Where it adds clear value:
- **Draft the Datasheets-for-Datasets narrative** from inspected schema + portal metadata — the 7-section
  questionnaire answers as a *reviewer-ready draft*, with **each sentence tagged with a provenance anchor**
  so the citation-coverage lint can verify it (see provenance lint below).
- **License-clause extraction → structured verdict PROPOSAL.** Claude reads the license/terms text and
  *proposes* `permitsDerivatives`, `nonCommercial`, `shareAlike`, and the **exact cited clause** as a
  structured candidate the License + Genomic-Privacy reviewer **confirms or overturns** — high leverage on
  cBioPortal's per-study notes and the ODbL-vs-CC-BY distinction.
- **Schema → Croissant JSON-LD mapping + clinical-field harmonization** — emit valid Croissant (target 1.1
  DUO/PROV-O) and **normalize cBioPortal's heterogeneous clinical field names to OncoTree/NCIt codes** (the
  documented "arbitrary terms for identical entities" problem the portal does not enforce).
- **Identifiability/quasi-identifier triage assist** — flag candidate quasi-identifier combinations and
  small-cell-count risks for the scanner/reviewer (assist input, **never** the gate decision).
- **Plain-language M4 explainer drafts** — education-only first drafts for oncologist + advocate review.

**Hard guardrails on Claude's role (non-negotiable):** access-tier and license/`permitsDerivatives`
determinations are **human-verified** — Claude proposes with citations, the named reviewer decides (an LLM
mislabeling a controlled file as open is the project's critical failure mode). **No fabricated provenance** —
every citation must resolve to a real source; Claude must never invent a DOI, license URL, accession, or
clause, and citation anchors are verified, not trusted. **Never ingest controlled/identifiable data** —
Claude operates only on confirmed open-tier, bounded (≤1,000 rows/5 MB), aggregate/de-identified samples and
must **never** be pointed at dbGaP/EGA/DACO endpoints; on any germline/individual-level/controlled signal,
halt. **No clinical interpretation** and **no de-identification by us** (Claude documents the *publisher's*
de-identification; it never anonymizes/aggregates data itself). Confirm current model IDs via the
`claude-api` skill before any build; do not quote model/pricing from memory.

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
| **TCGA via GDC** | GDC **open-access** data (e.g. gene expression, copy-number, de-identified clinical/biospecimen) | Open data: no use restrictions under the **NIH Genomic Data Sharing (GDS)** policy (bound by the GDS non-re-identification clause); TCGA publication/citation guidelines apply; **controlled-access (raw sequence, germline) via dbGaP is OUT OF SCOPE**. **Tier is per-file per-GDC-release, not per-data-type** — e.g. masked somatic-mutation MAFs have moved between open and controlled in past GDC policy iterations, so "masked somatic mutations = open" is **not durably true** and must be re-verified each release | **ACCEPT** open-tier **per file per release**; document required TCGA citation; never touch controlled tier |
| **GEO (NCBI)** | Public/open series (processed/aggregate; MIAME/MINSEQE minimum metadata) | NCBI public data, generally unrestricted reuse; **per-series check** — some series link raw/individual-level data in SRA/dbGaP (controlled) → exclude those linked files | **ACCEPT** open series; verify each series; exclude any controlled-linked components |
| **cBioPortal** | Studies whose underlying data are open; portal software is open-source | **Stated default: unless otherwise noted, data are under the ODC Open Database License (ODbL) with attribution**, and some studies additionally restrict commercial use; individual studies may carry their own terms (some derive from TCGA = GDS-open). **ODbL is share-alike** — record `shareAlike: true` and obtain a reviewer ruling on whether our CC-BY documentation is compatible **before** documenting. Clinical metadata is documented as **"extremely heterogeneous / non-standardized"** (arbitrary per-study terms for identical entities) | **ACCEPT per-study only after terms (ODbL-default vs. study-specific) AND the share-alike/CC-BY compatibility ruling are verified**; otherwise FLAG |
| **DepMap (Broad)** | Public DepMap/CCLE releases (quarterly, DOI-per-release on Figshare) | Recent public releases under **CC BY 4.0** (verify the specific release/file; some files carry distinct terms). **Pin the per-quarter release DOI** (e.g. DepMap 24Q4) in `upstreamVersionDoi`, never just "DepMap public" | **ACCEPT** with release-specific license + version DOI recorded; verify each release |
| **COSMIC** | — | **Non-commercial license**; commercial reuse requires a paid license; redistribution restricted | **FLAG / EXCLUDE from do-first**; escalate to license policy; never treat as open |
| **OncoKB** | — | **Custom license**, free for research only, redistribution/commercial restricted | **FLAG / EXCLUDE from do-first**; escalate; never treat as open |
| **ICGC / PCAWG** | Open-tier summaries only | Mixed: open + **DACO-controlled** individual-level data | **ACCEPT open-tier only**; controlled tier OUT OF SCOPE |
| **CPTAC (PDC)** | Open proteomics/aggregate | Often open, **but CPTAC genomic / germline-adjacent components also have controlled portions in the GDC/PDC split** — default to a **cautious lean**, verify per dataset, and never assume the whole study is open | **ACCEPT** open-tier **only after per-dataset verification**; controlled components OUT OF SCOPE |
| **SEER** | Public **aggregate** incidence/mortality statistics | Aggregate stats public; **SEER research (individual-level) data requires a signed agreement** → out of scope | **ACCEPT aggregate stats only**; individual-level OUT OF SCOPE |

*Versions/dates of each license are captured per dataset; the matrix is re-verified each milestone.*

**Objective "permits derivatives" criterion.** A dataset PASSes the license check only if its license is
accepted by the matrix/policy **and** `license.permitsDerivatives: true` is recorded with a cited
clause/URL evidencing derivative documentation/metadata is allowed. Missing evidence, an unparseable
license, non-commercial terms without a decided policy, or `permitsDerivatives` that cannot be set `true`
from the source text = **FLAG/EXCLUDE**, never default-allow. **Share-alike (ODbL/copyleft) sources** (cBioPortal
default) additionally require `license.shareAlike: true` **and** a recorded reviewer ruling on whether
CC-BY documentation derived from a share-alike source is compatible — until that ruling exists, the source is
**FLAG**, not PASS.

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
  equivalence class. Any hit on **individual-level** content → EXCLUDE/FLAG and halt inspection.
- **Suppression-aware small-cell exception (aggregate tables).** Rare-cancer cohorts routinely show
  small cell counts (k < 5) **even in legitimately-open aggregate summary tables** (e.g. SEER-style
  incidence counts that the publisher has already cell-suppressed/rounded). To avoid **over-EXCLUDING valid
  open summaries**, a small-cell finding in an *aggregate count table that is not individual-level* routes to
  a **documented suppression-aware review exception**, not an automatic halt: the reviewer records that the
  table is aggregate, confirms the publisher's suppression/rounding, and notes residual risk. This exception
  **never** applies to individual-level or re-identifiable records — those always EXCLUDE.
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
**Share-alike caveat:** for **ODbL/share-alike sources (cBioPortal default)**, the CC-BY output licensing is
contingent on the reviewer's compatibility ruling — metadata-about-data likely escapes the ODbL database
right, but where a share-alike obligation could attach to our derived output we follow the reviewer's
recorded determination (which may require a compatible license or a different attribution stance) rather than
defaulting to CC-BY.

## Quality, review & risk gates

**Risk tier: medium** for core documentation (factual accuracy about sensitive cancer data, license
interpretation, and identifiability judgement). **`high`** for any **patient-facing** content.

**Required review before a deed is "done":**
- **License + Genomic-Privacy reviewer** (mandatory, every dataset; non-skippable hard gate): confirms
  open-access tier, no individual-level/germline/re-identifiable content, license permits
  reuse+derivatives, COSMIC/OncoKB-style terms correctly flagged. **No deed ships without this sign-off.**
  This role must be filled **before the M0 pilot is reviewed**.
- **Technical reviewer:** confirms the data dictionary, access-tier assessment, Datasheet, Croissant
  metadata, and validation script are accurate, that **every assertion has a provenance citation** (verified
  by the citation-coverage lint, not eyeballed), that the **upstream version DOI** is pinned, and that CI is
  green.
- **Oncologist + patient-advocate reviewers** (mandatory for *any* patient-facing surface; escalates the
  task to `riskTier: high`): confirm the content is education only, accurate, non-stigmatizing, carries
  the "not medical advice" banner, and contains no clinical/treatment/prognostic guidance.

**Test fixtures & golden files (so "CI green" means something).** Each tool ships with committed test
assets exercised in CI, using only **synthetic/public** fixtures (never real inspected data):
- **Croissant validator** — golden JSON-LD fixtures (known-valid must pass; malformed must fail) against
  the pinned Croissant version (**target v1.1 incl. DUO/PROV-O permission fields**; v1.0 transitionally).
- **Germline/identifiability scanner** — synthetic fixtures that must trip each rule (individual-level,
  germline, low-k quasi-identifiers, controlled-access markers) and clean fixtures that must pass, **plus a
  fixture for the suppression-aware small-cell aggregate exception** (open aggregate count table that routes
  to review, not auto-halt).
- **Citation-coverage lint** — fixtures where every assertion is anchored (must pass) and where an assertion
  lacks a citation anchor (must fail), so provenance-on-every-assertion is machine-enforced.
- **Gate checklist** — a worked example PASS artifact and a worked EXCLUDE artifact (e.g. a COSMIC entry)
  committed as references, **plus a cBioPortal/ODbL FLAG-until-ruled artifact**.

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
- Exit criteria: (1) datasheet template + canonical metadata model published, **with the written reuse
  contract** declaring which components are reused from `open-data-datasheets` vs. cancer-specific
  extensions (no parallel canonical-model/validator/inspection reimplementation); (2) **cancer-source
  licensing matrix** published with cited evidence, **including ODbL/share-alike for cBioPortal** and the
  recorded **CC-BY-vs-share-alike reviewer ruling**; (3) **access-tier + identifiability gate** and
  **license gate** checklists exist (with the **suppression-aware small-cell exception** for open aggregate
  tables) and are applied to one dataset; (4) Croissant validator + germline/identifiability scanner working
  in CI with golden fixtures, **plus a Croissant-version decision recorded (target 1.1 DUO/PROV-O, or pinned
  1.0 with a scheduled bump)** and a **citation-coverage lint** enforcing provenance-on-every-assertion;
  (5) **License + Genomic-Privacy reviewer named** (blocking role filled before pilot review);
  (6) one open-access dataset documented end-to-end (with **upstream version DOI pinned** and the before-score
  taken against existing portal metadata) and **accepted** via informal channel or Zenodo DOI (acceptance
  artifact recorded) — or, if no channel materializes, **submitted** with the blocker surfaced; (7) ≥ 1
  steward-outreach thread opened; **OncoTree fixed as the primary disease vocabulary** (NCIt/DO secondary).

**M1 — Gates hardened + first acceptances**
- Goal: make both gates rigorous and get real deliveries accepted.
- Exit criteria: (1) both gates codified as reviewable artifacts and applied to ≥ 3 datasets;
  (2) ≥ 2 datasets **accepted** onto portal/repo/archive (acceptance artifacts recorded); (3) ≥ 1
  confirmed contribution partner; (4) license-snapshot capture automated where feasible; (5) **0**
  privacy/safety errors (any occurrence halts the milestone).

**M2 — Source coverage & scale (TCGA/GDC · GEO · cBioPortal · DepMap)**
- Goal: prove the template across all four target sources and reduce per-dataset effort.
- Exit criteria: (1) at least one accepted datasheet exists for **each** of TCGA/GDC, GEO, cBioPortal,
  and DepMap (cBioPortal study terms — ODbL-default vs. study-specific — and the share-alike ruling verified
  per study); (2) ≥ 5 datasets accepted cumulatively; (3) median per-dataset effort (AI-session minutes +
  human-review cycles, from the outcome ledger) measurably reduced vs. the recorded M0/M1 baseline;
  (4) germline/identifiability scanner integrated into the standard inspection flow; (5) **cBioPortal's
  heterogeneous clinical fields harmonized to OncoTree/NCIt** in at least the cBioPortal datasheet, and
  **Croissant 1.1 DUO/PROV-O emission landed** (or the scheduled bump executed) so access-tier/consent ship
  as ontology terms.

**M3 — Reuse outcomes & sustainability**
- Goal: demonstrate real downstream reuse and a maintenance model.
- Exit criteria: (1) ≥ 2 verifiable external reuse events; (2) ≥ 6 datasets accepted cumulatively;
  (3) documented refresh/version-drift process keyed on the **pinned upstream version DOI/release-id**
  (cancer datasets re-release regularly — e.g. GDC data releases, DepMap quarterly 23Q4/24Q2/24Q4) and a
  steward identified for ongoing liaison; (4) accepted datasheets **emit schema.org/`Dataset` + Croissant
  JSON-LD and are hosted where Google Dataset Search can crawl them** (e.g. Zenodo/GitHub Pages), converting
  the Zenodo fallback into a real, discoverable distribution channel.

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

- **External standards/specs (pinned):** Datasheets for Datasets (Gebru et al. 2021), **Croissant ML
  (target v1.1 for DUO/PROV-O; v1.0 only transitionally with a scheduled bump)**, schema.org/Dataset, SPDX
  license identifiers (incl. **ODC-ODbL** for cBioPortal); ontologies **OncoTree (primary)**, NCIt + Disease
  Ontology (secondary crosswalks), EDAM. Versions recorded in `specVersions` and bumped only via a
  deliberate task.
- **Sibling Elyos projects (reuse, not fork):** `open-data-datasheets` owns the shared canonical model /
  Croissant validator / inspection protocol (consumed as a dependency); `ewing-open-data-catalog` consumes
  and links the datasheets produced here. See the reuse contract under Solution approach.
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
| cBioPortal **ODbL share-alike** mis-handled / treated as CC-BY-compatible without a ruling | Medium | High | ODbL added to matrix + `shareAlike` flag; reviewer ruling on CC-BY-vs-share-alike required before any cBioPortal source PASSes; FLAG until ruled | License+Privacy reviewer |
| cBioPortal study terms vary per study + heterogeneous clinical fields | Medium | Medium | Per-study license verification; OncoTree/NCIt harmonization of clinical fields; no blanket acceptance | License+Privacy reviewer |
| **Validator/canonical-model triplication & drift** across the three sibling projects | Medium | Medium | Reuse contract: reuse `open-data-datasheets`'s shared toolkit; extract a shared verified-datasheet core; no parallel reimplementation | Maintainer |
| Croissant/ontology spec drift (e.g. 1.0→1.1) | Low | Low | Canonical-model-first; pinned spec versions; recorded version decision + isolated version-bump task | Maintainer |
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

## Adjacent opportunities

Parallel and perpendicular spin-offs surfaced by the competitive analysis — **not committed scope**, recorded
so the strategy is captured and the core stays focused. Each reuses (never re-implements) the verified core.
- **Shared Elyos "verified-datasheet core"** — extract the canonical model + Croissant validator + bounded
  inspection protocol into one package consumed by `open-data-datasheets`, `cancer-dataset-datasheets`, and
  `ewing-open-data-catalog` (kills the triple-validator drift; the structural realization of the reuse
  contract).
- **`open-cohort-catalog`** — a cancer-wide cohort-finder discovery index layered *on* these datasheets (the
  cancer analogue of what ewing does for one disease); consumes datasheets, does not re-document.
- **`cancer-data-dictionaries`** — spin the per-field dictionary + OncoTree/NCIt crosswalk into a standalone
  harmonization asset that directly attacks cBioPortal's documented metadata heterogeneity (potentially
  contributable upstream).
- **Verified-datasheet MCP server** — expose accepted datasheets/Croissant via MCP so agents can query "is
  dataset X open, what's its license, what fields" with cited provenance (aligns with Croissant's own MCP
  direction). **Read-only, open-tier metadata only** — never controlled/identifiable data.
- **"Gate-as-a-service"** — package the access-tier + identifiability + license gate as a reusable check
  other Elyos health/genomics projects (and external curators) invoke before touching a dataset.
- **Datasheet ↔ Dataset-Nutrition-Label adapter** — auto-render the M4 plain-language label from the
  canonical model, reusing the Data Nutrition Project's accessible framing for patients/advocates.

## Open questions

- Which specific steward(s)/maintainer(s) (GEO submitter, cBioPortal community, DepMap, a curation group)
  will be the first confirmed contribution partner?
- **cBioPortal / ODbL share-alike (blocks the entire cBioPortal source):** does CC-BY-licensed *documentation*
  derived from an ODbL share-alike source create a license conflict, and what is the License + Genomic-Privacy
  reviewer's standing rule?
- **Croissant 1.0 vs 1.1:** pin 1.0 for stability, or adopt 1.1's DUO/PROV-O permission layer that directly
  encodes access-tier/consent — and when is the bump scheduled?
- **Reuse contract with `open-data-datasheets`:** which components are shared dependencies vs. cancer-specific
  extensions, and who owns the shared core?
- **Boundary with `ewing-open-data-catalog`:** confirm catalog-consumes-datasheet (not parallel
  documentation) for any Ewing-sarcoma open dataset.
- **Small-cell / k≥5 over-exclusion:** what is the documented suppression-aware exception for legitimately-open
  aggregate cancer count tables that trip the identifiability scanner?
- **Reviewer-gated targets:** is the "6 accepted in 6 months" target explicitly conditioned on the
  License + Genomic-Privacy reviewer being seated (the named hard blocker)?
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
- Datasheets for Datasets — Gebru et al. (2018/2021) — https://arxiv.org/abs/1803.09010
- Croissant ML metadata format specification (MLCommons) — **v1.1 (DUO/PROV-O + MCP)**, v1.0 prior —
  https://mlcommons.org/2026/02/croissant-1-1-standard/ · https://mlcommons.org/2025/10/croissant-mcp/
- NCI Genomic Data Commons (GDC) / CRDC data access policies; TCGA publication/citation guidelines —
  https://gdc.cancer.gov/access-data/data-access-policies · https://datacommons.cancer.gov/cancer-research-data-commons
- NIH Genomic Data Sharing (GDS) policy; dbGaP / EGA controlled-access models (for exclusion)
- DepMap data-use terms (CC BY 4.0, quarterly Figshare DOIs); cBioPortal data-usage and **ODbL-default /
  per-study terms** — https://docs.cbioportal.org/user-guide/faq/ · https://plus.figshare.com/articles/dataset/DepMap_24Q4_Public/27993248
- cBioPortal clinical-metadata heterogeneity — https://www.biorxiv.org/content/10.1101/2025.11.26.689816v1
- COSMIC license (non-commercial); OncoKB terms of use (custom/non-commercial)
- Ontologies: **OncoTree (primary)**, NCI Thesaurus (NCIt), Disease Ontology, EDAM; SPDX license list
- Sibling projects — `open-data-datasheets` (shared toolkit owner), `ewing-open-data-catalog` (consumer)
- Competitive & improvement analysis — `COMPETITIVE-ANALYSIS.md` (basis for the v0.2 merge)

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

---

## Changelog — v0.2 (analysis merged)

This version merges the findings of `COMPETITIVE-ANALYSIS.md` (analyst pass, 2026-06-29) into the plan.
Changes are surgical/additive; no cancer guardrail was weakened, and no facts were invented. Web-cited
claims trace to the analysis's Sources list.

**Correctness / license / safety / schema fixes applied:**
1. **cBioPortal license accuracy (§1A).** Added **ODC-ODbL (share-alike) as cBioPortal's stated default** to
   the licensing matrix, the canonical `license` model (new `shareAlike` flag), the permits-derivatives
   criterion, and output-licensing — and made the **CC-BY-vs-share-alike compatibility a required reviewer
   ruling** (FLAG until ruled), replacing the prior "per-study TCGA-derived = open" understatement.
2. **Croissant 1.1 DUO/PROV-O (§1B).** Retargeted the spec to **Croissant 1.1's DUO + PROV-O permission/
   provenance layers** that natively encode access-tier/consent, with a required recorded decision + scheduled
   bump if 1.0 is held transitionally; threaded into specVersions, dependencies, fixtures, M2, references.
3. **Per-file/per-release tier (§1C).** Dropped the durable "masked somatic mutations = open" claim; tier is
   now a **per-file, per-GDC-release re-checked attestation**; **CPTAC** leans cautious (controlled
   components possible).
4. **Upstream version DOI (§1F).** Added `provenance.upstreamVersionDoi` (DepMap per-quarter Figshare DOI,
   GDC release-id) as a required, completeness-scored field; folded into M3 refresh.
5. **Machine-enforced provenance (§1G).** Added a **citation-coverage lint** (assertion count == citation
   count) feeding the completeness score; **redefined the before-score** against the source portal's
   *existing* metadata (GDC fields / GEO MIAME), not against nothing.
6. **Small-cell over-exclusion (§1 minor).** Added a **suppression-aware exception** so legitimately-open
   aggregate count tables that trip k≥5 route to documented review, not an auto-halt (individual-level still
   always EXCLUDE).
7. **Ontology ambiguity (§1 minor).** Fixed **OncoTree as the primary disease vocabulary**, NCIt/DO secondary.
8. **Reuse contract / superset (§1D, §1E).** Declared an explicit **reuse contract**: reuse
   `open-data-datasheets`'s shared canonical model / Croissant validator / inspection protocol; this project
   is the **genomics superset**; `ewing-open-data-catalog` **consumes/links** these datasheets. No parallel
   validator reimplementation; added a triplication-drift risk row.
9. **Metric realism (§1H).** Conditioned the cumulative acceptance targets on the **License + Genomic-Privacy
   reviewer being seated** and an acceptance channel existing.

**Strategy integrated:**
- New **## Competitive landscape & differentiation** section (NCI GDC/CRDC, cBioPortal, DepMap, GEO,
  Kaggle, MLCommons Croissant, Data Nutrition, HF cards, Google Dataset Search, OmicsDI), with the
  **two-part VERIFIED gate** (access-tier + identifiability + cited license clause = a committed,
  auditable "safely-open + proof" verdict) as the differentiator no portal ships.
- **Claude API leverage** folded into the architecture (narrative drafting with per-sentence provenance;
  license-clause extraction → structured verdict **proposal** for human confirmation; schema→Croissant
  mapping; cBioPortal clinical-field harmonization to OncoTree/NCIt) — with hard guardrails keeping
  access-tier/license determinations **human-verified** and **never** ingesting controlled/identifiable data.
- **Optimizations folded into the Roadmap** (M0: reuse contract, ODbL ruling, Croissant decision, citation
  lint, version DOI, OncoTree; M2: harmonization + 1.1 emission; M3: version-DOI refresh + Google Dataset
  Search distribution).
- New **## Adjacent opportunities** section (shared verified-datasheet core, `open-cohort-catalog`,
  `cancer-data-dictionaries`, verified-datasheet MCP server, gate-as-a-service, Nutrition-Label adapter).
- **Open questions merged** (ODbL/CC-BY ruling, Croissant 1.0-vs-1.1, reuse contract ownership, ewing
  boundary, small-cell exception, reviewer-gated targets).

**Preserved:** all binding cancer guardrails (open/de-identified only; controlled-access dbGaP/EGA/DACO
out of scope; per-source license verify; provenance-on-every-assertion; no medical advice / oncologist +
advocate sign-off at `riskTier: high`), the vision, structure, and all valid v0.1 content (incl. Appendix A).
