# Project Pulse Dashboard Implementation Plan

## Summary

Build a small static dashboard that helps contributors scan project ownership, status, recent activity, and priority. The repository has no existing app files or app-specific dependencies; the project brief calls for a static frontend, JSON data, and a VS Code launch configuration. Keep the implementation within the four assigned deliverable files and use the existing exercise requirements as the acceptance criteria.

The dashboard must load from `app/index.html`, display project cards using data from `app/project-data.json`, and be runnable through **Run Project Pulse Dashboard**. Serve the app over HTTP so the browser can fetch the JSON; opening the HTML directly as a `file://` URL is not a supported preview path.

## Ordered implementation steps

### 1. Agree on the design and interface contract

- **Designer:** Define information hierarchy, responsive layout, accessible status and priority treatments, and the CSS class/API contract needed by the markup. Confirm the expected visual hooks, including `.dashboard` and `.project-card`.
- **Coder:** Confirm the data fields the page will consume: `name`, `owner`, `status`, `recentActivity`, and `priority`. Include a short `summary` string as well, because the project brief asks for a contributor-friendly summary; treat it as an additional field, not a replacement for any required field.
- **Planner/Orchestrator:** Use this plan as the handoff contract; resolve any disagreement about selectors or optional fields before implementation begins.

**Files:** No dashboard files need to be changed in this step.  
**Depends on:** None.

### 2. Implement the independent presentation, data, and run-configuration work

These tasks can proceed in parallel once Step 1’s contracts are agreed:

- **Designer — `app/styles.css`:** Implement the agreed visual system, responsive layout, readable spacing and contrast, visible status/priority treatments, and card styling. Include the required `.dashboard` and `.project-card` selectors, plus `border-radius` and `box-shadow`.
- **Coder — `app/project-data.json`:** Create valid JSON with a top-level `projects` array. Give each project all five required fields and a concise `summary`. Use illustrative sample projects; do not imply that the static sample data is connected to a live source.
- **Coder — `.vscode/launch.json`:** CreaStage docs/project-pulse-plan.md.

Commit it with the message "Plan the Project Pulse dashboard".

Push the commit.
ments and the **Run Project Pulse Dashboard** configuration. Use `python3 -m http.server 5500`, set the working directory to `${workspaceFolder}/app`, and configure the ready action to open `http://localhost:%s/index.html`. Ensure the launch mechanism and readiness pattern are compatible with the chosen VS Code configuration type and the Python server’s output.

**Files:** Designer owns only `app/styles.css`; Coder owns only `app/project-data.json` and `.vscode/launch.json`.  
**Depends on:** Step 1. These three file tasks may run in parallel because the markup/data/design contracts are fixed first.

### 3. Build the dashboard markup and data rendering

- **Coder:** Create `app/index.html` with the exact page title **Project Pulse**. Reference `styles.css`, load `project-data.json`, and render visible project cards with the `project-card` class. Display each project’s name, owner, status, recent activity, priority, and summary.
- Use semantic structure and a clear heading hierarchy. Keep status and priority understandable without relying on color alone.
- Render fetched data using safe text insertion rather than treating project values as HTML.

**Files:** Coder owns `app/index.html`.  
**Depends on:** Step 1’s agreed class and field contracts, and the outputs of Step 2. This step is sequential after Step 2 to avoid markup/style or data-contract mismatch.

### 4. Integrate and validate

- **Coder:** Check that the page, stylesheet, data file, and launch configuration work together; fix issues only within the assigned files.
- **Designer:** Review the rendered experience for layout, readability, contrast, and responsive behavior; report any design defects to the Coder for correction.
- **Orchestrator:** Confirm the acceptance criteria and report outstanding limitations. Keep corrections to a file with its assigned owner; coordinate any needed handoff rather than making overlapping edits.

**Files:** Validation may cover all four deliverables. Fixes remain with their assigned owners.  
**Depends on:** Steps 2 and 3.

## File assignments

| File | Owner | Responsibility |
|---|---|---|
| `app/index.html` | Coder | Page title and semantic markup; references CSS and JSON; renders visible `project-card` elements and all required project information. |
| `app/styles.css` | Designer | Polished, responsive dashboard presentation; accessible visual hierarchy; required `.dashboard`, `.project-card`, `border-radius`, and `box-shadow` hooks/properties. |
| `app/project-data.json` | Coder | Valid JSON with top-level `projects`; every item has `name`, `owner`, `status`, `recentActivity`, and `priority`, plus a short `summary`. |
| `.vscode/launch.json` | Coder | Strict JSON launch configuration named **Run Project Pulse Dashboard**; serves from `app/` using the specified Python command and opens `index.html`. |

