---
name: github-actions-docs
description: Use when users ask how to write, explain, customize, migrate, secure, or troubleshoot GitHub Actions workflows, workflow syntax, triggers, matrices, runners, reusable workflows, artifacts, caching, secrets, OIDC, deployments, custom actions, or Actions Runner Controller, especially when they need official GitHub documentation, exact links, or docs-grounded YAML guidance.
---

> Vendored from xixu-me/skills (skills/github-actions-docs). Source: https://github.com/xixu-me/skills

GitHub Actions questions are easy to answer from stale memory. Use this skill to ground answers in official GitHub documentation and return the closest authoritative page instead of generic CI/CD advice.

## When to Use

Use for: Actions concepts/terminology; workflow YAML, triggers, jobs, matrices, concurrency, variables, contexts, expressions; runners (hosted/larger/self-hosted) and Actions Runner Controller; artifacts, caches, reusable workflows, templates, custom actions; secrets, `GITHUB_TOKEN`, OIDC, attestations, secure patterns; environments and deployment protection; migrating from Jenkins/CircleCI/GitLab CI/Travis/Azure Pipelines; troubleshooting that needs docs/syntax references.

Do not use for: a specific failing PR check or CI failure triage (use gh-fix-ci); general PR/branch/repo ops (use github); CodeQL/code scanning (use codeql); Dependabot config (use dependabot).

## Workflow

1. **Classify** the request into a bucket (getting started, authoring/syntax, runners, security, deployments, custom actions, monitoring/troubleshooting, migration). If you need a starting point, load `references/topic-map.md`.
2. **Search official GitHub docs first** — treat `docs.github.com` as source of truth, prefer pages under https://docs.github.com/en/actions. Search the user's exact terms plus a focused Actions phrase.
3. **Open the best page before answering** — read the most relevant page/section. If a page seems renamed/moved, say so and return the nearest authoritative pages instead of guessing.
4. **Answer with docs-grounded guidance** — direct answer first, exact docs links (not just the homepage), YAML/steps only if asked or necessary, and make any inference explicit ("According to GitHub docs, ..." / "Inference: ...").

## Answer Shape

1. Direct answer  2. Relevant docs  3. Example YAML/steps (only if needed)  4. Explicit inference callout (only if connecting multiple pages). Keep citations close to the claim.

## Common Mistakes

- Answering from memory without verifying current docs
- Linking the Actions landing page when a narrower page exists
- Mixing up reusable workflows and composite actions
- Suggesting long-lived cloud credentials when OIDC is the better path
- Treating repo-specific CI debugging as a docs question

## Bundled Reference

Read `references/topic-map.md` only as a compact index of likely doc entry points. It is intentionally incomplete and should never replace the live GitHub docs as the final authority.
