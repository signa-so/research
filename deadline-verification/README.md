# Deadlines You Can Docket Against

**Purpose: so you can verify, rather than take on faith, the deadlines the
Signa API computes.**

Signa computes trademark lifecycle deadlines (renewal cycles, US
declarations of use, grace periods, restoration windows, and opposition
periods) from statutory rules (22 jurisdictions at the version 1.0 evaluation; 29
in October 2026, see `GET /v1/deadline-rules`). A deadline
product asks for a specific kind of trust: a missed renewal does not degrade
a registration, it cancels it. This evaluation measures Signa's computed
deadlines against the register itself, and this folder contains everything
needed to check the work: the frozen samples, the manifests that prove they
were frozen before any comparison ran, and the per-record results including
every disagreement and its classification.

The full report is published at
https://docs.signa.so/research/deadline-verification
([report.pdf](./report.pdf) in this folder).

## The study in brief

We froze a sample of 31,547 registrations across ten trademark offices
(hash-manifested before any comparison ran), then an expanded same-seed
sample of 306,896 as a stability check, and compared computed deadlines
against three complementary forms of ground truth: the dates offices publish
on their own records, 27,562 renewal and maintenance filings that actually
took place, and 34,132 real opposition proceedings.

How to read the table: the first row is what the API serves; the next two
score the statutory computation on its own, with the office's date
withheld; "declared" means the engine stated that it needed the office's
date rather than guessing (version 1.1, 2026-10-04).

| What was measured | Result |
|---|---|
| Agreement of the schedule the API serves with office-published dates (anchors on the office's stated date where one exists) | **99.99%** (29,292 of 29,295; 99.99% production-weighted) |
| Records either verified exactly or explicitly declared as needing the office's date | **99.68%** of 29,295 |
| Pure statutory computation, office's date withheld, on the records the engine computes | **99.65%** of 26,800 (99.82% production-weighted) |
| Records declared rather than guessed (three documented kinds, stated in the product) | 2,495 (8.5%) |
| Observed renewal and maintenance filings inside computed windows | **94.7%** of 24,091 on computed records (99.7% at USPTO); **92.7%** of all 27,562 on the served schedule |
| Observed opposition filings inside computed opposition windows | **91.0%** of 23,751 (99.7% at EUIPO) |
| Disagreements remaining after the declared kinds are set aside | 95 records, each classified |
| Unexplained residual after classifying every disagreement | **3 records** |

Version 0.91 (July 2026) scored the engine's guesses on the declared kinds
against it; its full-population figure was 96.08% of 282,393 comparisons at
expanded scale. The v1.1 per-record results are `results/run-2026-10-04-v1.1.json`
(office date withheld), `results/run-2026-10-04-v1.1-anchored.json` (as served)
and `results/oppositions-2026-10-04-v1.1.json` (oppositions); the v1.0 results
(`results/run-2026-08-30-v1*.json`) are kept for comparison.

## Version 1.1 (2026-10-04)

A measurement refresh on the same frozen samples, at the same as-of date
(2026-07-07), against the current rules. No change of method or policy.
Two corrections:

- **Served schedule.** An evaluation-tool scoring error compared correctly
  rolled due dates with un-rolled office expiries on the served path. Fixed,
  the served schedule reproduces the office's date for 29,292 of 29,295
  records (99.99%), against 99.65% and 99.89% reported in version 1.0. The
  Singapore served figures previously attributed to register errors are
  withdrawn.
- **Sweden.** The "Swedish 2019 trigger" rule defect is withdrawn: the
  original filing-date key was correct under the transitional provisions
  of SFS 2018:1652. The rule-defect total is six, not seven.

Served-schedule filings inside computed windows moved from 93.2% to 92.7%
after re-measuring on the released engine, which no longer projects a
renewal term past a lapsed office-stated expiry. Every movement is listed
in the report's changelog.

The evaluation worked in both directions: it surfaced six defects in
Signa's own rules, each fixed, tied to its statute, and published with
agreement before and after, and three in data ingestion (two fixed, one
filed); and it identified 311 records classified by Signa as register-data errors, with
per-record evidence, where the register rather than the computation carries
the wrong date.

**This is a first-party evaluation.** Signa selected the metrics, wrote the
evaluator, corrected the system under test, and classified the
disagreements. It has not been independently audited. That is exactly why
these artifacts are public.

## What is in this folder

- `report.pdf`: the full report (methodology, per-office results, every
  fix with its legal basis, limitations).
- `manifests/`: frozen sample manifests (seeded sampling design and
  SHA-256 digests of the record-identifier lists, written before any
  comparison ran). `strata-populations.json` carries the production
  population counts used for the weighted results.
- `data/samples/`, `data/events/`: the frozen 31,547-record maintenance
  sample and its register events (gzipped JSON, public register data).
- `data/oppositions/`: the frozen 34,132-proceeding opposition sample.
- `results/`: per-run comparison outputs, including per-record
  disagreement lists and their mechanism classifications, retained for
  every stage (pre-fix and post-fix runs both).

## Check it yourself

The rule corpus under test is inspectable through the API with statutory
citations: `GET /v1/deadline-rules` and `GET /v1/opposition-rules`. Any
deadline in the sample can be recomputed via `POST /v1/deadlines/compute`
with an API key. No customer data appears in any evaluation set; source
data is public trademark register data.
