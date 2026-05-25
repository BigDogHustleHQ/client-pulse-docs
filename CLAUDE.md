# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Product Context

This repo documents **Client Pulse** — an AI-driven growth and operations platform for local businesses. The org and repo names use the "client-pulse" prefix, but the product is called Client Pulse throughout all documentation and code.

Load the full product spec at the start of every session:

@docs/SPEC.md

## Repo Ecosystem

Client Pulse spans four repos:

| Repo | Purpose |
|------|---------|
| client-pulse-frontend | Frontend application |
| client-pulse-backend | Backend services |
| client-pulse-api | API layer |
| client-pulse-docs | Documentation (this repo) |

## Environment Ladder

Use these environment terms consistently across Client Pulse docs:

| Env | Where | Purpose |
|-----|-------|---------|
| local | localhost | Individual developer loop (`client-pulse-api` on `localhost:4000`, backend service on `localhost:3001`) |
| development | Railway | Shared, deployed, always-on integration target for frontend, WebSocket service, API, and Integration Hub work |
| staging | Railway | Production mirror and release-candidate gate |
| production | Railway | Live service |

Important distinction:
- `local` means a developer's own machine.
- `development` means the shared deployed Railway environment. It exists so frontend, WebSocket, API, and Integration Hub work can point at stable services without using someone's laptop or polluting staging.

## This Repo

- Content is Markdown files organized by topic under `docs/`
- Architecture diagrams are draw.io files under `docs/architecture/`
- No build, test, or lint commands — this is a docs-only repo
- Add new documentation as `.md` files under a clearly named subdirectory in `docs/`
