# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Overview

OpenCode is an open-source, provider-agnostic AI coding agent with both CLI/TUI and desktop/web interfaces. Built with Bun and TypeScript, it features a client/server architecture supporting multiple LLM providers.

## Development Commands

```bash
# Install dependencies (Bun 1.3+ required)
bun install

# Run dev server (runs in packages/opencode by default)
bun dev

# Run against a different directory
bun dev <directory>
bun dev .                    # Run in repo root

# Type checking across all packages
bun typecheck

# Build standalone executable
./packages/opencode/script/build.ts --single

# Run tests (must run in individual packages, not from root)
bun run --cwd packages/opencode test

# Run web UI dev server
bun run --cwd packages/app dev

# Run desktop app (requires Tauri/Rust toolchain)
bun run --cwd packages/desktop tauri dev

# Regenerate SDK after API changes
./script/generate.ts
```

## Architecture

This is a Bun monorepo using workspaces and Turborepo for build orchestration.

### Key Packages

- **packages/opencode** - Core CLI, server, and business logic
- **packages/app** - Shared SolidJS web UI components
- **packages/desktop** - Tauri native desktop wrapper
- **packages/plugin** - Plugin SDK (`@opencode-ai/plugin`)
- **packages/sdk/js** - Generated JavaScript SDK
- **packages/ui** - Reusable UI component library

### Core Module Structure (packages/opencode/src/)

- **cli/** - CLI commands and TUI (SolidJS + OpenTUI)
- **agent/** - Agent system (build, plan, general subagent)
- **server/** - Hono HTTP API server
- **provider/** - LLM provider integrations (30+ providers)
- **lsp/** - Language Server Protocol
- **mcp/** - Model Context Protocol
- **tool/** - Agent tools
- **session/** - Session management
- **config/** - Configuration handling
- **project/** - Project context

## Code Style

Follow STYLE_GUIDE.md conventions:

- Prefer `const` over `let`; use ternary operators instead of if/else assignment
- Avoid `else` statements; use early returns
- Prefer single-word variable names when descriptive
- Avoid unnecessary destructuring; use `obj.a` to preserve context
- Prefer `.catch()` over `try`/`catch`
- Avoid `any` type
- Use Bun APIs (e.g., `Bun.file()`)

## Git Workflow

- Default branch is `dev` (not main/master)
- PR titles use conventional commits: `feat:`, `fix:`, `docs:`, `chore:`, `refactor:`, `test:`
- Optional scope: `feat(app):`, `fix(desktop):`
- All PRs must reference an existing issue (`Fixes #123`)

## Notes

- Use parallel tool calls when applicable
- After modifying API/SDK code, regenerate with `./script/generate.ts`
- UI changes can be tested in `packages/app` web dev server before integration
