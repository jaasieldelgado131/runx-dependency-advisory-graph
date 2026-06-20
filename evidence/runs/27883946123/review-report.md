# Exact CVE audit delivery review

This packet maps every acceptance criterion for Frantic bounty #21 to public,
recomputable evidence. The governed run audited the immutable OWASP NodeGoat
commit `c5cb68a7084e4ae7dcc60e6a98768720a81841e8` and completed successfully in
GitHub Actions.

## Acceptance matrix

- Complete governed skill: [`X.yaml`](https://raw.githubusercontent.com/jaasieldelgado131/runx-exact-cve-audit/main/X.yaml) declares typed inputs,
  graph outputs, network and filesystem scopes, named emits, packet schemas,
  and a fail-closed verification guard.
- Real dogfood run: [workflow run 27883946123](https://github.com/jaasieldelgado131/runx-exact-cve-audit/actions/runs/27883946123)
  executed the skill against OWASP NodeGoat on Linux and finished green.
- Exact versions and real advisories: [`evidence.json`](https://raw.githubusercontent.com/jaasieldelgado131/runx-exact-cve-audit/main/evidence/runs/27883946123/artifacts/evidence.json)
  records all 16 exact direct production dependency queries and 13 OSV
  advisory findings, each with package, installed version, advisory ID, URL,
  and exact OSV query.
- Zero false hits: [`verification.json`](https://raw.githubusercontent.com/jaasieldelgado131/runx-exact-cve-audit/main/evidence/runs/27883946123/artifacts/verification.json) independently
  replayed every exact-version OSV query and recorded `false_hits: 0` and
  `missing_hits: 0`.

## Immutable input

- Project: https://github.com/OWASP/NodeGoat
- Commit: `c5cb68a7084e4ae7dcc60e6a98768720a81841e8`
- Lockfile: https://raw.githubusercontent.com/OWASP/NodeGoat/c5cb68a7084e4ae7dcc60e6a98768720a81841e8/package-lock.json
- Lockfile SHA-256: `b9ed49893a9d3bcf6fe567cd38fe8168850591e0018f19d3bb0d1f80a4c2eecc`
- Scope: direct production dependencies from the lockfile packages map

## Verified result

- 16 exact dependency versions queried.
- 13 non-withdrawn advisory findings.
- 6 affected dependencies.
- 0 false hits after independent replay.
- 0 missing hits after independent replay.
- No target package code was installed or executed.

The complete finding-by-finding report is in [`report.md`](https://raw.githubusercontent.com/jaasieldelgado131/runx-exact-cve-audit/main/evidence/runs/27883946123/artifacts/report.md).
It includes each dependency, exact installed version, GHSA/CVE alias, advisory
URL, installed path, and advisory summary.

## Sealed receipt

- Root receipt: `runx:receipt:sha256:ffdfafde844815fa10f7255d30202051ada5e164f15edd633e770a620f4b2c29`
- [Signed root receipt](https://raw.githubusercontent.com/jaasieldelgado131/runx-exact-cve-audit/main/evidence/runs/27883946123/receipts/sha256_ffdfafde844815fa10f7255d30202051ada5e164f15edd633e770a620f4b2c29.json)
- [Runx execution output](https://raw.githubusercontent.com/jaasieldelgado131/runx-exact-cve-audit/main/evidence/runs/27883946123/artifacts/runx-output.json)
- [Delivery digest and artifact hashes](https://raw.githubusercontent.com/jaasieldelgado131/runx-exact-cve-audit/main/evidence/runs/27883946123/artifacts/delivery.json)

The root receipt is signed with Ed25519, has disposition `closed`, and links
the signed scan, verify, and finalize child receipts. The workflow verified all
four receipts with `runx receipt verify` before publishing this packet.

## Reproduction

1. Clone https://github.com/jaasieldelgado131/runx-exact-cve-audit.
2. Run `npm test`.
3. Run the `Reproduce sealed CVE audit` workflow or execute the governed
   command documented in the root README.
4. Compare the generated evidence and verification sets with this packet.
5. Verify the four JSON receipts using `runx receipt verify`.

The scanner fails closed if the lockfile digest changes, OSV is unavailable,
an installed version is not exact, or independent replay finds an unsupported
or omitted advisory.
