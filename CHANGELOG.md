# Changelog

All notable changes to the Internship Acquisition System.

## [0.1.0] — 2026-09-13

### Phase 0 — Project Safety and State

- Inspected workspace: empty directory at `c:\Users\Lenovo\Desktop\intern`
- No existing `PROJECT_STATE.md` found
- No pre-existing user files to protect
- Created `PROJECT_STATE.md`
- Created `CHANGELOG.md`
- Created `.gitignore`
- Initialized Git repository

### Phase 1 — Environment Audit

- Detected: Windows 11 (NT 10.0.26200.0), PowerShell 5.1, Git 2.53.0, Python 3.11.9, Node.js v24.15.0, npm 11.12.1, Docker 29.7.2
- Noted: PowerShell ExecutionPolicy is `Restricted` (blocks npm/pnpm .ps1 scripts without per-session bypass)
- Noted: No sqlite3 CLI, no GitHub CLI, no pipx, no uv, no yarn
- Noted: Antigravity active (ANTIGRAVITY_CSRF_TOKEN present)
- Noted: No existing MCP configuration

### Phase 2 — career-ops Verification

- Repository: `career-ops-hq/career-ops` (https://github.com/career-ops-hq/career-ops)
- Stars: 71.4k | Forks: 13.5k | Commits: 1,942
- License: MIT
- Version: 1.32.0
- Last updated: 6 hours ago (actively maintained)
- Supported CLIs: Claude Code, Codex, OpenCode, Antigravity, Qwen, Kimi, Copilot, Grok
- Installation: `npx @santifer/career-ops init`
- Language: JavaScript (Node.js ≥ 18)
- Dependencies: @google/generative-ai, dotenv, js-yaml, playwright
- openings-mcp: NOT required (career-ops has built-in scanning)

### Phase 3 — Installation

- Installed via `npx @santifer/career-ops init` at tagged release `career-ops-v1.32.0`
- Cloned to `career-ops/` subdirectory
- npm install initially failed during scaffolder run (recovered manually)
- npm install retry: 6 packages, 0 vulnerabilities
- Playwright Chromium: Chrome 151.0.7922.34 (191.8 MiB)
- Playwright FFmpeg and Chrome Headless Shell also downloaded
- 7 CLI skill entrypoints bootstrapped

### Phase 4 — Smoke Test

- `node doctor.mjs`: PASSED (5 warnings for pending user setup)
  - ⚠ Playwright MCP tools not detected
  - ⚠ cv.md not found
  - ⚠ config/profile.yml not found
  - ⚠ modes/_profile.md not found
  - ⚠ portals.yml not found
- `node verify-pipeline.mjs`: PASSED (fresh setup — no applications.md yet)

### Phase 5 — Architecture Discovery

- Documented full architecture: inputs → processing → outputs → state
- Identified 43 mode files + 22 language translation directories
- Identified all user-facing configuration points
- Mapped Antigravity skill entrypoint at `.antigravitycli/skills/career-ops/SKILL.md`

### Phase 7 — Git Safety

- Initialized Git repository at `c:\Users\Lenovo\Desktop\intern`
- Created `.gitignore` excluding secrets, credentials, PII, IDE artifacts
- career-ops subdirectory has its own comprehensive `.gitignore`
