# Project Pulse Dashboard Implementation Plan

## Summary

Project Pulse will be a static HTML/CSS/JSON dashboard that presents a
scannable view of project status, priorities, ownership, and progress. The
dashboard will load deterministic project data from `app/project-data.json`,
render it through semantic markup in `app/index.html`, and use
`app/styles.css` for the visual system and responsive layout. It will have no
external dependencies and will run locally through a VS Code launch
configuration.

## File Assignments

| Owner | Assigned files | Scope |
| --- | --- | --- |
| Designer | `app/styles.css` | Visual direction, layout, responsive behavior, states, and accessible presentation styling. |
| Coder | `app/index.html` | Semantic document structure, dashboard rendering hooks, and client-side data loading/rendering logic if needed by the static dashboard. |
| Coder | `app/project-data.json` | Deterministic project data and the agreed JSON schema. |
| Coder | `.vscode/launch.json` | The launch configuration for opening the dashboard. |
| Orchestrator | Integration review | Confirm the Designer and Coder outputs work together, preserve existing files, and meet the end-to-end validation requirements. |

The existing `.vscode/tasks.json` must be preserved and must not be modified.
No unrelated files may be changed. No commit or push is part of this work.

## Responsibilities

### Designer

- Establish a clear visual hierarchy for the dashboard header, summary
  metrics, project list, status indicators, priority indicators, and progress
  details.
- Implement the primary layout around the `.dashboard` CSS hook and each
  project item around the `.project-card` CSS hook.
- Define reusable styles for status and priority variants without relying on
  color alone to convey meaning.
- Make the layout responsive for narrow mobile viewports, medium tablet
  widths, and wide desktop screens; avoid horizontal overflow.
- Use readable typography, sufficient spacing, visible focus states, adequate
  contrast, and touch-friendly interactive controls.
- Keep styling self-contained in `app/styles.css` with no external fonts,
  frameworks, icon packages, or network assets.
- Coordinate class names and state hooks with the Coder so the final HTML can
  use the styles without adapter or duplicate markup.

### Coder

- Build semantic, accessible markup in `app/index.html`, using appropriate
  landmarks such as `header`, `main`, `section`, `nav` where applicable, and
  meaningful headings in a logical order.
- Provide accessible names and relationships for metrics, project cards,
  controls, progress indicators, status labels, and any navigation elements.
  Do not use placeholder text or inaccessible color-only indicators.
- Use the `.dashboard` and `.project-card` hooks consistently so the Designer's
  layout and component styles apply to the rendered content.
- Keep the page usable without external services. If JavaScript is used to
  load the JSON data, handle loading and data errors visibly and preserve a
  useful page structure. Do not hide failures behind silent fallbacks.
- Create deterministic `app/project-data.json` with a documented-by-shape
  structure that is stable across runs and sufficient to populate every
  dashboard field.
- Create `.vscode/launch.json` with a configuration named `Run Project Pulse
  Dashboard`, set `cwd` to `${workspaceFolder}/app`, and use it for opening
  `index.html`.
- Avoid changing the existing `.vscode/tasks.json` or any files outside the
  assignment.

### Orchestrator integration review

- Verify the HTML class and state hooks match the CSS implementation,
  especially `.dashboard` and `.project-card`.
- Verify every data field consumed by the page exists in the deterministic JSON
  schema and that representative statuses and priorities render correctly.
- Confirm the launch configuration opens the intended dashboard from the
  assigned working directory and that `.vscode/tasks.json` is unchanged.
- Review the complete assigned-file diff for scope, accessibility, responsive
  behavior, and absence of external dependencies before validation is
  reported complete.

## Data Contract

`app/project-data.json` should use a deterministic top-level object with a
version and project collection, for example:

```json
{
  "version": 1,
  "projects": [
    {
      "id": "project-alpha",
      "name": "Project Alpha",
      "owner": "Name",
      "summary": "Short project description.",
      "status": "on-track",
      "priority": "high",
      "progress": 72,
      "dueDate": "2026-10-15",
      "updatedAt": "2026-09-15",
      "tags": ["web", "release"]
    }
  ]
}
```

The schema must remain deterministic and use explicit types: `version` is an
integer, `projects` is an array, identifiers and labels are strings,
`progress` is a numeric percentage from 0 through 100, dates use ISO
`YYYY-MM-DD` strings, and `tags` is an array of strings. Representative data
must include multiple projects and exercise the statuses `on-track`,
`at-risk`, and `blocked`, plus priorities `high`, `medium`, and `low`. Status
and priority values should be rendered as text as well as styled variants so
the meaning remains available to assistive technology and users who cannot
distinguish colors.

