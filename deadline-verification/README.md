# Deadlines You Can Docket Against: evaluation artifacts

Artifacts for the Signa report published at
https://docs.signa.so/research/deadline-verification.

- `manifests/` — frozen sample manifests: seeded sampling design and SHA-256
  digests of the record-identifier lists, written before any comparison ran.
  `strata-populations.json` carries the production population counts used
  for the weighted headline.
- `data/samples/`, `data/events/` — the frozen 31,547-record maintenance
  sample and its register events (gzipped JSON, public register data).
- `data/oppositions/` — the frozen 34,132-proceeding opposition sample.
- `results/` — per-run comparison outputs, including per-record
  disagreement lists and their mechanism classifications, retained for
  every stage (pre-fix and post-fix runs both).

The evaluation methodology, metric definitions, and every limitation are in
the report itself. The rule corpus under test is inspectable via the API:
`GET /v1/deadline-rules` and `GET /v1/opposition-rules`. Any deadline in the
sample can be recomputed via `POST /v1/deadlines/compute` with an API key.
