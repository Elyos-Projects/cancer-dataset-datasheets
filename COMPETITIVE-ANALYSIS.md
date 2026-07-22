# Competitive & Improvement Analysis — `cancer-dataset-datasheets`

_Analyst pass · 2026-06-29 · Sources web-verified and cited inline. Scope under review: Datasheets-for-Datasets + Croissant + provenance + license records for OPEN cancer datasets (TCGA/GDC open tiers, GEO, cBioPortal, DepMap, ICGC open). Documentation only, never the data._

This is the **cancer-wide** sibling of `open-data-datasheets` (general open/civic data) and `ewing-open-data-catalog` (single-disease discovery). Differentiation vs. those siblings is treated as a first-class correctness concern throughout.

---

## 1. Correctness & completeness review of PLAN.md

The plan is unusually rigorous: the two-part gate (access-tier/identifiability **before** license), the genomics-aware k≥5 identifiability scanner, the canonical-model-first invariant rejecting `accessTier != open` by construction, "provenance on every assertion," and the explicit COSMIC/OncoKB flag are all correct and well-operationalized (not slogans). The 25-item Appendix A shows the obvious gaps were already self-caught. Remaining concrete issues:

**A. License accuracy — cBioPortal is the biggest factual gap.** The matrix (line 270) says cBioPortal data is "redistributed under each original study's terms — must be verified per study." That under-states the actual default: cBioPortal's stated terms are that, **unless otherwise noted, data are available under the ODC Open Database License (ODbL)** with attribution, and some studies additionally restrict commercial use ([cBioPortal FAQ](https://docs.cbioportal.org/user-guide/faq/)). **ODbL is a share-alike license** — it is materially different from CC-BY and the plan never mentions ODbL anywhere. The sibling `open-data-datasheets` plan *does* treat ODbL/share-alike as a first-class policy decision (`policy-022`). This plan must (a) add ODbL to the matrix and the canonical `license` model, (b) decide whether share-alike terms permit CC-BY-licensed derivative *documentation* (the plan licenses its own output CC-BY — a potential conflict with an ODbL share-alike obligation on derived data, though metadata-about-data likely escapes the database right; this needs the License+Privacy reviewer to rule, not a guess), and (c) stop implying cBioPortal is purely "per-study TCGA-derived = open."

