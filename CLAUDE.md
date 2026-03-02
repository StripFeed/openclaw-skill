# CLAUDE.md

## Overview

OpenClaw skill for StripFeed. The skill teaches OpenClaw agents how to use the StripFeed API to fetch any URL as clean, AI-ready Markdown.

## Structure

- `stripfeed/SKILL.md` - The actual OpenClaw skill (frontmatter + instructions)
- `.github/workflows/publish.yml` - Auto-publishes to ClawHub on push to main

## Editing the Skill

- All API details must match the live StripFeed API at `https://www.stripfeed.dev/api/v1/`
- Version field in `SKILL.md` frontmatter must be bumped on every change (semver)
- The GitHub workflow extracts the version automatically from `SKILL.md`

## Git Rules

- All commits must use Claude as author:
  ```bash
  GIT_COMMITTER_NAME="Claude" GIT_COMMITTER_EMAIL="noreply@anthropic.com" git commit --author="Claude <noreply@anthropic.com>" -m "message"
  ```
- Never push without approval
