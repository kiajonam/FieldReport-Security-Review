# Dependency Security Review Request

## Problem to review

A dependency audit of the FieldReport backend reported vulnerabilities involving transitive dependencies. The packages mentioned during the initial investigation included `@vitest/mocker`, `deepmerge-ts`, and `mysql2`. The exact raw audit JSON/text and advisory IDs still need to be attached to this review so the findings can be independently reproduced.

The main concern is how to remediate vulnerable dependencies safely without accidentally breaking the backend test suite, build, or unrelated application behavior.

## What is visible in this snapshot

- The backend manifest declares `vitest: ^3.2.4` as a development dependency.
- The lockfile resolves Vitest to `3.2.7` in this snapshot.
- The lockfile includes entries for `@vitest/mocker`, `deepmerge-ts`, and `mysql2`; their presence alone does not establish exploitability or whether they are included in a production deployment.
- CI installs dependencies with `npm ci`, generates Prisma Client, runs backend tests, and builds the backend. The displayed CI workflow does not run `npm audit`, OSV-Scanner, or Trivy.

## Prior isolated test result (context from the project owner)

The owner reports that a separate upgrade experiment completed installation, Prisma generation, backend build, and 468 backend tests successfully, with no vulnerabilities reported by the audit after the upgrade. This was an isolated worktree/experiment, not proof that the change is already present on `main`, nor proof that all security risks are eliminated. The exact upgrade diff, package-lock, audit output, and CI run should be checked before relying on this result.

## Questions for the reviewer

1. Which exact advisories affect the locked versions, and what are their severity, affected version ranges, and fixed versions?
2. Are the vulnerable packages reachable only through development/test tooling, or can they enter a production installation/build? Please explain the evidence rather than inferring from the dependency name.
3. Is a Vitest major upgrade necessary, or can a narrower compatible update resolve the advisories?
4. What peer-dependency, Node.js, Vite, TypeScript, and test-runner compatibility issues should be checked?
5. What verification would you require before accepting the upgrade (clean install, full tests, build, audit, lockfile diff, CI, and relevant E2E tests)?
6. Would you recommend OSV-Scanner or Trivy for additional coverage, and which exact commands/configuration would be appropriate here?

## Requested output

Please provide a concise review with:
- confirmed findings versus assumptions,
- recommended minimal version changes,
- commands to reproduce the audit,
- a safe upgrade and rollback strategy,
- any additional files or logs genuinely needed.

## Safety boundaries

This repository is a public, limited review snapshot. Do not assume it contains the whole project. Do not request secrets, `.env` files, production database access, or the full private repository unless a specific need is justified. Do not recommend `npm audit fix --force` without first explaining the proposed breaking changes and their impact.
