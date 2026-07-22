# cancer-dataset-datasheets

> Open cancer datasets — The Cancer Genome Atlas (TCGA, via the NCI Genomic Data Commons / GDC), the Gene Expression Omnibus (GEO), cBioPortal, and the Cancer Dependency Map (DepMap) — are among the mos  ·  **Risk tier:** med  ·  **Status:** planning

Open cancer datasets — The Cancer Genome Atlas (TCGA, via the NCI Genomic Data Commons / GDC), the Gene Expression Omnibus (GEO), cBioPortal, and the Cancer Dependency Map (DepMap) — are among the most valuable public resources in oncology. Yet they are frequently reused with **incomplete, scattered, or out-of-date documentation**: which access tier a file belongs to, whether the license actually permits the reuse a researcher has in mind, exactly which patients/samples are included and how they were processed, what was masked or filtered, and how to attribute the source correctly. These gaps slow cancer research, cause licensing mistakes (e.g. treating non-commercial COSMIC/OncoKB data as freely reusable), and — most seriously — create avenues for **re-identification or controlled-data mishandling** when the access-tier boundary is misunderstood.

**Definition of shipped:** portal/repo/archive** (per the per-channel acceptance definitions in Success metrics), with: open-access tier confirmed, no controlled/identifiable content, a verified license with cited `permitsDerivatives`, recorded provenance + required citation, **every assertion sourced**, c

This is an **Hee-Lee Oss** good-deed project. Contributors pull a task, do it with their own coding agent, and open a PR. Platform: https://github.com/jdev1977/hee-lee-oss

## Plan
- [PLAN.md](./PLAN.md) — robust enterprise plan (vision, architecture, roadmap, risks; includes an applied-improvements appendix + review sign-off)
- [TASKS.md](./TASKS.md) — schema-mapped task backlog
- [tasks/](./tasks/) — ready-to-pull task JSON(s)

## Contribute
```bash
hee-lee-oss browse
hee-lee-oss next --repo Hee-Lee-Oss-Projects/cancer-dataset-datasheets --no-fork
```

## Licensing & review
- Open license (see PLAN.md).
- Risk tier **med** — deeds are *delivered, not merged*; a domain reviewer (and expert sign-off for any high-stakes content) must approve before merge.

> Planning stage; no adopting partner secured yet (`verifiedNeed: false` on delivery-dependent tasks).
