---
name: dependency-advisory-graph
description: Match exact npm dependency versions to OSV advisories and emit a fix-prioritized, independently verified graph evidence packet.
source:
  type: cli-tool
  command: node
  args:
    - scripts/scan.mjs
  timeout_seconds: 120
  sandbox:
    profile: workspace-write
    cwd_policy: skill-directory
    network: true
    writable_paths:
      - artifacts
    require_enforcement: false
inputs:
  target_name:
    type: string
    required: true
    description: Human-readable name of the audited project.
  target_repo:
    type: string
    required: true
    description: Public source repository URL.
  target_commit:
    type: string
    required: true
    description: Immutable source commit containing the lockfile.
  lockfile_url:
    type: string
    required: true
    description: Public raw URL for the package-lock.json at target_commit.
  dependency_scope:
    type: string
    required: false
    default: direct-production
    description: direct-production or all-installed.
runx:
  artifacts:
    named_emits:
      audit_result: audit_result
      report: report
      evidence: evidence
---

# Dependency Advisory Graph

Audit an immutable npm `package-lock.json` against the public OSV API. Every
finding is tied to an exact installed package version and an advisory returned
by OSV for that exact package/version query.

## Quality Profile

- Purpose: produce a reproducible dependency vulnerability report without
  speculative version-range matching.
- Audience: maintainers and security reviewers.
- Artifact contract: `report.md`, `evidence.json`, `verification.json`, and
  `delivery.json`, plus the sealed runx receipt.
- Evidence bar: immutable target commit, lockfile SHA-256, exact versions,
  OSV advisory IDs, advisory URLs, and independent query replay.
- Stop conditions: fail closed when the lockfile is mutable, malformed, lacks
  exact installed versions, OSV is unavailable, or verification finds an
  omitted or unsupported advisory.

Each finding includes package, installed version, advisory ID, evidence URL,
severity, fix version when OSV supplies one, confidence, advisory source, and
retrieval timestamp. A second graph step independently replays every query.

## Governance

The skill reads one public lockfile, contacts only `api.osv.dev`, and writes
only beneath its `artifacts` directory. It does not install dependencies,
execute target code, mutate the target repository, or publish an advisory.

The default scope is `direct-production`: only direct runtime dependencies
declared by the root package are audited. The scope is recorded in every
artifact so the report never implies a transitive or development audit.
