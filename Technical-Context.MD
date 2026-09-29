# Technical Context

Engineering-practice contract for code-writing agents in this project. Every implementation must respect what's written here. If a constraint conflicts with the user's request, flag the conflict — don't silently bypass it.

## Overriding Principles
1. **Electron security baseline is always on.** Every `BrowserWindow` runs with `contextIsolation: true`, `nodeIntegration: false`, `sandbox: true` and a strict Content-Security-Policy; the renderer reaches Node and the OS only through a typed `preload` IPC bridge (`contextBridge`). A renderer with Node access turns any injected script into full code execution on the user's machine.
2. **Secrets never live in source or in the bundle.** No API key, token or credential is committed, hard-coded, or shipped in plain text in the packaged app; any secret the app needs is stored in the Windows credential store via Electron `safeStorage`. Anything in the bundle is readable by anyone who installs it.
3. **All network calls happen in the main process, behind one provider adapter.** Weather data is fetched only by the main-process weather-provider adapter and handed to the renderer over IPC; the renderer never fetches an external URL, and the CSP forbids it. One egress point keeps the CSP tight and the provider swappable.
4. **No breaking change without an ADR.** A change to an Overriding Principle, the weather provider, or the IPC contract between main and renderer lands with an ADR in `docs/adr/` in the same PR. These are the decisions every later Story builds on.

## Frameworks
- **Electron** (exact version pinned in `package.json` at scaffold) — desktop shell; main process owns window lifecycle, IPC handlers, network and file access.
- **TypeScript** (exact-pinned, `strict: true`) — the only language for main, preload and renderer code.
- **React** (exact-pinned) — renderer UI.
- **electron-vite** (on Vite, exact-pinned) — builds the main, preload and renderer bundles and runs the dev server.
- **Node.js LTS** (pinned via `.nvmrc` + `engines`) — build and test runtime.

## 3rd-party tech
- **Open-Meteo** — weather forecast and geocoding (location search) API; keyless free tier, called only from the main-process provider adapter. The free tier is licensed for non-commercial use; commercial distribution requires their paid, keyed plan (Principle 2 then applies).
- **Azure DevOps Boards** (`EnateInternal` / `TestWeatherApp3`) — work tracking; the Factory's tracker (`.factory.yml`).
- **GitHub** (`crdev317/TestWeatherApp3`) — source hosting and pull requests.

## Packages in use
**HTTP:** native `fetch` (main process only) — no axios/got/node-fetch.
**Validation:** `zod` — every Open-Meteo response and every IPC payload is parsed through a schema before use.
**State:** React built-in state and context — no Redux/Zustand/MobX.
**Styling:** CSS Modules — no CSS-in-JS or utility-CSS framework.
**Logging:** `electron-log`.
**Testing:** `vitest`, `@testing-library/react`, `msw` (recorded-replay HTTP fixtures), `@playwright/test` with its Electron support (app-level tests).
**Packaging:** `electron-builder` (NSIS Windows installer).
**Lint/format:** `eslint`, `prettier`.

## Toolchain pinning
- Policy: **exact-pinned.**
- Pinned tools: Node.js via `.nvmrc` and the `engines` field; `electron`, `typescript`, `vite`, `electron-vite`, `electron-builder` and every other dependency at exact versions (no `^`/`~`) in `package.json`; `package-lock.json` committed; installs use `npm ci`, never `npm install`, in CI and in agent sessions.
- Bumping: a version bump is its own PR that changes only the pinned version(s) and the lockfile, and is green on the full Tier-1 suite and a packaged Windows build before merge. An Electron major bump also re-checks the Principle-1 security settings against that release's defaults.

## Instrumentation
- `electron-log` in the main process writes a rotating log file to `%APPDATA%\TestWeatherApp3\logs\`, plus the console in development. Renderer logs are forwarded to the main process over IPC; the renderer never writes files.
- Levels: `error` — a failure the user can see or an unhandled exception; `warn` — a degraded but recovered path (a retry, stale data shown); `info` — lifecycle events and provider calls; `debug` — development only, off in packaged builds.
- Always logged: app start with version and Electron version; every provider call (endpoint, HTTP status, latency — no query coordinates); every failed call or schema-validation failure; every unhandled exception and unhandled rejection in main and renderer.
- Never logged: the user's precise location (coordinates or searched place names above city level) or any secret.
- No remote telemetry, crash reporting or analytics.

## User Feedback Approach
- Channel: in-app only — inline status and error messages in the main window. No OS notifications, no tray icon.
- Tone: short, plain, friendly English; no jargon, error codes or stack traces.
- Every error message says what happened and what the user can do next, e.g. "Can't reach the weather service — showing data from 10:42. We'll try again in a minute."
- When data is stale or cached, the UI always shows when it was last updated.

## Branching and Environments
- Model: trunk-based with a per-Feature integration buffer (full rules in `CONTRIBUTING.md`).
- Branches: `main`; `feature/<feature-id>-<slug>` per Feature; `story/<issue#>-<slug>` per Story, cut from its Feature branch (or from `main` for parentless work, `bug/…` for Bugs).
- Environments: local development (`electron-vite dev`); CI (Azure DevOps pipeline reporting the `ci` check — not yet wired); packaged Windows installer built by `electron-builder`. No distribution/release channel exists yet.
- Protected: `main` — PR-only, squash-merge, linear history, no force-push, no deletion.
- Platform: Windows 10/11 only.

