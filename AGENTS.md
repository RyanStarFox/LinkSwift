# AGENTS.md

This file defines repository-specific guidance for autonomous coding agents.
Follow this file over generic assumptions.

## 1) Repository Overview

- Project type: browser userscript repository.
- Primary runtime: script managers (Tampermonkey/Violentmonkey/ScriptCat).
- Main deliverables are `*.user.js` scripts and JSON config files.
- There is no package-manager workspace in this repository.
- There is no compiled build pipeline checked into this repository.

## 2) Source of Truth for Process

- `README.md`: install/usage and release-channel context.
- `README-ScriptCat.md`: platform-specific distribution notes.
- `.github/contributing.md`: contribution preference (target `dev` branch).
- `config/*.json`: runtime config schema and key layout patterns.
- `*.user.js`: executable source of behavior and coding style.

## 3) Build / Lint / Test Commands

Important: this repository currently defines no authoritative build/lint/test scripts.

### Official commands

- Build: `N/A` (no `package.json`, `Makefile`, `Cargo.toml`, `Package.swift`, or Xcode project).
- Lint: `N/A` (no ESLint/Prettier/Biome/SwiftLint config present).
- Test: `N/A` (no test framework config or test directory present).
- Single test: `N/A` (cannot run one test because no test harness exists in-tree).

### What to run instead (manual validation)

- Validate userscript header integrity by checking metadata block consistency.
- Validate config integrity by ensuring edited JSON parses and key names are unchanged.
- Validate behavior in browser with a script manager against affected provider flows.

### Manual verification checklist (recommended)

1. Load modified script in Tampermonkey/Violentmonkey/ScriptCat.
2. Open an affected target site and trigger the changed feature.
3. Confirm no console syntax errors on initial page load.
4. Confirm fallback/error UI still appears on expected failure paths.
5. Confirm existing untouched providers still initialize.

## 4) Single-Test Guidance (Explicit)

- There is no project-supported command for single-test execution.
- Do not invent `npm test`, `pnpm test`, `yarn test`, `bun test`, or `pytest` commands here.
- If tests are introduced in future, update this section with exact command and file pattern.

## 5) JavaScript Style (Userscript Code)

### Module and dependency style

- Use userscript metadata (`@require`, `@grant`, `@match`) for runtime integration.
- Do not introduce ESM/CJS imports unless repository tooling is explicitly added.
- Assume browser + userscript globals (`GM_*`, `unsafeWindow`, `$`, `Swal`) are valid.

### File structure

- Keep code wrapped in an IIFE with strict mode.
- Prefer object-literal module organization for shared behavior.
- Keep provider-specific logic namespaced (for example `$baidu`, `$aliyun` style).
- Keep shared state grouped in central objects rather than scattered globals.

### Formatting and readability

- Preserve local formatting style; existing JS is tab-indented in major script paths.
- Keep semicolon and quote style consistent with nearby lines.
- Add comments only for non-obvious logic or fragile integration points.
- Prefer concise JSDoc for complex functions and cross-provider utilities.

### Naming

- Use `camelCase` for variables, functions, and methods.
- Keep internal utility names descriptive and action-oriented.
- Preserve established prefixes and namespace patterns already used in file.
- Keep CSS/UI hook naming consistent with existing prefix conventions.

### Types and contracts

- Codebase is plain JavaScript, not TypeScript.
- Express contracts with JSDoc where useful (`@param`, `@returns`, intent notes).
- Use runtime guards (`typeof`, array checks, null checks) for external data.
- Do not introduce TS build requirements unless explicitly requested.

### Error handling

- Prefer defensive `try/catch` around brittle runtime integrations.
- Preserve graceful degradation and user-facing fallback messages.
- Avoid hard-failing entire flow when one provider request fails.
- Log contextual errors where existing code already logs, without noisy spam.

### Async and events

- Follow existing async flow style (`async/await` and Promise handling).
- Preserve delegated event patterns used for dynamic page DOM.
- Avoid blocking operations on hot UI paths.
- Keep retries/fallback behavior aligned with existing network logic.

## 6) JSON Config Style (`config/*.json`)

- Preserve schema shape and key naming; avoid opportunistic key renames.
- Keep provider blocks aligned with current structure and key ordering.
- Keep values as strings/objects consistent with neighboring entries.
- Do not remove legacy keys unless migration logic is included in script code.
- Ensure edited JSON is valid and parseable after changes.

## 7) YAML / GitHub Metadata Style

- Keep issue template structure declarative and minimal.
- Preserve indentation and list formatting used by existing `.github/ISSUE_TEMPLATE/*.yml`.
- Do not add CI workflow assumptions unless workflow files are introduced.

## 8) Markdown / Docs Style

- Keep documentation updates focused, factual, and user-actionable.
- Do not document commands that are not present in this repository.
- When adding troubleshooting steps, separate manual checks from official commands.

## 9) Contribution and Branching Notes

- Prefer opening PRs to `dev` branch (per `.github/contributing.md`).
- Keep changes scoped; avoid mixing refactors with behavioral fixes.
- When editing large script files, minimize unrelated formatting churn.

## 10) Cursor / Copilot Rules Status

At the time this file was authored, no repository rule files were found at:

- `.cursor/rules/`
- `.cursorrules`
- `.github/copilot-instructions.md`

If any of these files are later added, treat them as higher-priority agent instructions and update this document.

## 11) Safe Agent Workflow for This Repo

1. Read affected `*.user.js` and neighboring provider/config blocks before editing.
2. Preserve userscript metadata and grants unless change explicitly requires updates.
3. Make minimal, targeted changes that match local style.
4. Perform manual browser validation for impacted provider flows.
5. Report clearly that build/lint/test commands are currently not defined in-tree.

## 12) What Not to Do

- Do not scaffold build tooling as a side effect of feature work.
- Do not introduce new dependencies through imagined package scripts.
- Do not replace resilient fallback behavior with strict exceptions.
- Do not rewrite large sections solely for formatting preferences.

## 13) Future Improvements (When Requested)

If maintainers want automation, introduce it explicitly and then update this file with exact commands:

- Add lint tooling and pin an official lint command.
- Add test harness and define full + single-test commands.
- Add CI workflow so command behavior is validated in pull requests.