## Agent responsibilities

- **Planner:** Researches repository constraints and produces this plan, including file ownership, sequencing, risks, and validation expectations. Does not implement dashboard files.
- **Orchestrator:** Uses the plan to delegate bounded tasks, manages the dependencies and parallel work described here, resolves integration issues, and reports the result.
- **Designer:** Owns the visual and accessibility direction and `app/styles.css`. Coordinates CSS hooks with the Coder before implementation.
- **Coder:** Owns the HTML, JSON data, and launch configuration. Implements the rendering and run support, integrates with the Designer’s CSS contract, and validates the implementation.

## Dependencies and execution decisions

- **Must be sequential:** Agree on the interface contract first. Build `app/index.html` after the CSS hooks and data fields are fixed. Run integration validation after the deliverables exist.
- **Can run in parallel:** After Step 1, Designer can implement `app/styles.css` while Coder creates `app/project-data.json` and `.vscode/launch.json`. These tasks have separate file ownership and can use the agreed contracts without waiting for one another.
- **Must not overlap:** Designer edits only `app/styles.css`; Coder edits `app/index.html`, `app/project-data.json`, and `.vscode/launch.json`. Any ownership change should be explicitly coordinated.

## Edge cases and risks

- **JSON fetch under `file://`:** Browser restrictions can prevent loading JSON directly from a local file. Validate through the HTTP launch configuration, not by opening the HTML file directly.
- **Empty or invalid project data:** Handle an empty `projects` array with a clear empty state. Invalid JSON or a failed fetch should produce a visible, understandable error state rather than a blank dashboard.
- **Missing or unexpected values:** Do not let blank status, activity, or priority values break the card layout. Keep text legible and meaningful when content length varies.
- **Accessibility:** Status and priority should have readable text, not color-only distinctions. Preserve keyboard access, semantic headings, and sufficient contrast.
- **Launch mismatch:** A launch command can start a server but still open a directory listing if its working directory or URL is wrong. Confirm both the `app/` working directory and explicit `/index.html` URL.
- **VS Code/server readiness behavior:** The launch type and server-ready pattern must match the environment and Python server output. The repository specifies the command and URL but does not contain an existing `launch.json` example to copy.
- **Scope:** There is no package manifest or existing frontend framework. Do not add a framework, build system, or dependency unless a new requirement makes one necessary.

## Validation expectations

1. Parse `app/project-data.json` with `python3 -m json.tool`; verify the top-level `projects` value is an array and each project has the required fields.
2. Parse `.vscode/launch.json` with `python3 -m json.tool`; confirm it is strict JSON and has the required configuration name, command, `app/` working directory, and URL ending in `/index.html`.
3. Inspect `app/index.html` to confirm the exact **Project Pulse** title, stylesheet and JSON references, visible card rendering, `project-card` class, and presentation of every required field.
4. Inspect `app/styles.css` for the required selectors and styling hooks, then check the layout at narrow and wide viewport sizes.
5. Run **Run Project Pulse Dashboard** from VS Code. Confirm the browser opens the dashboard at `index.html`, not a directory listing, and the project cards load.
6. Exercise the empty-data and fetch/JSON failure states where practical; check keyboard navigation and that status/priority remain understandable without color.
7. Run `scripts/validate-exercise.sh` once the expected learner files exist. It checks required exercise artifacts and phrases, including JSON parsing and launch-configuration requirements; it does not replace a browser/runtime check.

## Open questions and assumptions

- The repository does not provide authoritative project names, owners, statuses, or activity. Assume the initial data is clearly illustrative sample content.
- The brief asks for a short summary but does not list `summary` among its required JSON keys. This plan includes `summary` as an additional string field to satisfy both requirements.
- The repository does not currently include an app framework, application dependencies, or a launch-configuration example. Assume a static implementation using browser-native HTML, CSS, and JavaScript, with the specified Python HTTP server for preview.
- The repository prescribes the server command and URL, but does not settle the exact VS Code launch type or readiness regex. The Coder should choose a compatible configuration and verify it in the target Codespace.
