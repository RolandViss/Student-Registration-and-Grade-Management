# Development Roadmap

Ordered from safest to most impactful. Each phase is approved separately, committed separately and keeps
the test suite green. Current status is tracked in [current-state.md](current-state.md).

## Phase 0: Project context and safety (in progress)

- Root `CLAUDE.md`, `docs/current-state.md` and this roadmap; tracked root `CLAUDE.md`, local `.claude/`.
- Later, optional: a shared `.claude/settings.json` that allows only read-only Git commands and the three
  verification commands, with no personal paths or secrets.

## Phase 1: Documentation maintenance

- Update `docs/verification.md`: test count, template count, the "Publish final grade" wording, the current
  environment, and which checks were or were not repeated.
- Update `docs/visual-design.md`: phone course-card layout, new components (availability badges,
  callouts, setup steps, form sections, confirmation summaries) and clickable statistics.
- Refresh or archive `docs/ui-audit.md` line references after the next audit.
- Done when every statement in `docs/` matches the current code and verified results.

## Phase 2: Development tooling and browser testing

- With approval, install `requirements-dev.txt` into `.venv` (ruff, djlint, Playwright) and run the lint
  and format checks listed in the README; fix only formatting findings in a separate commit.
- Add a repeatable visual snapshot script to `scripts/` that renders pages from the isolated test database
  (never the development database) and captures desktop and true phone widths.
- Run `scripts/browser_check.py` against a seeded database that is safe to modify, never the team's
  working data.
- Done when lint, format and browser checks run from documented commands on any team machine.

## Phase 3: TSI-inspired visual design

- Prerequisite: official TSI brand guidance and permission from the institution or course supervisor.
  Use only verified colors, typography and logo files; do not copy assets from websites without permission.
- Do not present Campus as an official TSI system; keep the Campus name unless the team is told otherwise.
- Implement the palette only through the CSS tokens in `static/css/app.css`; first convert the seven
  literal colors added in commit `9f10495` into tokens.
- Check WCAG 2.2 AA contrast for every text, badge, focus and button combination after any palette change.
- Done when the restyle passes the full test suite, visual snapshots and contrast checks, with no business
  logic changes.

## Phase 4: Accessibility and responsive testing

- Run axe-core through `scripts/browser_check.py` (`BROWSER_AXE_SCRIPT`) at 1440, 1024, 768 and 390 px.
- Manual keyboard pass for every role: skip link, menu drawer, forms, gradebook, confirmations.
- Screen-reader spot checks of the gradebook, catalog badges and confirmation pages.
- Improve the phone gradebook (horizontal scrolling today) only if testing shows it is a real problem.
- Done when results are recorded in `docs/verification.md` with dates and tool versions.

## Phase 5: Future feature decisions (each needs explicit approval)

- Optional feature removal or hiding, based on the earlier impact analysis: CSV export and print button,
  audit history page, archive actions, clear grade. No migrations; services kept where other code depends
  on them (for example `archive_academic` in `seed_demo`).
- R7: an explicit message for blocked users (authentication change; security review required).
- More specific confirmation button labels instead of "Confirm action".
- A distinct catalog status for offerings whose enrollment has not opened yet.

## Phase 6: Team review and release preparation

- Run the `security-review` and `code-review` skills on the branch diff against `main`.
- Teammate walkthrough using the demo in [application-workflows.md](application-workflows.md).
- Push `review-and-improvement` and open a pull request to `main` only with team approval.
- Before any public hosting, follow the README checklist: unique secrets, demo accounts removed,
  `DEBUG=False`, exact hosts, HTTPS settings and `check --deploy`.
