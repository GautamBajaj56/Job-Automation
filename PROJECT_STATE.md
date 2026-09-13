# Project

**Paid Full-Stack Internship Acquisition System**

# Objective

Get a PAID Full-Stack Developer internship in Delhi/NCR or remotely, while minimizing manual effort in discovering, filtering, preparing, applying, contacting, and tracking opportunities.

# Current Phase

**Phase 0 — career-ops installation and audit** (COMPLETE)

# Environment

| Component | Version / Status |
|---|---|
| OS | Windows 11 (NT 10.0.26200.0) |
| Shell | PowerShell 5.1.26100.9444 |
| Working Directory | `c:\Users\Lenovo\Desktop\intern` |
| Git | 2.53.0.windows.2 |
| Python | 3.11.9 |
| Node.js | v24.15.0 |
| npm | 11.12.1 |
| pip | 26.2.1 |
| Docker | 29.7.2 |
| SQLite CLI | NOT installed (sqlite3 not on PATH) |
| GitHub CLI | NOT installed |
| pipx | NOT installed |
| uv | NOT installed |
| yarn | NOT installed |
| pnpm | installed but blocked by ExecutionPolicy |
| Playwright Chromium | Installed (v1234, Chrome 151.0.7922.34) |
| Claude Code | NOT detected on PATH |
| Antigravity | Active (ANTIGRAVITY_CSRF_TOKEN env var present) |

### PowerShell Note
Default execution policy is `Restricted`. Must use `Set-ExecutionPolicy -Scope Process -ExecutionPolicy Bypass` per session to run npm/pnpm .ps1 scripts.

### Existing MCP Configuration
- No MCP configuration detected in Claude Desktop config, Claude settings, or Cursor config.
- Claude settings file exists at `~\.claude\settings.json` but contains only `autoUpdatesChannel` and `theme`.

### Environment Variables Detected (names only)
- `ANTIGRAVITY_CSRF_TOKEN`
- `APPDATA`
- `GOPATH`
- `HOMEDRIVE` / `HOMEPATH`
- `LOCALAPPDATA`
- `PATH` / `PATHEXT`
- `PSModulePath`
- `PT8HOME`
- `VBOX_MSI_INSTALL_PATH`
- `VSCODE_CODE_CACHE_PATH`

# Architecture

## Overview

career-ops is the selected orchestration layer. It is NOT a standalone app — it is a collection of Markdown prompt files (modes), Node.js scripts, and HTML templates that run inside an AI coding CLI (Claude Code, Antigravity, Codex, etc.).

## Architecture Diagram

```
INPUTS                           PROCESSING                        OUTPUTS                    STATE/TRACKING
─────────────────────           ───────────────────────            ──────────────────────     ─────────────────────
Job URL / JD text         →     modes/oferta.md (A-H eval)   →    reports/NNN-company.md     data/applications.md
cv.md (user CV)           →     modes/pdf.md (CV tailoring)  →    output/*.pdf (ATS CVs)     data/pipeline.md
config/profile.yml        →     modes/scan.md (portal scan)  →    output/*.html              portals.yml
modes/_profile.md         →     modes/cover.md (cover letter)→    jds/*.md (saved JDs)
portals.yml               →     modes/email.md (draft email) →    interview-prep/
                          →     modes/apply.md (prep app)    →    batch/logs/
                          →     modes/interview-prep.md      →
                          →     doctor.mjs (health check)    →
                          →     verify-pipeline.mjs          →
```

### System vs User Layer (DATA_CONTRACT.md)
- **System layer**: `modes/`, `*.mjs` scripts, templates, dashboard — versioned, updated by `update-system.mjs`
- **User layer**: `cv.md`, `config/profile.yml`, `modes/_profile.md`, `data/`, `reports/`, `jds/` — NEVER touched by updater

### Key Design Principles
1. **Local-first**: Everything runs on your machine. No server required.
2. **AI-agnostic**: Works with Claude Code, Antigravity, Codex, OpenCode, Qwen, Kimi, Grok, Copilot.
3. **Human-in-the-loop**: Tool evaluates and recommends. User decides and acts. NEVER auto-submits.
4. **Files are canonical**: Human-readable markdown files are the permanent source of truth. SQLite is only a derived index.

# Files

## GitHub Repository

**https://github.com/GautamBajaj56/Job-Automation.git**

- Remote: `origin`
- Branch: `main`
- career-ops tracked as git submodule (upstream: `career-ops-hq/career-ops`)

## Workspace Root (`c:\Users\Lenovo\Desktop\intern\`)
| File | Purpose |
|---|---|
| `.gitignore` | Excludes secrets, PII, IDE artifacts |
| `.gitmodules` | Git submodule configuration (career-ops) |
| `PROJECT_STATE.md` | This file — persistent project state |
| `CHANGELOG.md` | Change history |
| `career-ops/` | Cloned career-ops v1.32.0 (git submodule) |

