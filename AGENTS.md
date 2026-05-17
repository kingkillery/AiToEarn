# AGENTS.md

This file defines Codex `AiToEarn` the default working rules in this repository.

## Communication

- Use Simplified Chinese by default.

## Project Layout

- `project/aitoearn-backend` is the Nx + pnpm backend workspace.
- `project/aitoearn-web` is the Next.js + pnpm frontend project.
- The repository root mainly maintains README,Docker deployment docs,`docker-compose.yml` presentation assets.

## Package & Command Rules

- backend/web use `pnpm`.
- package, install/build.
- backend changes should first be validated in `project/aitoearn-backend` `pnpm nx ...` validated with, and follow `project/aitoearn-backend/CLAUDE.md`.
- web `project/aitoearn-web` ,use `pnpm run type-check` `pnpm build`.
- For docs-only changes, run at least `git diff --check`.

## Documentation Rules

- README public-facing docs include `README.md`,`README_EN.md`,`README_JA.md`;when involvinguser-visible capabilities,,OpenClaw,MCP,Relay,API Key sync all three languages.
- Docker Descriptionwhen involving, `docker compose` ,check in sync `DOCKER_DEPLOYMENT_CN.md` `DOCKER_DEPLOYMENT_EN.md`.
- README keep minimal viable rewrites,do not copy full reference docs verbatim.
- README,skill,capability reference only describe current capabilities and environment rules,do not include `dev`,,DateDescription.

## Environment Rules

- OpenClaw,MCP,Relay China regionGlobal region:`*.aitoearn.cn` China region,`*.aitoearn.ai` Global region.
- China region API Key can only be used with `aitoearn.cn` related URLs;Global region API Key can only be used with `aitoearn.ai` related URLs.environment and key mismatch will cause 401.
- MCP examples must be separated by environment `https://aitoearn.cn/api/unified/mcp` / `https://aitoearn.ai/api/unified/mcp`,SSE same for `/api/unified/sse`.
- Relay `RELAY_API_KEY` select based on source `RELAY_SERVER_URL`:China regionuse `https://aitoearn.cn/api`,Global regionuse `https://aitoearn.ai/api`.