**B. Croissant version is already stale / a richer option exists.** The plan pins **Croissant v1.0** (lines 218, 424). As of Feb 2026, **Croissant 1.1** is published, and critically it adds first-class support for the **DUO (Data Use Ontology)** and PROV-O for machine-readable *permissions and provenance*, plus an MCP integration for agent consumption ([Croissant 1.1, MLCommons](https://mlcommons.org/2026/02/croissant-1-1-standard/); [Croissant+MCP](https://mlcommons.org/2025/10/croissant-mcp/)). DUO is the standard vocabulary for exactly the access-tier/consent constraints this project cares about (e.g. `DUO:0000004` no-restriction, disease-specific, non-commercial). Pinning v1.0 is defensible for stability, but the plan should (1) record *why* it isn't using 1.1's DUO/PROV-O permission layer, which is squarely on-mission, and (2) schedule the bump, because emitting consent/access-tier facts as free-text prose when a machine-readable ontology exists is a missed differentiator.

**C. Open vs controlled tier distinctions — mostly correct, two refinements.** The GDC open/controlled split is accurately stated (open data needs no authorization but is bound by the NIH GDS non-re-identification clause; controlled needs dbGaP/DAC approval — [GDC Data Access Policies](https://gdc.cancer.gov/access-data/data-access-policies)). Refinements: (1) GDC **open-access masked somatic mutation MAFs were themselves moved to controlled access** in past GDC policy iterations for some projects — the plan must verify tier *per file per GDC data release*, not per data type, since "masked somatic mutations = open" (line 268) is not durably true. (2) **CPTAC** is asserted "generally open" (line 276) but CPTAC genomic/germline-adjacent data also has controlled components in the GDC/PDC split; "verify per dataset" is right but the default lean should be more cautious.

**D. Overlap / dedup with siblings is the structural risk and is under-specified.** The plan builds a *fresh* canonical metadata model, Croissant validator, gate checklist, inspection protocol, and CC-BY/MIT licensing — nearly all of which already exist in `open-data-datasheets` (its canonical model, Croissant v1.0 validator with golden fixtures, bounded 1,000-row/5MB inspection protocol, and CC-BY-output/MIT-code split are line-for-line ancestors). The plan references the sibling only as "house style" (line 503). **It should explicitly declare which components it reuses vs. forks.** Genuinely cancer-specific additions (the access-tier+identifiability gate, germline scanner, k≥5 check, NCIt/OncoTree ontologies, cancer license matrix) justify a *superset*, not a parallel reimplementation. Without an explicit reuse contract, Hee-Lee Oss ends up maintaining three drifting Croissant validators.

**E. Scope vs `ewing-open-data-catalog`.** Ewing is a single-disease *catalog/discovery* effort; this is cancer-wide *deep per-dataset datasheets*. Any Ewing-sarcoma open dataset is in-scope for *both*. The plan never states the boundary. It should: cancer-dataset-datasheets owns the datasheet+gate+Croissant artifact; ewing-open-data-catalog *consumes/links* those artifacts for its disease vertical (catalog points at datasheets; datasheets don't re-implement a disease catalog). Otherwise the same TCGA-EWS / TARGET / GEO Ewing series get documented twice with divergent verdicts.

**F. Versioning/DOIs — partially handled, one gap.** Provenance captures release/data-freeze/cadence and a Zenodo DOI for the *documentation*. But cancer datasets are versioned aggressively (GDC data releases ~quarterly; **DepMap is quarterly — 23Q4, 24Q2, 24Q4** per [Figshare](https://plus.figshare.com/articles/dataset/DepMap_24Q4_Public/27993248)). The plan should require pinning the **upstream dataset DOI/release identifier** (DepMap Figshare per-quarter DOI; GDC data-release number) inside the datasheet, not only a free-text "release" string — otherwise a datasheet silently describes an ambiguous "DepMap public" rather than "DepMap 24Q4 (DOI …)."

**G. Completeness rubric — robust but partly subjective.** The 0–100 score ties to field-population + source-verification + provenance-citation coverage, which is good. Weakness: "every assertion carries a provenance citation" is binary-scored by a human; there's no automated check that *count(assertions) == count(citations)*. Consider a lint that flags datasheet sentences lacking a citation anchor, so the 90/100 bar is machine-enforced, not reviewer-judged. Also: the "before-score on the dataset as-published" is ill-defined for a dataset that has *no* existing datasheet (before = near-0 trivially), inflating the apparent improvement — define before-score against the source portal's *existing* metadata (GDC fields, GEO MIAME record), not against nothing.

**H. Metric realism.** "6 accepted in 6 months" with a still-unsecured License+Genomic-Privacy reviewer (the named hard blocker, line 412) and zero confirmed steward is optimistic; M0's Zenodo self-serve fallback is the only thing making any acceptance achievable. The plan honestly flags `verifiedNeed:false` until a steward exists — good — but the 6-accepted target should be explicitly conditioned on the reviewer being seated, or it reads as aspirational.

**Minor:** "Disease Ontology" + OncoTree + NCIt is three overlapping disease vocabularies; pick a primary (OncoTree is the de-facto cancer standard cBioPortal uses) to avoid mapping ambiguity. SEER aggregate-only stance (line 276) is correct. The germline scanner's "quasi-identifier age+sex+rare-diagnosis+geo below k=5" is sound but rare-cancer cohorts routinely violate k≥5 even in *aggregate* count tables (small-cell counts) — the plan should anticipate that many legitimately-open cancer summary tables will trip the scanner and need a documented suppression-aware exception, or it will over-EXCLUDE.

---

## 2. Competitive landscape

**Documentation standards / frameworks**
- **Datasheets for Datasets (Gebru et al., CACM 2021).** The 57-question, 7-section questionnaire the plan adopts ([arXiv 1803.09010](https://arxiv.org/abs/1803.09010); [CACM](https://cacm.acm.org/research/datasheets-for-datasets/)). _Strength:_ widely accepted vocabulary, peer-reviewed. _Gap:_ prose-only, no machine-readable output, no license/access-tier verification, no genomics/identifiability notion. The plan correctly treats it as a *template*, not a competitor.
- **MLCommons Croissant (1.0 / now 1.1).** Machine-readable JSON-LD extending schema.org; 1.1 adds DUO/PROV-O permission+provenance layers and MCP ([1.1 announce](https://mlcommons.org/2026/02/croissant-1-1-standard/); [spec](https://docs.mlcommons.org/croissant/docs/croissant-spec.html)). _Strength:_ the interoperability substrate, indexable by Google Dataset Search. _Gap:_ a *format*, not curated verified content; says nothing about whether a given cancer file is open vs dbGaP-controlled.
- **Data Nutrition Project (Dataset Nutrition Label, 2nd Gen 2022).** FDA-style standardized label; 501c3 ([datanutrition.org](https://datanutrition.org/label/); [arXiv 2201.03954](https://arxiv.org/abs/2201.03954)). _Strength:_ accessible "at-a-glance" framing — a model for the optional M4 patient explainers. _Gap:_ generic, not cancer/genomics-specific, no license-gate, limited live coverage.
- **Hugging Face dataset cards.** README + YAML frontmatter (license/size/task), `mlcroissant` export supported ([HF docs](https://huggingface.co/docs/hub/datasets-cards)). _Strength:_ huge reach, standardized sections, Croissant interop. _Gap:_ author-self-reported and frequently empty/unverified; license fields routinely wrong; no genomic-privacy gating; not where cancer-portal data lives.

**Discovery / search**
- **Google Dataset Search.** Indexes schema.org/`Dataset` JSON-LD across the web ([research.google](https://research.google/blog/building-google-dataset-search-and-fostering-an-open-data-ecosystem/)). _Strength:_ universal discovery surface; emitting Croissant/schema.org makes our datasheets findable. _Gap:_ indexes whatever publishers assert — no verification, no provenance check; a complement, not a rival.
- **OmicsDI (EMBL-EBI).** Aggregates metadata across genomics/proteomics/transcriptomics repositories with a unified REST API and citation-based impact scoring ([NAR 2020](https://academic.oup.com/nar/article/48/W1/W380/5831190)). _Strength:_ broad omics metadata harmonization + discovery, established. _Gap:_ thin per-dataset metadata (discovery-grade, not datasheet-depth); no license-verification gate, no access-tier/identifiability verdict, no Datasheets-for-Datasets narrative.

**The cancer data portals themselves (the real incumbents)**
- **NCI GDC / CRDC.** Authoritative TCGA host; clear open/controlled tiering and GDS policy ([GDC policies](https://gdc.cancer.gov/about-gdc/gdc-policies); [CRDC](https://datacommons.cancer.gov/cancer-research-data-commons)). _Strength:_ canonical, harmonized, programmatic API; the source of truth on tier. _Gap:_ documentation is portal-/schema-centric and technical; no Datasheets-for-Datasets narrative, no Croissant, no plain-language layer; tier facts live in policy pages, not per-file machine metadata.
- **cBioPortal.** Open-source portal over TCGA/TARGET + many individual studies ([about](https://about.cbioportal.org/)). _Strength:_ rich genomic browsing, ODbL default with per-study notes. _Weakness:_ **clinical metadata is "extremely heterogeneous and lacks standardization" — different studies use arbitrary terms for identical entities** ([per recent curation literature](https://www.biorxiv.org/content/10.1101/2025.11.26.689816v1)); license is per-study and easy to get wrong. This heterogeneity is precisely the gap to fill.
- **DepMap (Broad).** CCLE/dependency data, **CC BY 4.0 on Figshare, quarterly releases** ([DepMap 24Q4 Figshare](https://plus.figshare.com/articles/dataset/DepMap_24Q4_Public/27993248); [Bioconductor depmap](https://bioconductor.org/packages/depmap)). _Strength:_ cleanly open, DOI-per-release, no login. _Gap:_ documentation is release-notes + readmes; no standardized datasheet/Croissant; version drift across quarters is a documentation burden nobody centrally solves.
- **GEO (NCBI).** MIAME/MINSEQE-compliant series submissions ([GEO](https://www.ncbi.nlm.nih.gov/geo/); [FAQ](https://www.ncbi.nlm.nih.gov/geo/info/faq.html)). _Strength:_ minimum-metadata enforced at submission, stable accessions (GSE/GSM/GPL). _Gap:_ MIAME is *minimum*; processing/license/reuse-fitness is often thin or free-text; some series link controlled SRA/dbGaP components — exactly the open-vs-controlled boundary risk the plan targets.
- **Kaggle cancer datasets.** _Strength:_ accessible, popular for ML. _Gap:_ provenance/license frequently broken or mis-stated; re-hosted derivatives with lost lineage — an anti-pattern that motivates this project.

**Bottom line:** no incumbent delivers *verified, license-gated, access-tier-aware, provenance-complete, machine-readable* datasheets across cancer portals. Portals own the data and tier-of-record; standards own the format; none own the **curated, verified bridge**.

---

## 3. Gaps we can fill

1. **A verified open/controlled tier verdict per dataset/file** — turning GDC/ICGC/CPTAC policy prose into a committed, cited, machine-readable attestation (Croissant 1.1 DUO would express this natively).
2. **Cross-portal license harmonization with cited clauses** — one matrix that correctly distinguishes CC-BY (DepMap), ODbL share-alike (cBioPortal default), GDS-open (GDC), NC/custom (COSMIC/OncoKB), and per-series GEO checks — the single thing every reuser gets wrong.
3. **Standardization over cBioPortal's documented metadata heterogeneity** — mapping arbitrary per-study clinical terms to NCIt/OncoTree, which the portal itself does not enforce.
4. **Provenance-on-every-assertion + license snapshot (copy+SHA-256+Wayback)** — a verifiability standard neither HF cards nor portal readmes meet.
5. **A genomics-aware identifiability/germline gate** — a reusable artifact that documents *why a dataset is safely open*, which no documentation framework provides.
6. **Version/DOI pinning across quarterly re-releases** (DepMap 24Q4, GDC releases) so a datasheet names an exact, citable dataset version.
7. **Machine-readable + Google-Dataset-Search-indexable output** for cancer datasets whose metadata currently isn't emitted as schema.org/Croissant.
8. **An education-only, expert-reviewed plain-language layer** (the Data-Nutrition-style M4) — currently nonexistent for open cancer datasets.

---

## 4. Differentiators to win

- **The two-part gate is the moat.** Access-tier + identifiability verified *before* license, both as committed auditable artifacts. No competitor (portal, HF, OmicsDI, Croissant) ships a verified "this is safely open and here's the cited proof" verdict.
- **Verification, not assertion.** HF cards and Google Dataset Search index self-reported metadata; we deliver *source-verified, cited, snapshotted* records with a zero-tolerance privacy/safety metric.
- **Cancer-license-matrix-of-record.** A correctly-tiered, clause-cited matrix spanning GDC/GEO/cBioPortal(ODbL)/DepMap/COSMIC/OncoKB/ICGC/CPTAC/SEER — the artifact everyone needs and nobody maintains.
- **Machine-readable permissions via Croissant (push to 1.1 DUO/PROV-O).** Emitting consent/access constraints as ontology terms, not prose, is uniquely on-mission and future-proofs for agent consumption + Google Dataset Search indexing.

**Vs. the Hee-Lee Oss siblings (explicit division of labor):**
- **`open-data-datasheets`** = general/civic open data, **portal adapters (CKAN/Socrata/DCAT-US)**, low–medium risk, no genomics. **Owns the shared toolkit**: canonical model, Croissant validator, bounded inspection protocol, CC-BY/MIT split. → cancer-dataset-datasheets should **reuse these as a dependency and extend them**, not fork. Cancer project = the genomics *superset* (adds tier/germline gate, cancer license matrix, NCIt/OncoTree, oncologist-review track).
- **`ewing-open-data-catalog`** = single-disease **discovery/catalog** (find/link Ewing-sarcoma datasets). → cancer-dataset-datasheets **produces the deep datasheet artifact**; ewing **consumes/links** it for its vertical. Catalog points at datasheets; datasheets don't re-build a disease catalog.
- **Net:** three layers — *general toolkit* (open-data-datasheets) → *cancer-wide verified datasheets + gate* (this project) → *disease-specific discovery* (ewing). Win condition: one shared codebase, three scopes, **zero validator triplication**.

---

## 5. Claude API leverage

**Where Claude adds clear value (always human-verified):**
1. **Drafting the Datasheets-for-Datasets narrative** from inspected schema + portal metadata — generating the 7-section questionnaire answers as a *reviewer-ready draft*, each sentence tagged with a provenance anchor for the citation-coverage lint.
2. **License-clause extraction → structured verdict proposal.** Claude reads the license text/terms page and *proposes* `permitsDerivatives`, `nonCommercial`, share-alike, and the exact cited clause — a structured candidate the License+Privacy reviewer confirms or overturns. (High leverage on cBioPortal's per-study notes and ODbL vs CC-BY distinction.)
3. **Schema → Croissant JSON-LD mapping + metadata harmonization** — emit valid Croissant (target 1.1 DUO/PROV-O), and normalize cBioPortal's heterogeneous clinical field names to NCIt/OncoTree codes — exactly the "arbitrary terms for identical entities" problem the literature flags.
4. **Identifiability/quasi-identifier triage assist** — Claude flags candidate quasi-identifier combinations and small-cell-count risks for the scanner/reviewer (assist, never the gate decision).
5. **Plain-language M4 explainer drafts** — education-only first drafts for oncologist+advocate review.

**Where Claude must NOT decide (hard guardrails):**
- **Access-tier and license/`permitsDerivatives` determinations are human-verified.** Claude *proposes with citations*; the named License+Genomic-Privacy reviewer *decides*. An LLM mislabeling a controlled file as open is the project's critical failure mode.
- **No fabricated provenance.** Every citation must resolve to a real source; Claude must never invent a DOI, license URL, accession, or clause. Citation anchors are verified, not trusted.
- **Never ingest controlled/identifiable data.** Claude operates only on confirmed open-tier, bounded (≤1,000 rows/5MB), aggregate/de-identified samples; on any germline/individual-level/controlled signal, halt — Claude must not be pointed at dbGaP/EGA/DACO endpoints.
- **No clinical interpretation.** No prognosis, biomarker, or treatment inference — descriptive, sourced, education-only.
- **No de-identification by us** — Claude documents the *publisher's* de-identification; it never anonymizes/aggregates data itself (that's transforming data, out of scope).

_(Model/pricing specifics intentionally omitted — confirm current model IDs via the `claude-api` skill before any build; do not quote from memory.)_

---

## 6. Ten concrete optimizations

1. **Add ODbL (share-alike) to the matrix and canonical `license` model**, and have the reviewer rule on whether CC-BY documentation derived from an ODbL source is compatible — fixes the cBioPortal correctness gap (§1A).
2. **Adopt Croissant 1.1's DUO + PROV-O permission/provenance layers** (or formally record why not + schedule the bump), emitting access-tier/consent as ontology terms not prose (§1B).
3. **Pin upstream dataset version DOI/release-id** (DepMap Figshare per-quarter DOI, GDC data-release number) as a required provenance field (§1F).
4. **Verify tier per-file per-release**, not per-data-type — drop the durable claim "masked somatic mutations = open" and make tier a re-checked attestation (§1C).
5. **Machine-enforce provenance-on-every-assertion** with a citation-coverage lint (assertion count == citation count) feeding the completeness score (§1G).
6. **Redefine the before-score** against the source portal's *existing* metadata (GDC fields/GEO MIAME), not against nothing, so "improvement" isn't trivially inflated (§1G).
7. **Reuse, don't fork, `open-data-datasheets`'s canonical model + Croissant validator + inspection protocol**; ship a written reuse contract listing shared vs cancer-specific components (§1D).
8. **Add a suppression-aware exception path** for legitimately-open small-cell aggregate cancer tables that trip k≥5, to prevent over-EXCLUSION of valid open summaries (§1 minor).
9. **Pick OncoTree as the primary disease vocabulary** (de-facto cBioPortal standard), with NCIt/DO as secondary crosswalks, to remove three-vocabulary mapping ambiguity.
10. **Emit schema.org/`Dataset` + Croissant JSON-LD for every datasheet and host where Google Dataset Search can crawl it** (e.g. Zenodo/GitHub Pages) so verified datasheets become discoverable, converting the Zenodo fallback into a real distribution channel.

---

## 7. Parallel & perpendicular spin-offs

- **Shared Hee-Lee Oss "verified-datasheet core"** — extract the canonical model + Croissant validator + bounded inspection protocol into one package consumed by open-data-datasheets, cancer-dataset-datasheets, and ewing-open-data-catalog (kills triplication; §4).
- **`open-cohort-catalog`** — a discovery index layered on these datasheets (cancer-wide cohort finder), the cancer analogue of what ewing does for one disease; consumes datasheets, doesn't re-document.
- **`cancer-data-dictionaries`** — spin the per-field dictionary + NCIt/OncoTree crosswalk into a standalone harmonization asset that directly attacks cBioPortal's documented metadata heterogeneity (could be contributed upstream).
- **Verified-datasheet MCP server** — expose accepted datasheets/Croissant via MCP so agents query "is dataset X open, what's its license, what fields" with cited provenance — aligns with Croissant's own MCP direction ([MLCommons](https://mlcommons.org/2025/10/croissant-mcp/)). Read-only, open-tier metadata only.
- **"Gate-as-a-service"** — package the access-tier + identifiability + license gate as a reusable check other Hee-Lee Oss health/genomics projects (and external curators) invoke before touching a dataset.
- **Datasheet ↔ Dataset-Nutrition-Label adapter** — auto-render the M4 plain-language label from the canonical model (reuses Data Nutrition Project's accessible framing for patients/advocates).

---

## 8. Open questions for the maintainer

1. **cBioPortal/ODbL:** Does CC-BY-licensed *documentation* derived from an ODbL (share-alike) source create a license conflict, and what's the reviewer's standing rule? (Blocks the entire cBioPortal source.)
2. **Croissant 1.0 vs 1.1:** Pin 1.0 for stability or adopt 1.1's DUO/PROV-O permission layer that directly encodes access-tier/consent? When is the bump scheduled?
3. **Reuse contract with `open-data-datasheets`:** Which components are shared dependencies vs cancer-specific extensions — and who owns the shared core?
4. **Boundary with `ewing-open-data-catalog`:** Confirm catalog-consumes-datasheet (not parallel documentation) for any Ewing-sarcoma open dataset.
5. **Small-cell / k≥5 over-exclusion:** What's the documented exception for legitimately-open aggregate cancer count tables that trip the identifiability scanner?
6. **Reviewer-gated targets:** Is the "6 accepted in 6 months" target explicitly conditioned on the License+Genomic-Privacy reviewer being seated (the named hard blocker)?
7. **Which OncoTree/NCIt/DO is primary** for the canonical `ontology.disease[]` field?
8. **DepMap version policy:** Pin one quarterly release per datasheet, or maintain a rolling refresh task as each quarter ships?

---

### Sources
Datasheets for Datasets — https://arxiv.org/abs/1803.09010 · https://cacm.acm.org/research/datasheets-for-datasets/ ·
Croissant — https://mlcommons.org/2026/02/croissant-1-1-standard/ · https://mlcommons.org/2025/10/croissant-mcp/ · https://docs.mlcommons.org/croissant/docs/croissant-spec.html ·
Data Nutrition Project — https://datanutrition.org/label/ · https://arxiv.org/abs/2201.03954 ·
Hugging Face dataset cards — https://huggingface.co/docs/hub/datasets-cards ·
Google Dataset Search — https://research.google/blog/building-google-dataset-search-and-fostering-an-open-data-ecosystem/ ·
OmicsDI — https://academic.oup.com/nar/article/48/W1/W380/5831190 ·
GDC/CRDC access policies — https://gdc.cancer.gov/access-data/data-access-policies · https://gdc.cancer.gov/about-gdc/gdc-policies · https://datacommons.cancer.gov/cancer-research-data-commons ·
cBioPortal terms (ODbL default) — https://docs.cbioportal.org/user-guide/faq/ · https://about.cbioportal.org/ · cBioPortal metadata heterogeneity — https://www.biorxiv.org/content/10.1101/2025.11.26.689816v1 ·
DepMap (CC BY 4.0, quarterly Figshare) — https://plus.figshare.com/articles/dataset/DepMap_24Q4_Public/27993248 · https://bioconductor.org/packages/depmap ·
GEO/MIAME — https://www.ncbi.nlm.nih.gov/geo/ · https://www.ncbi.nlm.nih.gov/geo/info/faq.html