## Ordered Implementation Steps

1. Planner finalizes the dashboard information architecture, required fields,
   interaction assumptions, and this implementation plan.
2. Orchestrator confirms the file assignments and integration contract:
   Designer owns only `app/styles.css`; Coder owns `app/index.html`,
   `app/project-data.json`, and `.vscode/launch.json`.
3. Designer defines the visual direction and implements the responsive,
   accessible component styles and required hooks in `app/styles.css`.
4. In parallel, Coder defines the provisional data schema and representative
   records in `app/project-data.json`, then implements semantic markup and
   rendering in `app/index.html`.
5. Coder adds `.vscode/launch.json` with the exact launch name, working
   directory, and `index.html` entry point, without changing
   `.vscode/tasks.json`.
6. Orchestrator performs the integration review after both parallel work
   streams are available, resolving any schema, selector, accessibility, or
   launch-contract mismatches.
7. The integrated HTML, CSS, JSON, and launch configuration are validated
   sequentially: validate the JSON, launch/render the dashboard, inspect
   responsive states and accessibility, check the browser console, and verify
   assigned-file scope.
8. Report the completed files, validation results, remaining risks if any, and
   confirm that no commit or push was performed.

## Parallel Work Decisions

- Planner analysis, Designer direction, and the Coder's provisional data
  schema may proceed in parallel because each can establish its deliverable
  independently from the stable dashboard requirements.
- The Coder may begin semantic structure while the provisional schema is
  finalized, but the final page must consume the agreed schema rather than an
  undocumented variant.
- Final HTML/CSS integration, launch verification, and validation are
  sequential. They depend on the final class hooks, data contract, and launch
  configuration and must be reviewed together to catch cross-file defects.

## Dependencies

- No external runtime, build, or network dependencies.
- A browser or VS Code browser-capable preview is needed for rendering and
  responsive checks.
- The project must retain the existing `.vscode/tasks.json`; it is not a
  dependency to rewrite or replace.
- The dashboard should work from the assigned `app` working directory and
  should not assume a backend or API server.

## Edge Cases and Risks

- A missing, malformed, or empty JSON project collection must produce a clear,
  user-visible error or empty-state message rather than a blank dashboard or
  silent failure.
- Unknown status or priority values must not break rendering; they should have
  a readable text representation and a safe visual treatment.
- Progress values outside 0 through 100, missing dates, long project names,
  long summaries, and empty tag lists must not create invalid markup or
  overflow the layout.
- Local file restrictions can prevent `fetch` from reading JSON in some
  browsers. The implementation must use the repository's supported preview
  path or otherwise provide an explicit, visible error; it must not conceal
  the problem with fabricated data.
- Color contrast, focus visibility, heading order, keyboard navigation, and
  screen-reader naming can regress when styles are refined; these require
  explicit validation.
- The launch configuration can be syntactically valid but point at the wrong
  working directory or entry file, so its name, `cwd`, and `index.html`
  target must be checked directly.

## Validation Expectations

- Parse `app/project-data.json` with an available JSON validator and confirm
  the schema, types, deterministic ordering, representative statuses, and
  priorities.
- Launch through `Run Project Pulse Dashboard`, confirm its `cwd` is
  `${workspaceFolder}/app`, and verify that it opens `index.html`.
- Render the dashboard and confirm summary content, project cards, status
  labels, priority labels, progress values, and empty/error states where
  applicable.
- Check responsive behavior at mobile, tablet, and desktop widths, including
  no unintended horizontal scrolling and readable wrapping for long content.
- Check semantic structure, keyboard access and focus visibility, accessible
  names, heading order, progress semantics, contrast, and non-color status
  communication.
- Inspect the browser console for errors and warnings caused by the
  dashboard, including data-loading failures or invalid selectors.
- Confirm there are no external dependencies or network-required assets.
- Confirm only the assigned files were added or changed and that
  `.vscode/tasks.json` was preserved.
- Do not commit or push the implementation or this plan.

## Assumptions

- The dashboard is intentionally static and does not require authentication,
  persistence, live updates, or a backend API.
- The `app` directory and assigned files may be created if they do not yet
  exist; the plan does not authorize changes to unrelated existing files.
- The Designer and Coder will coordinate on the CSS hooks and JSON contract
  before the sequential integration review.
- ISO dates and a percentage progress value are sufficient for the initial
  dashboard; timezone-aware timestamps and localization are out of scope.
- The repository's existing VS Code setup supports the requested launch
  configuration without replacing `.vscode/tasks.json`.
