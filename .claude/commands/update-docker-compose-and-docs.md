---
name: update-docker-compose-and-docs
description: Workflow command scaffold for update-docker-compose-and-docs in FastGPT.
allowed_tools: ["Bash", "Read", "Write", "Grep", "Glob"]
---

# /update-docker-compose-and-docs

Use this workflow when working on **update-docker-compose-and-docs** in `FastGPT`.

## Goal

Synchronize Docker Compose deployment files across environments and update related documentation.

## Common Files

- `deploy/docker/cn/docker-compose.*.yml`
- `deploy/docker/global/docker-compose.*.yml`
- `deploy/templates/docker-compose.*.yml`
- `deploy/args.json`
- `deploy/dev/docker-compose*.yml`
- `document/public/deploy/docker/cn/docker-compose.*.yml`

## Suggested Sequence

1. Understand the current state and failure mode before editing.
2. Make the smallest coherent change that satisfies the workflow goal.
3. Run the most relevant verification for touched files.
4. Summarize what changed and what still needs review.

## Typical Commit Signals

- Edit docker-compose YAML files under deploy/docker/cn, deploy/docker/global, and deploy/templates.
- Update corresponding copies in document/public/deploy/docker.
- Optionally update deploy/args.json and deploy/dev/docker-compose*.yml.
- Update or create documentation in document/content/docs/self-host/upgrading/* and document/data/doc-last-modified.json.

## Notes

- Treat this as a scaffold, not a hard-coded script.
- Update the command if the workflow evolves materially.