# Campus Visual Design

## Direction

Campus uses a restrained, TSI-inspired academic look: a white page, TSI dark blue for navigation, headings, links and primary actions, neutral dark body text, and one sans-serif typeface. Simplicity and readability come before decoration. The architectural arch is a custom vector mark that represents an entrance to learning; the TSI logo is not used.

Campus is a student group project, not an official TSI service, and the footer says so on every page. Brand colours follow the official [TSI Brand Book](https://tsi.lv/tsi-brand-book/); the dark blue `#002F68` also matches the official logo file. Tints use the brand book's 85/70/45/20% steps. The November 9, 2026 refresh replaced the earlier warm-paper, deep-green and serif editorial look; the paragraphs below about the October redesign are kept as history.

The redesign uses the installed `ui-ux-pro-max` skill. Its university research recommended an institutional palette, serif headings, a structured grid, and clear filtering. Its suggested portfolio layout was unsuitable for student registration and grade management, so the final layouts follow the actual workflows instead. Focused accessibility research informed table scrolling, keyboard navigation, and control labeling.

The [web-design-guidelines skill](https://github.com/vercel-labs/agent-skills/blob/main/skills/web-design-guidelines/SKILL.md) was read from its published source because it was not installed locally. The audit follows its [current interface guidelines](https://github.com/vercel-labs/web-interface-guidelines/blob/main/command.md), adapted to Django's server-rendered, English-only interface.

## Existing application and scope

The application uses Django Templates, locally hosted Bootstrap 5.3, vanilla JavaScript, PostgreSQL, and WhiteNoise. The redesign preserves that stack. No business services, view authorization, models, migrations, route definitions, form validation, or database records are changed.

| Area | Pages and interactions |
| --- | --- |
| Shared shell | Desktop sidebar, mobile drawer, account link, POST logout, messages, skip link |
| Authentication | Sign-in, registration, password recovery, reset results, password changes |
| Account | Profile editing; pending, approved, and rejected application states |
| Dashboards | Role-specific counts, course records, recent grades, direct next actions |
| Courses | Search and semester filters, catalog, enrolled and archived courses, course details, enrollment and withdrawal confirmation |
| Teacher | Assigned courses, assessments, edit/delete confirmation, batch gradebook, final publication, CSV export |
| Administrator | People and applications, approval/rejection, blocking, teacher creation, academic structure, gradebooks, correction confirmations, audit history |
| Reports | Semester transcript, filters, CSV export, print layout |
| Supporting pages | Pagination, empty states, 403, 404, standalone 500, supplementary Django Admin |

The former interface had very small low-contrast text, decorative dashboard artwork, repeated card surfaces, and inconsistent information density. The new design gives content more hierarchy and reduces those decorative elements.

## Design tokens

The shared token source is `static/css/app.css`. The supplementary administration skin uses `static/css/admin.css` without importing Bootstrap into Django Admin.

| Token | Value | Source | Purpose | Contrast |
| --- | --- | --- | --- | --- |
| `--primary` | `#002f68` | TSI dark blue | Sidebar, headings, links, primary buttons | 13.1:1 on white |
| `--primary-hover` | `#264e7f` | Dark blue 85% tint | Button hover | White text 8.5:1 |
| `--primary-soft` | `#ccd5e1` | Dark blue 20% tint | Selection, outline-button hover | Dark blue text 8.8:1 |
| `--accent` | `#0da9d3` | TSI light blue | Active menu marker and focus ring, only on dark blue | 4.75:1 on dark blue |
| `--text` | `#1f2328` | Neutral dark | Body text | 15.8:1 on white |
| `--muted` | `#56606b` | Neutral grey | Secondary text | 5.2:1 or more on every light background |
| `--control-border` | `#4d6d95` | Dark blue 70% tint | Input and outline-button borders | 5.3:1 on white |
| `--line` | `#c9d2d8` | Bluish grey 45% tint | Dividers and panel borders | Non-interactive |
| `--subtle` | `#e7ebee` | Bluish grey 20% tint | Table headers, light buttons, neutral badges | Dark text 13.2:1 |
| `--info-soft` | `#cfeef6` | Light blue 20% tint | Open/active badges, information callouts | Dark blue text 10.7:1 |
| `--success-soft` | `#d1f3e9` | Green 20% tint | Success messages | Dark text 13.3:1 |
| `--warning-soft` | `#fdf3d5` | Yellow 20% tint | Full, draft and attention states | Dark text 14.3:1 |
| `--danger` / `--danger-soft` | `#a23130` / `#f6e4df` | Functional, not a TSI colour | Errors, rejected and blocked states | 5.7:1 |

TSI light blue, green, yellow and bluish grey are never used as text or control borders on white, because they measure 2.2 to 2.9:1 there. Green and yellow appear only as decorative left borders. Muted grey is never placed on the dark blue 20% tint.

Spacing follows a 4/8-based scale: 4, 8, 12, 16, 24, 32, 48, and 64 pixels. Shared padding, margins, and gaps use the corresponding custom properties. Badges use a 4-pixel radius, buttons and inputs 6 pixels, and panels 8 pixels. Only the mobile drawer uses an overlay shadow.

DM Sans is the only typeface, for interface text, headings and numbers; it is hosted locally under `static/fonts/` with its SIL Open Font License, and the primary weight is preloaded. TSI's own typeface, Manrope, would need new font files and is not used. Body and editable-field text is 16 pixels, table text 14 pixels, table headers 13 pixels in sentence case, and no screen text is smaller than 12 pixels. Headings are semibold dark blue: 32 pixels for page titles (26 on phones) and 21 pixels for section titles. Numeric data uses tabular figures.

## Layout and component rules

- Use the dark green sidebar as the principal navigation anchor. Active destinations have both a visible rule and a selected background. Icons are consistent, decorative outline SVGs beside text labels.
- Keep the main canvas light. Separate data with thin rules and purposeful surfaces rather than shadows or gradients.
- Present dashboard totals as a single divided strip. Use only server-provided counts; do not invent graphs, activity, or trends. Cards that lead somewhere are links; a card needing attention uses the yellow tint.
- Present the course catalog as a list with code and availability status on the first line, then title, description, teacher, semester, capacity, and one navigation action. Do not add number markers: the list is not a sequence. Avoid repeated ornamental cards.
- Use small context labels above headings only when they add information, such as the role, the course code and semester, or a setup step. Write them in sentence case.
- Keep forms to a readable width. Pair labels with controls, preserve server validation, and connect help/error descriptions to fields.
- Use real tables for academic records. Allow horizontal scrolling inside the table region. Overflowing regions receive an accessible name, visible keyboard focus, and keyboard scrolling.
- Keep the gradebook's student column visible on larger screens. On small phones, release the sticky column so it does not cover editable grades.
- Show current averages and published final grades as distinct information. The course detail result panel emphasizes the student's own results.
- Use standard links, forms, and buttons. Mutations retain CSRF-protected POST and their existing confirmation workflows.
- Submission feedback appears only after native validation succeeds. The submit control shows a waiting state and prevents duplicate submissions. Browser back navigation restores the control.
- Gradebook edits show a live unsaved-changes message and retain the navigation warning.

## Responsive behavior

The sidebar becomes a Bootstrap drawer below 992 pixels. The authentication page's editorial panel disappears below 768 pixels so the form becomes the main task. Course cards place metadata beside the description on wide screens and become a single column below 768 pixels. Buttons and filters wrap naturally. Tables scroll locally without widening the document.

At wide desktop sizes, the student dashboard places recent grades beside course records. At smaller widths, the two sections stack. Dashboard totals become a two-column strip on phones. The transcript's print stylesheet removes navigation, filters, export buttons, and decorative surfaces.

## Accessibility and interaction

The implementation includes semantic page landmarks, one main heading, labeled controls, decorative icon hiding, named icon links, explicit table header scopes, a skip link, visible keyboard focus, accessible current navigation, native password-manager support, inline server errors, and a live gradebook status.

Primary actions have at least 44-pixel height. Native text inputs use 16-pixel text to prevent mobile input zoom. Nonessential transitions respect reduced-motion preferences. No animation, external image, chart library, or frontend framework was added.

Axe checks supplement visual and keyboard inspection; they do not establish complete assistive-technology compatibility. The interface deliberately retains consistent sentence-case labels and Django's configured English date formatting. React-specific hydration and client-router rules do not apply to this server-rendered application.

## Verification

Run the application with the README workflow, seed demonstration data explicitly, then run:

```sh
python scripts/browser_check.py
```

The browser check covers 1440, 1024, 768, and 375 pixel widths. It checks public authentication pages, error and empty states, all five demo account states, administrator forms, course details, gradebooks, assessment forms, and mobile navigation. It asserts successful page responses, one main heading, English document language, no document-wide horizontal overflow, and no JavaScript errors. Screenshots are saved under the ignored `test-results/` directory.

For an additional automated WCAG audit, provide a locally downloaded axe-core script:

```sh
BROWSER_AXE_SCRIPT=/absolute/path/to/axe.min.js python scripts/browser_check.py
```

No accessibility scanner is shipped to application users. Use `BROWSER_EXECUTABLE` to select an existing Chromium installation and `BROWSER_BASE_URL` to choose another running preview.

The completed findings and corrections are recorded in [Frontend Audit Results](ui-audit.md).
