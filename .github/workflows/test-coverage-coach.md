---
description: Daily test coverage improvement workflow that adds focused unit tests for under-tested solution files.

on:
  schedule: daily
  workflow_dispatch:

network:
  allowed:
    - defaults
    - dotnet
    - node
    - python

permissions: read-all

tracker-id: test-coverage-coach-daily

tools:
  github:
    toolsets: [default]
    min-integrity: none
  bash: true

safe-outputs:
  create-pull-request:
    expires: 2d
    title-prefix: "[test-coverage] "
    allowed-files:
      - Solutions/CSharp/**/*Test*.cs
      - Solutions/JavaScript/**/*.test.js
      - Solutions/Python/**/test_*.py
      - Solutions/Python/**/*_test.py
    protected-files: allowed
    github-token: ${{ secrets.GH_AW_CI_TRIGGER_TOKEN }}
    github-token-for-extra-empty-commit: ${{ secrets.GH_AW_CI_TRIGGER_TOKEN }}

timeout-minutes: 45
engine: copilot
---

# Daily Test Coverage Coach

You are a test coverage improvement agent for `${{ github.repository }}`.

## Goal

Each day, improve test coverage in a small, safe, reviewable batch by adding tests for under-tested files in `Solutions/`.

## Hard Constraints

1. Analyze the codebase and select **exactly 3 target source files** with missing or inadequate tests whenever at least 3 candidates exist.
2. Add tests for **at most 3 files per run** (never exceed 3).
3. Keep changes focused on test additions only.
4. Follow existing test conventions in this repository.
5. Open a pull request with your test changes using the `create-pull-request` safe output.

## Repository Test Conventions

- **C#**: tests are typically in `Solutions/CSharp/*Test*.cs` with custom assertion/test-runner style.
- **JavaScript**: tests are typically `*.test.js`, often near the related adventure solution.
- **Python**: tests typically use `test_*.py` naming.

Preserve local naming, formatting, and assertion style used near the files you modify.

## Workflow

1. Discover candidate source files under:
   - `Solutions/CSharp/**/*.cs`
   - `Solutions/JavaScript/**/*.js` (excluding existing `*.test.js`)
   - `Solutions/Python/**/*.py` (excluding existing `test_*.py` and `*_test.py`)
2. Determine where tests are missing or clearly inadequate.
3. Select up to 3 files total (prefer 3; if fewer valid candidates exist, use fewer and explain why).
4. Add meaningful unit tests for those targets.
5. Validate changes with relevant existing commands.
6. Create a PR via safe output.

## Selection Guidance

- Prefer one file per language (C#, JavaScript, Python) when practical.
- Prioritize files with meaningful logic and weak or no test coverage.
- Avoid very large refactors; keep PRs manageable and narrowly scoped.

## Validation Commands

Run applicable validations after adding tests:

- `dotnet build Solutions/CSharp/CopilotAdventures.sln`
- `dotnet run --project Solutions/CSharp/CopilotAdventures.csproj mythos-test`
- `for f in Solutions/JavaScript/*.js; do node --check "$f"; done`
- `node Solutions/JavaScript/The-Gridlock-Arena-of-Mythos/The-Gridlock-Arena-of-Mythos.test.js`
- `python -m py_compile Solutions/Python/*.py`
- `python Solutions/Python/test_gridlock_arena.py`

If you add new language-specific tests beyond existing ones, run those too using existing project conventions.

## Pull Request Requirements

- Keep title concise and test-focused.
- In the PR body include:
  - The 3 selected target files
  - Why each needed better coverage
  - A short summary of tests added
  - Validation commands and outcomes

If no safe, meaningful test additions are possible, do not force changes; clearly report why.
