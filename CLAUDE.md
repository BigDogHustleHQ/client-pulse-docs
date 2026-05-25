# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Product Context

This repo documents **Vendrr** — an AI-driven growth and operations platform for local businesses. The org and repo names use the "client-pulse" prefix, but the product is called Vendrr throughout all documentation and code.

Load the full product spec at the start of every session:

@docs/SPEC.md

## Repo Ecosystem

Vendrr spans four repos:

| Repo | Purpose |
|------|---------|
| client-pulse-frontend | Frontend application |
| client-pulse-backend | Backend services |
| client-pulse-api | API layer |
| client-pulse-docs | Documentation (this repo) |

## This Repo

- Content is Markdown files organized by topic under `docs/`
- Architecture diagrams are draw.io files under `docs/architecture/`
- No build, test, or lint commands — this is a docs-only repo
- Add new documentation as `.md` files under a clearly named subdirectory in `docs/`
