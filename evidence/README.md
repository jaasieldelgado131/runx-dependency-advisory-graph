# Recomputed evidence

This directory contains the output of the successful governed run at
https://github.com/jaasieldelgado131/runx-exact-cve-audit/actions/runs/27807231592.

The run audited the immutable OWASP NodeGoat commit
`c5cb68a7084e4ae7dcc60e6a98768720a81841e8`. It queried 16 exact direct
production dependency versions, reported 13 OSV advisories affecting six
dependencies, then independently replayed every exact-version query. The
verification packet records zero false hits and zero missing hits.

## Files

- `artifacts/report.md`: human-readable findings.
- `artifacts/evidence.json`: exact queries and advisory evidence.
- `artifacts/audit-result.json`: complete inventory and findings.
- `artifacts/verification.json`: independent replay result.
- `artifacts/delivery.json`: final governed delivery packet.
- `portable-receipts/*.json`: the four signed runx receipts. Colons in the
  original content-addressed filenames are replaced with underscores for
  Windows-compatible distribution; receipt content and IDs are unchanged.
- `summary.json`: compact target, result, verification, and delivery digest.

## Verify receipts

The workflow signs with a published demonstration key so any stranger can
verify the receipts without a secret. Its trusted public key is:

```text
kid: runx-public-demo-key
Ed25519 public key (base64): IVL40Zt5HSRFMkLhXy6rbLfP+ntqXtMAl5YOBpiB2xI=
```

With `@runxhq/cli@0.6.2` installed, set these variables and verify each receipt:

```sh
export RUNX_RECEIPT_VERIFY_KID=runx-public-demo-key
export RUNX_RECEIPT_VERIFY_ED25519_PUBLIC_KEY_BASE64=IVL40Zt5HSRFMkLhXy6rbLfP+ntqXtMAl5YOBpiB2xI=
runx verify --receipt evidence/portable-receipts/sha256_07b6520aff9055c5ae6acfb71c79a9a1bf87d9c4d9cfccbbd0deba9f612378e6.json --json
```

The same strict verification ran against all four receipts in GitHub Actions.
