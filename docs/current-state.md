# Current State

Last updated: November 9, 2026. Update this file at every approved checkpoint.

## Checkpoint

| Item | Value |
|---|---|
| Branch | `review-and-improvement` (`main` is the release branch) |
| Latest application commit | `9f10495` Improve UI guidance and clarity across all roles |
| Previous commit | `b913819` Build student registration and grade management application |
| Automated tests | 69 passing (`manage.py test`), system check clean, no migration drift |
| Pushed to remote | No |

## Implemented workflows

Details and rules: [application-workflows.md](application-workflows.md).

- **Administrator:** approve or reject student applications (reason required to reject), create teachers,
  edit users, block and unblock accounts (reason required), create semesters, library courses and course
  offerings (Draft or Active), view any gradebook read-only, unpublish final grades and clear grades with a
  reason, archive completed offerings, semesters and courses, review the audit history.
- **Teacher:** see assigned offerings, add, edit and delete assessments, enter grades and comments in a
  batch gradebook, publish calculated final grades after enrollment closes, export the gradebook as CSV.
- **Student:** register, wait for approval, browse the catalog, enroll and withdraw within the enrollment
  window, view grades, comments and averages, view the academic transcript, export CSV and print.

## Completed UI/UX improvements (commit `9f10495`)

- **Dashboards:** an actionable "Your next step" banner for each role, clickable statistic cards, a direct
  link to pending applications with the count, a "Finals still to publish" count for teachers,
  role-specific course panels, and a fixed page title.
- **Navigation and terminology:** the admin sidebar follows the setup order (Semesters, Course library,
  Course offerings); page titles match the sidebar; each setup page explains its concept and shows a
  three-step strip; "Publish final grade" is used consistently, paired with "Unpublish".
- **Course offerings:** form sections and help text, a prerequisite checklist when a semester, library
  course or active teacher is missing, and enrollment-window and place columns in the list.
- **Confirmations:** every confirmation page lists its consequences; destructive actions use a danger
  style; success messages are action-specific.
- **Students:** catalog availability badges (You are enrolled, Open for enrollment, Enrollment full,
  Enrollment closed); the catalog semester filter excludes archived semesters; the course page explains
  enrollment state and why withdrawal is unavailable; "Not yet graded" and "Not yet published" wording,
  with an explanation of when final grades appear; next steps for pending and rejected applicants.
- **Teacher gradebook:** "Publish final grade" appears only when publication is possible, otherwise the
  actual reasons are listed; the enrollment window is shown; assessment actions are hidden with an
  explanation while finals are published; read-only views explain why.
- **Shared:** state badges with a dot and text, callouts, grouped form sections, a single-column course
  card layout on phones, and test files read as UTF-8 (the suite passes on Windows).

## Decisions already made

- Optional features are **retained**: audit history, archiving, block/unblock, clear grade, unpublish, CSV
  export and print. Removal is a separate, future decision (see the roadmap).
- **R7 deferred:** blocked users still see the generic login error; authentication behavior is unchanged.
- The catalog uses exactly four availability statuses, so an offering whose enrollment has not opened yet
  shows "Enrollment closed"; its course page states when enrollment opens.
- Confirmation buttons keep the generic "Confirm action" label.
- CSV headers changed from "Missing grades" and "Not published" to "Not yet graded" and "Not yet published".
- The Unpublish confirmation page returns 404 immediately for an enrollment outside the offering.
- Claude Code configuration follows Option A: the root `CLAUDE.md` is tracked; `.claude/` stays local.
  A shared `.claude/settings.json` is deferred.

## Known limitations

- No notifications or emails for approval, grades or finals (password reset email only).
- The gradebook scrolls horizontally on phones.
- The UI/UX commit added about seven literal background colors in `static/css/app.css` (callouts,
  confirmation summary, danger button hover, dashboard hover and attention states) instead of tokens.
- Browser verification for commit `9f10495` was layout-only: static screenshots of 20 pages at 1440 px and
  390 px rendered from the isolated test database, with no horizontal overflow. No interactive browser
  run, keyboard test or axe-core scan was performed after the UI/UX changes.

## Environment notes (development machine)

- Windows 11, Python 3.13 virtual environment in `.venv`, local PostgreSQL 18 service. README and
  `compose.yaml` target PostgreSQL 17 with Docker; Docker is not installed on this machine.
- `ruff`, `djlint` and `playwright` (from `requirements-dev.txt`) are not installed in `.venv`.
- During tests, WhiteNoise warns that `staticfiles/` does not exist; this is expected without `collectstatic`.
- Headless screenshots: full Chromium enforces a minimum width of 484 px, so true phone widths need the
  `chrome-headless-shell` build; in this environment Chromium needed `--no-sandbox`. The screenshot script
  was a temporary session file and is not in the repository.

## Stale documentation (record only; not yet rewritten)

- **docs/verification.md:** reports 46 tests (now 69) and 30 HTML templates (now 33); journey step 7 says
  "Confirm the student's final grade" (the button is now "Publish final grade"); results describe PostgreSQL
  17.11 and Docker runs; lint, format, axe and Docker checks were not repeated after commit `9f10495`.
- **docs/ui-audit.md:** line references predate the UI/UX commit (for example `static/css/app.css:35`,
  `:150`, `templates/core/form_fields.html:19` (field markup now lives in `templates/core/form_field.html`),
  `templates/grading/gradebook.html:81`); its passing results describe the October 23 interface.
- **docs/visual-design.md:** says the course index collapses "from three columns to two" (phones now use a
  single column); the component rules do not mention availability badges, callouts, setup steps, form
  sections or confirmation summaries; the stat strip is now clickable.

## Outstanding questions

1. Which optional features, if any, should be hidden or removed (audit page, archive actions, CSV, print,
   clear grade)?
2. Should R7 (an explicit message for blocked users) be implemented, given it changes authentication behavior?
3. Does the team have official TSI brand guidance and permission to use TSI visual identity?
4. May the development tools in `requirements-dev.txt` be installed for linting and browser checks?
5. Should the team standardize on Docker or on local PostgreSQL (and which major version)?
6. Who reviews the pull request to `main`, and when?
7. Should a shared `.claude/settings.json` with read-only command permissions be added later?
