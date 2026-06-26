# AGENTS.md — C# Dev Container

This repo provides the **development environment** (Docker/Devcontainer config), not a C# application. There are no test, lint, build, or format commands to run.

## Structure

- `.devcontainer/Dockerfile.dev` — .NET 10.0 SDK (Ubuntu Noble base) with Docker CLI, buildx, `just`, `dotnet-ef`, PostgreSQL dev libs
- `.devcontainer/docker-compose.yml` — mounts repo to `/home/developer/app`, binds Docker socket, VS Code extensions volume
- `.devcontainer/docker-compose.override.yml` — host UID/GID/DOCKER_GROUP_ID overrides (tracked in git)
- `.devcontainer/devcontainer.json` — VS Code config: service `app`, workspace `/home/developer/app`, remote user `developer`
- `global.json` — SDK `10.0.300`, `rollForward: latestPatch`

## Key facts

- **Workspace in container:** `/home/developer/app`
- **Pre-installed tools:** `dotnet-ef` (global tool), `just` (1.51.0), Docker CLI + buildx
- **UID/GID mismatch** is the most common build failure — set `UID`, `GID`, `DOCKER_GROUP_ID` in `docker-compose.override.yml` or environment to match the host
- **Base image is Ubuntu Noble** (not Debian) — the Docker repository URL must use `linux/ubuntu`, not `linux/debian`
- **User conflict on UID 1000** — the base image already has an `ubuntu` user with UID 1000; the Dockerfile removes it before creating `developer`
- **No `.gitignore`** — `docker-compose.override.yml` is intentionally tracked
- **No test/lint scripts exist** — the consuming app defines those
- **All devcontainer files are under `.devcontainer/`** — the project root stays clean for the consuming application