## career-ops Key Directories
| Directory | Purpose |
|---|---|
| `modes/` | Markdown prompt files (43 modes + 22 language dirs) |
| `config/` | Profile config templates |
| `data/` | Application data, pipeline state |
| `reports/` | Evaluation reports |
| `output/` | Generated PDFs, HTML |
| `jds/` | Saved job descriptions |
| `templates/` | Portal configs, CV templates |
| `batch/` | Batch processing logs/state |
| `scripts/` | Helper scripts |
| `lib/` | Shared library modules |
| `plugins/` | Plugin integrations (Gmail, Notion, Apify, H1B) |
| `dashboard/` | Go-based terminal UI dashboard |
| `interview-prep/` | Interview story bank |
| `.antigravitycli/skills/career-ops/` | Antigravity CLI skill entrypoint |

# Dependencies

## Installed
| Package | Version | Purpose |
|---|---|---|
| `@google/generative-ai` | ^0.24.1 | Gemini API integration |
| `dotenv` | ^17.0.0 | Environment variable loading |
| `js-yaml` | ^5.3.0 | YAML config parsing |
| `playwright` | 1.62.1 | Browser automation for PDF generation and portal scanning |

## NOT Installed (by design)
- JobSpy / jobspy-mcp-server
- openings-mcp
- Reactive Resume
- browser-use
- Apollo
- Any custom scrapers

# Configuration

## Created / Available
- `config/profile.example.yml` — Template for user profile
- `modes/_profile.template.md` — Template for targeting narrative
- `templates/portals.example.yml` — Template for portal configuration
- `.env.example` — Template for API keys
- `config/profile.yml` — TEST DATA profile
- `modes/_profile.md` — TEST DATA targeting narrative
- `cv.md` — TEST DATA resume
- `portals.yml` — TEST DATA portals

*Note: All current profiles and CVs use fictional test data ("TEST CANDIDATE") purely for configuration pipeline validation. No real PII is stored.*

## NOT Configured
- No `.env` file created (no API keys entered)

# External Services

## Required (but NOT yet configured)
- AI provider (Anthropic/Claude via Antigravity, or Gemini, or OpenRouter)

## Optional (NOT configured)
- Gemini API (free tier available)
- OpenRouter API (free tier available)
- Gmail plugin (for email drafting — NOT auto-sending)
- Notion plugin (for syncing to Notion)
- Apify plugin (for additional job source scraping)

## Technical Test Endpoints (Safe/Read-only)
- Greenhouse API (`https://boards-api.greenhouse.io/v1/boards/anthropic/jobs`) — Verified for read-only ATS discovery test. No credentials used.

# Implemented Features

Features verified by smoke test on 2026-09-13:

- [x] career-ops installation via `npx @santifer/career-ops init`
- [x] Dependencies installed (`npm install` — 6 packages, 0 vulnerabilities)
- [x] Playwright Chromium installed (Chrome 151.0.7922.34)
- [x] `doctor.mjs` runs successfully (5 expected warnings for user setup)
- [x] `verify-pipeline.mjs` runs successfully (reports fresh setup, no applications.md yet)
- [x] Antigravity CLI skill entrypoint exists at `.antigravitycli/skills/career-ops/SKILL.md`
- [x] Project structure created with all expected directories
- [x] `data/pipeline.md` auto-created
- [x] Git repository initialized at workspace root
- [x] Master CV configured (`cv.md` - REAL CANDIDATE)
- [x] Target profile established (Paid Full-Stack/Software Engineer Intern, Delhi/Remote)
- [x] Real candidate profile configured (`config/profile.yml` - REAL CANDIDATE)
- [x] Real targeting narrative configured (`modes/_profile.md` - REAL CANDIDATE)
- [x] Privacy and Git safety checks validated (real PII is ignored)
- [x] Configuration validated locally (`doctor.mjs` & `cv-sync-check.mjs` passed)

- [x] First real AI Job Evaluation test completed (Anthropic test job)

- [x] REAL portal configuration (`portals.yml` verified and filtered)
- [ ] API key configuration (`.env`)
- [ ] Delhi/NCR location preferences
- [ ] Internship-specific scoring rules
- [ ] Job discovery / portal scanning
- [ ] Batch evaluation
- [ ] PDF CV generation
- [ ] Cover letter generation
- [ ] Email drafting
- [ ] Application tracking (beyond auto-created pipeline.md)
- [ ] Interview preparation
- [ ] JobSpy / openings-mcp integration
- [ ] Custom job source scrapers
- [ ] Browser automation (browser-use)
- [ ] Email automation
- [ ] Dashboard/analytics

# Known Issues

