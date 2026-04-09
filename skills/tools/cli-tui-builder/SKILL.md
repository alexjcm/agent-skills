---
name: cli-tui-builder
description: >
  Provides best practices and library guidance for building CLI and TUI applications. Use when
  the user is building, designing, or iterating on a terminal tool, interactive CLI, TUI interface,
  or any app that runs in the terminal. Also use when the user asks about prompts, signals, exit codes,
  TTY detection, output formatting, terminal UX, project structure, or which libraries to use for a terminal app.
compatibility: "Designed for TypeScript projects. Development runtime: Node.js 20+ or Bun. Distribution runtime: Node.js 20+."
metadata:
  version: "1.0"
---

# Terminal App Builder

This skill provides implementation guidance for CLI and TUI apps with deterministic stack selection and terminal UX rules.

## Activation Signals

Use this skill when the user asks to:
- build or refactor a CLI tool
- design a terminal UX or TUI
- choose libraries for prompts, flags, spinners, colors, or process execution
- define terminal behavior (TTY detection, streams, exit codes, signals)

## Out of Scope

- browser UI work not related to terminal apps
- backend API design not related to CLI/TUI execution
- non-TypeScript-first stack recommendations unless the user requests them

## Decision Policy (Mandatory)

1. Load only the reference sections needed for the current task. Do not load all references by default.
2. Prefer the narrowest stack that satisfies the requested complexity.
3. Respect user-selected libraries when provided; otherwise recommend from `references/cli-tui-stack.md`.
4. Assume development on Node.js 20+ or Bun, and distribution on Node.js 20+, unless the user states otherwise.

## Core Rules

- Follow Unix stream conventions (`stdout` for data, `stderr` for diagnostics).
- Gate prompts and visual formatting by the correct TTY checks (`stdin` vs `stdout`).
- Do not emit interactive UI elements when output is piped/redirection is detected.
- Handle cancellation/signals and return appropriate exit codes.
- Prefer composable commands and clear flag/argument contracts.

## Reference documents

**Best practices** → [`references/cli-tui-best-practices.md`](references/cli-tui-best-practices.md)

Use for terminal behavior, UX contracts, output/error handling, signals, config conventions, and architecture boundaries.

**Library stack** → [`references/cli-tui-stack.md`](references/cli-tui-stack.md)

Use for stack selection by interface complexity, library tradeoffs, and native Node API alternatives for Node 20+ environments.

## Final Verification Checklist

- Confirm runtime constraints (dev: Node 20+ or Bun; distribution: Node 20+).
- Confirm chosen stack matches requested complexity (minimal CLI vs interactive CLI vs full TUI).
- Confirm no interactive prompts/spinners are shown when non-TTY.
- Confirm stream usage is correct (`stdout` data, `stderr` diagnostics).
- Confirm command/flag design is predictable and has `--help` support.