## Git Guardrails
- Never force-push, `git reset --hard`, `git clean -fd`, or `git branch -D` without explicit human confirmation.
- Never commit or push directly to `main`; it moves only through a PR.
- Never bypass the `.githooks/pre-commit` hook (`--no-verify`) without explicit human confirmation; enable it once per clone with `git config core.hooksPath .githooks`. It blocks scratch/diff dump files from being committed.
- To install once the Node project is scaffolded: Claude Code git-guardrail hooks (`/factory-git-guardrails`) and Husky + lint-staged pre-commit (`/factory-setup-pre-commit`).
- **HITL delivery opens and closes its own board state — nothing else will.** No skill in the HITL pipeline (`/factory-to-stories`, `/factory-check-security-design`, `/factory-triage`) sets an assignee, an iteration, or an in-progress state at creation — they deliberately leave a HITL-tagged Story, Bug, or wholly-HITL Feature untouched for a human to clear. So: **before** making any change while hand-delivering one, set `System.AssignedTo` to yourself, `System.IterationPath` to the current sprint, and `System.State` to `Active`. **When** the delivering PR merges and the work item is done, set `System.State` to `Resolved`/`Closed` yourself before ending the session. Skipping either half leaves the item unassigned, out of sprint, and looking untouched for the entire time it's actually being worked.

## Testing & the ratchet
> Seeded from the Factory standard — **opt-out, not opt-in**. Trim a line only with a deliberate, recorded decision; don't start from nothing. Full standard: <https://github.com/enateltd/factory-skills/blob/main/docs/testing-and-the-ratchet-standard.md> · the *why*: <https://github.com/enateltd/factory-skills/blob/main/docs/agentic-e2e-testing-principles.md>.

- **Test the contract, not the LLM.** Assert on the deterministic envelope (output shape, state transition, side-effect ordering), never on generated text or chain-of-thought.
- **Every seam gets a real-IO test on at least one side.** Mock-on-both-sides only proves the mocks agree; defects live at integration seams, not inside modules.
- **Three tiers, each catching a class the others can't.** Tier 1 — recorded-replay (real local I/O, fakes at the seams, replayed LLM fixtures; $0, every commit); Tier 2 — live-LLM, disposable (real subprocess against a per-run throwaway substrate; bounded cost, scheduled); Tier 3 — live (real production substrate and subprocess; continuous).
  - In this project: Tier 1 is Vitest + `msw` replaying recorded Open-Meteo responses; Tier 2 is Playwright driving the built Electron app against live Open-Meteo with a throwaway user-data directory; Tier 3 is the packaged installer on a real Windows machine.
- **Platform matrix is non-negotiable for OS-touching code.** Filesystem/subprocess code runs Tier 1 + Tier 2 on every OS it supports, not just the dev box's. For this project the matrix is Windows only.
- **Plan test coverage at the Feature level, not the Story level.** Each Feature declares at kickoff which tiers it needs, which fixtures/substrates must exist, and a Tier-2 cost ceiling.
- **The ratchet — every Tier-3 defect pins a cheap-tier regression.** A live defect's fix lands with a Tier-1 (or Tier-2) regression that would have caught it, encoding the **class** of defect **bounded by the seam** where it surfaced — not the exact reproducer, and not a project-wide sweep. A Story that surfaced from a live defect carries that ratcheting fixture as an acceptance criterion.
- **Disposable substrate, not shared.** Tier-2 substrate is created per run and torn down; reproducibility comes from a seed (tarball/manifest/script), never persistent state.
- **Assert on observable behaviour, not internal state.** For Tier 2/3 the target is what an operator could verify from external artefacts — tracker state, commit shape, ordered log events, posted comments.
