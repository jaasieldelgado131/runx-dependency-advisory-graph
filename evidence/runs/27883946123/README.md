# Sealed run 27883946123

- Workflow: https://github.com/jaasieldelgado131/runx-exact-cve-audit/actions/runs/27883946123
- Source commit: `3c0cc172f46dc68eacd5e2c5ba623978337e2800`
- Runx status: `sealed`
- Root receipt: `runx:receipt:sha256:ffdfafde844815fa10f7255d30202051ada5e164f15edd633e770a620f4b2c29`
- Target: OWASP NodeGoat commit `c5cb68a7084e4ae7dcc60e6a98768720a81841e8`
- Result: 16 exact dependencies queried, 13 advisory findings, zero false hits, zero missing hits

The `artifacts` directory is the exact workflow output. The `receipts` directory
contains the signed graph receipt and the three signed step receipts with
portable filenames. `runx-output.json` binds the root receipt to the successful
graph execution and lists every child receipt.
