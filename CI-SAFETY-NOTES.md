# CI safety review

## Concern identified

The original workflow at `.github/workflows/ci.yml:10` set
`permissions: write-all`, granting write access to every available scope of the
workflow's `GITHUB_TOKEN` even though CI only checks out code, lints, and tests.

## Potential risk

If a dependency or build step is compromised during a push run with a writable
token, it can use the granted access to change repository content or tamper with
issues or pull requests. The precise impact depends on repository rules.

## Recommended improvement

Set `permissions: contents: read` as a YAML mapping at the workflow level.
This gives checkout enough access to read the code and disables all other token
scopes. This change is applied in `.github/workflows/ci.yml`.
