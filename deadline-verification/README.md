# Deadlines You Can Docket Against

**Purpose: so you can verify, rather than take on faith, the deadlines the
Signa API computes.**

Signa computes trademark lifecycle deadlines (renewal cycles, US
declarations of use, grace periods, restoration windows, and opposition
periods) from statutory rules modeled for 22 jurisdictions. A deadline
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

| What was measured | Result |
|---|---|
| Full-population exact agreement with office-published dates (primary result) | **96.08%** of 282,393 comparisons |
| Production-weighted exact agreement, pure statutory computation | **98.55%** |
| Agreement on the modeled population (post hoc subgroup, exclusions documented) | 99.74% |
| Office-date-anchored schedule the API serves (uses the office's stated date as an input) | 99.89% |
| Observed renewal and maintenance filings inside computed windows | **93.1%** (99.4% at USPTO) |
| Observed opposition filings inside computed opposition windows | **90.2%** (98.1% at EUIPO) |
| Unexplained residual after classifying every disagreement | **0.001%** (3 records) |

The evaluation worked in both directions: it surfaced seven defects in
Signa's own rules and three in data ingestion (each fixed and published with
agreement before and after, every fix tied to its statute), and it
identified 311 records classified by Signa as register-data errors, with
per-record evidence, where the register rather than the computation carries
the wrong date.

**This is a first-party evaluation.** Signa selected the metrics, wrote the
evaluator, corrected the system under test, and classified the
disagreements. It has not been independently audited. That is exactly why
these artifacts are public.

## What is in this folder

- `report.pdf` — the full report: methodology, per-office results, every
  fix with its legal basis, limitations.
- `manifests/` — frozen sample manifests: seeded sampling design and
  SHA-256 digests of the record-identifier lists, written before any
  comparison ran. `strata-populations.json` carries the production
  population counts used for the weighted results.
- `data/samples/`, `data/events/` — the frozen 31,547-record maintenance
  sample and its register events (gzipped JSON, public register data).
- `data/oppositions/` — the frozen 34,132-proceeding opposition sample.
- `results/` — per-run comparison outputs, including per-record
  disagreement lists and their mechanism classifications, retained for
  every stage (pre-fix and post-fix runs both).

## Check it yourself

The rule corpus under test is inspectable through the API with statutory
citations: `GET /v1/deadline-rules` and `GET /v1/opposition-rules`. Any
deadline in the sample can be recomputed via `POST /v1/deadlines/compute`
with an API key. No customer data appears in any evaluation set; source
data is public trademark register data.
