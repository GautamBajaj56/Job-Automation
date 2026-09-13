# Changelog

All notable changes to the Internship Acquisition System.

## [0.2.0] — 2026-09-13

### Real Candidate Profile Configuration

- Performed Git privacy check. Verified that `cv.md`, `config/profile.yml`, `modes/_profile.md`, and `portals.yml` are correctly ignored and will not be pushed to the public repository.
- Replaced the TEST CANDIDATE data with the real factual candidate information using the provided master resume.
- Generated the Master CV (`cv.md`) containing actual NSUT education, SoCTeamup Internship, and real full-stack/AI projects without hallucinating or fabricating information.
- Configured the candidate profile (`config/profile.yml`) to target "Full-Stack Developer Intern / Software Engineer Intern" roles with location preferences of Delhi/NCR and Remote, specifically seeking Paid internships.
- Drafted the primary targeting narrative (`modes/_profile.md`) to evaluate MERN/Full-stack/Node.js signals highly, and Python/AI signals as secondary differentiators. Unpaid roles are marked as a hard block.
- Ran local verification (`doctor.mjs`, `cv-sync-check.mjs`, `verify-pipeline.mjs`) and confirmed consistency. No PII was exposed or committed.
- Executed the first real AI job evaluation on the Anthropic "AI Operations Engineer, Partnerships" test job to measure baseline scoring accuracy. System correctly identified the geo-mismatch and experience gap, scoring it 1.5/5 (Fail).

## [0.3.0] — 2026-09-13

### Initial Target Company Universe Built

- Built an initial dataset (`data/target-companies.json`) of 51 target technology companies with engineering footprints in India (Delhi/NCR) or Remote.
- Utilized `career-ops` ATS discovery tools (`discover-ats.mjs`) to scan the public APIs of Greenhouse, Lever, Ashby, SmartRecruiters, and Pinpoint to determine which companies currently have active ATS systems.
- Successfully resolved 27 companies to scannable ATS boards with active open roles, while 24 were identified as valid but currently listing 0 open roles.
- Replaced the Anthropic technical test target in `portals.yml` with a curated set of the top resolved targets to prepare for automated job scanning. Historical Anthropic evaluation is preserved in `reports/001-anthropic-2026-09-13.md`.

## [0.3.3] — 2026-09-13

### Discovery Filter Calibration (Geography & Substring Bugfixes)

- **Identified Root Cause:** The `intern` title filter behaved as a raw substring, catching full-time roles with `Internal` or `International`. Further, the scan lacked a `location_filter`, resulting in high noise from US/UK/Canada internship programs.
- **Title Filter Fix:** Replaced `"intern"` with anchored `"word:intern"` and `"word:interns"` in `portals.yml`'s positive title filters, utilizing career-ops's native prefix syntax for whole-word boundary matching.
- **Geography Filter Fix:** Implemented a targeted `location_filter` in `portals.yml`, allowing `[Delhi, Noida, Gurgaon, India, Bengaluru, Remote]` and hard-blocking non-relevant global hubs `[United States, UK, Canada, Australia, Europe, San Francisco, New York]`.
- **Before vs After Testing:** 
  - *Before:* 1,408 raw jobs → 1,319 filtered by title → 0 filtered by location → 89 matches.
  - *After:* 1,408 raw jobs → 1,329 filtered by title → 52 filtered by location → 27 matches.
  - *Result:* False positives like "Internal Systems" and "Internal Audit" were successfully removed (10 additional title rejections). US/UK internships from Coinbase, Vercel, and Postman were cleanly rejected (52 location rejections). Correctly captured valid matches like "Paytm - HR Payroll-Intern - Noida" and "Stripe - Software Engineer, Intern - Bengaluru".

## [0.3.2] — 2026-09-13

### First Real Internship Discovery Scan

- Executed the first live automated job discovery scan (`career-ops scan`) against the top 13 verified target companies configured in `portals.yml`.
- Scanned 1,408 active job listings, applying positive/negative title filters to isolate "intern/fresher" roles, resulting in 81 new matches added to the pipeline.
- Conducted an AI evaluation of the most promising software engineering intern roles from Vercel, Coinbase, Stripe, and Postman.
- **Discovery Failure Identified:** 100% of the shortlisted roles were classified as Tier C (Reject) due to severe geographic incompatibility (e.g., US-only, Hybrid-SF, London) and false positive title matches (e.g., "Internal Systems" matching the "intern" keyword). 0 matching internships for a Delhi/NCR student were found.

## [0.3.1] — 2026-09-13

### Target Universe Verification & Ranking

- Performed a deep verification pass over the initial 51-company dataset (`career-ops/data/target-companies.json`), ranking targets 0-100 on internship evidence, full-stack relevance, geography, current hiring, and application accessibility.
- Successfully stratified the dataset into Tiers A (11), B (11), C (23), and D (6), eliminating strictly senior freelance networks (Toptal, Turing, Andela, Deel) as false positive D-tier targets.
- Identified 24 highly relevant companies with direct Delhi/NCR presence (Paytm, Zomato, Innovaccer, Cars24, MakeMyTrip, etc.) and explicitly flagged India-compatible remote targets.
- Updated `portals.yml` to contain only the top 13 highest-confidence operational scan targets (Tier A and top Tier B) that currently expose active ATS boards, reducing scanning noise while preserving the full universe in JSON.

## [0.1.4] — 2026-09-13

### Safe, Read-Only Direct ATS Discovery Test

- Configured a single technical test target: Anthropic (via Greenhouse public ATS) in `portals.yml`.
- Ran `node scan.mjs` to retrieve live job listings using a read-only WebFetch scan.
- Successfully retrieved 111 job listings without requiring the Playwright MCP or any authentication.
- Extracted a full job description using the CLI extractor (`browser-extract.mjs`) to verify data quality.
- Confirmed that title, company, location, and full description (including compensation details) are successfully retrieved and suitable for AI scoring.
- Verified zero unintended actions: no applications submitted, no credentials created, no emails sent.

## [0.1.3] — 2026-09-13

### Browser Capability Audit

- Investigated `career-ops` Playwright MCP dependency and Antigravity's capabilities.
- Confirmed Playwright MCP is optional and unnecessary for this environment.
- `career-ops` supports a native CLI extractor (`browser-extract.mjs`) for headless scraping.
- PDF generation uses the local `playwright` node module directly, not the MCP server.
- Antigravity already natively provides the `browser_subagent` capability for broad browsing tasks.
- Documented that the remaining `doctor.mjs` warning can be safely ignored.

## [0.1.2] — 2026-09-13

### Configuration Pipeline Validation

- Initialized required career-ops configuration files with sanitized test data.
- Created `config/profile.yml` (TEST CANDIDATE profile).
- Created `cv.md` (TEST CANDIDATE resume).
- Created `modes/_profile.md` (test targeting narrative).
- Created `portals.yml` (test portal config).
- Ran `doctor.mjs`: successfully validated all configuration files. 4 missing file warnings resolved.
- Ran `cv-sync-check.mjs`: successfully verified profile schema and CV structure match.
- Profile loading test passed without external API calls or real PII leakage.

## [0.1.1] — 2026-09-13

### Git/GitHub Synchronization

- Configured remote: `origin` → `https://github.com/GautamBajaj56/Job-Automation.git`
- Added `career-ops/` as git submodule (upstream: `career-ops-hq/career-ops` @ commit `e58eb65`)
- Created `.gitmodules` for submodule tracking
- Pushed 2 commits to `origin/main` with upstream tracking configured
- Verified: branch up to date with `origin/main`, no credential issues

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
