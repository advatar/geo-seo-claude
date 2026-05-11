# Code Review: geo-seo-claude

Review date: 2026-05-11
Tracker: https://github.com/advatar/Tracker/issues/53
Scope: top-level app folder `geo-seo-claude` and nested project manifests under this folder, excluding generated dependency/build directories such as `.git`, `node_modules`, `target`, `.build`, `dist`, and virtual environments.

## Executive Summary

- Overall risk from this sweep: **Medium**
- Findings by severity: High 0, Medium 3, Low 0
- Source footprint: 14 source files by extension scan (Python 7, HTML 4, Shell 3)
- Test footprint: 0 test-like files detected
- CI footprint: 0 GitHub Actions workflow files detected
- Git posture: clean before review generation
- Pattern scan budget used: 46 text/source files scanned

## Architecture Snapshot

Detected project and build surfaces:
- `requirements.txt`

Nested manifest owners sampled:
- `.`

Package scripts sampled:
- No JavaScript package scripts detected.

Local instruction/status files:
- No nested AGENTS.md or STATUS.md files detected.

## Findings

### 1. [Medium] Broad CORS/debug or insecure transport settings need environment gating

Wildcard CORS, debug flags, or disabled TLS verification should be mechanically limited to local/dev environments. Scanner count: 1.

Evidence:
- scripts/webapp/app.py:214 `app.run(debug=True, port=5050)`
### 2. [Medium] No GitHub Actions workflows were detected

There is no visible CI safety net for build/test verification. Add at least one workflow that installs dependencies and runs the canonical build and test commands.

Evidence:
- No `.github/workflows/*.yml` or `.yaml` files found.
### 3. [Medium] No test files were detected in the reviewed source tree

The app has source/project manifests but no obvious unit, integration, or UI tests after pruning generated directories.

Evidence:
- No common test directory or test/spec filename matched.

## Testing and Build Posture

Detected tests:
- No representative tests detected.

Detected CI workflows:
- No GitHub Actions workflows detected.

Inferred verification commands to standardize:
- No canonical build command could be inferred from the scanned manifests.

## Review Limitations

- This was a broad static review across many local apps, not a full manual product walkthrough.
- Generated directories and dependency trees were pruned so findings focus on app-owned source.
- Secret-like values were not reproduced; examples are redacted or limited to path/line evidence.
- Pattern scanning is capped per app to keep the cross-repository sweep tractable; high-risk folders need focused follow-up review.

## Recommended Next Steps

1. Resolve every High finding first, especially secret material, tracked generated output, and dynamic execution paths.
2. Add or tighten the app's canonical CI workflow so build and tests run on every push.
3. Convert inferred build/test commands into documented commands in the app README or STATUS file.
4. Add smoke tests around app launch, persistence, API boundaries, and security-sensitive adapters.
5. Re-run this review after cleanup and replace this file with a human-reviewed release checklist.
