# runx dependency advisory graph

A governed runx graph skill that audits exact npm dependency versions against OSV.
It emits a report, machine-readable evidence, an independent replay
verification, and a delivery packet.

The checked-in harness targets the immutable OWASP NodeGoat commit
`c5cb68a7084e4ae7dcc60e6a98768720a81841e8` and its raw
`package-lock.json`. A second harness case uses an immutable clean manifest and
must remain free of findings.

## Local test

```sh
npm test
```

## Direct deterministic scan

```sh
RUNX_INPUTS_JSON='{
  "target_name": "OWASP NodeGoat",
  "target_repo": "https://github.com/OWASP/NodeGoat",
  "target_commit": "c5cb68a7084e4ae7dcc60e6a98768720a81841e8",
  "lockfile_url": "https://raw.githubusercontent.com/OWASP/NodeGoat/c5cb68a7084e4ae7dcc60e6a98768720a81841e8/package-lock.json",
  "dependency_scope": "direct-production"
}' node scripts/scan.mjs
```

## Governed run

```sh
runx skill . --runner audit \
  --input "target_name=OWASP NodeGoat" \
  --input "target_repo=https://github.com/OWASP/NodeGoat" \
  --input "target_commit=c5cb68a7084e4ae7dcc60e6a98768720a81841e8" \
  --input "lockfile_url=https://raw.githubusercontent.com/OWASP/NodeGoat/c5cb68a7084e4ae7dcc60e6a98768720a81841e8/package-lock.json" \
  --input "dependency_scope=direct-production" \
  --json
```

The run writes only to `artifacts/` and contacts only the immutable GitHub raw
lockfile URL and `api.osv.dev`.

## Reproducible evidence

The `Reproduce dependency advisory graph` GitHub Actions workflow runs the
vulnerable and clean harness cases, the unit tests,
executes the governed graph on Linux, verifies every emitted runx receipt, and
publishes the report, JSON evidence, delivery packet, and receipts as one
workflow artifact. The workflow pins `@runxhq/cli` to `0.6.6`.

The successful real-run outputs and independently verifiable receipts are also
checked into [`evidence/`](evidence/README.md) for anonymous public review.