1. **PowerShell ExecutionPolicy**: Default policy is `Restricted`, blocking npm/pnpm .ps1 scripts. Workaround: `Set-ExecutionPolicy -Scope Process -ExecutionPolicy Bypass` per session.
2. **Playwright MCP not configured**: `doctor.mjs` warns that Playwright MCP tools are not detected. **This warning can be safely ignored.** `career-ops` provides a CLI extractor alternative (`browser-extract.mjs`) that uses the local Playwright installation. PDF generation also uses the local Playwright installation directly. Furthermore, Antigravity natively provides browser interaction capabilities.
3. **sqlite3 not on PATH**: No SQLite CLI available. Not critical — career-ops uses files as canonical store and SQLite only as derived index.
4. **GitHub CLI not installed**: Cannot use `gh` commands. Not blocking for current phase.
5. **pnpm blocked**: pnpm installed but blocked by ExecutionPolicy. Not needed — npm works.

# Security / Privacy Considerations

1. **No API keys stored**: No `.env` file created. `.env` is in `.gitignore`.
2. **No PII entered**: No real resume, personal information, or contact details entered.
3. **career-ops is local-first**: All data processed locally. AI interactions go to chosen provider.
4. **LEGAL_DISCLAIMER.md**: career-ops explicitly disclaims liability. Users must verify AI outputs.
5. **Platform ToS**: Users must comply with job board ToS. career-ops recommends against mass applications.
6. **NEVER auto-submits**: career-ops evaluates and prepares. User manually submits everything.
7. **Git safety**: `.gitignore` excludes `cv.md`, `config/profile.yml`, `.env`, PII patterns, credentials.

# Important Decisions

1. **career-ops v1.32.0 selected as core orchestration layer** — 71.4k GitHub stars, MIT license, actively maintained (updated 6 hours ago), explicitly supports Antigravity CLI.
2. **No custom job discovery added yet** — career-ops has built-in scanning for Greenhouse, Ashby, Lever, Wellfound + HN + InterAMT + funded company discovery.
3. **No outreach automation added** — Email drafting is supported but career-ops NEVER sends emails.
4. **No application auto-submit** — Human-in-the-loop is enforced by design.
5. **openings-mcp is NOT required** — career-ops has its own scanning infrastructure. openings-mcp would be an optional additional source.
6. **Installation method**: Used official scaffolder (`npx @santifer/career-ops init`) at tagged release v1.32.0.
7. **Workspace structure**: career-ops installed as subdirectory `career-ops/` within `c:\Users\Lenovo\Desktop\intern\`.

# Next Tasks

1. **[NEXT] Create minimal test profile and CV** — Copy example configs, create placeholder `cv.md` and `config/profile.yml` with test data (not real PII). Verify the configuration initializes properly.
2. Configure MCP server for Playwright (needed for portal scanning).
3. Create real CV and profile (requires user's actual information).
4. Configure Delhi/NCR location preferences and internship-level targeting.
5. Set up API keys (Gemini free tier or Anthropic via Antigravity).
6. Run first portal scan test.
7. Evaluate integration points for JobSpy / openings-mcp.

# Change Log

| Date | Phase | Action | Details |
|---|---|---|---|
| 2026-09-13 | Phase 0 | Created | `PROJECT_STATE.md` — project state tracking |
| 2026-09-13 | Phase 0 | Created | `CHANGELOG.md` — change history |
| 2026-09-13 | Phase 0 | Created | `.gitignore` — root-level git exclusions |
| 2026-09-13 | Phase 0 | Initialized | Git repository at workspace root |
| 2026-09-13 | Phase 2 | Verified | career-ops repository (career-ops-hq/career-ops, v1.32.0, MIT, 71.4k stars) |
| 2026-09-13 | Phase 3 | Installed | career-ops via `npx @santifer/career-ops init` |
| 2026-09-13 | Phase 3 | Installed | npm dependencies (6 packages, 0 vulnerabilities) |
| 2026-09-13 | Phase 3 | Installed | Playwright Chromium (Chrome 151.0.7922.34) |
| 2026-09-13 | Phase 4 | Verified | `doctor.mjs` — passed (5 warnings for pending user setup) |
| 2026-09-13 | Phase 4 | Verified | `verify-pipeline.mjs` — passed (fresh setup, no applications.md) |
| 2026-09-13 | Phase 5 | Documented | Architecture map, file structure, capabilities |
| 2026-09-13 | Git sync | Configured | Remote origin → https://github.com/GautamBajaj56/Job-Automation.git |
| 2026-09-13 | Git sync | Added | career-ops as git submodule (upstream: career-ops-hq/career-ops @ e58eb65) |
| 2026-09-13 | Git sync | Pushed | 2 commits to origin/main (branch set to track upstream) |
| 2026-09-13 | Config Valid | Configured | Initialized cv.md, profile.yml, _profile.md, portals.yml with TEST DATA |
| 2026-09-13 | Config Valid | Verified | doctor.mjs passes with 1 warning (MCP missing). Profile loads correctly. |
| 2026-09-13 | Audit | Decision | Playwright MCP is unnecessary. CLI extractor and Antigravity native browser will be used instead. |
