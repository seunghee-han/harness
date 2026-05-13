# Frontend Design Rules

This document defines the frontend design direction for Etcher Solution. Agents should read it before creating or changing pages, components, layouts, charts, or user-facing copy.

## Product Character

Etcher Solution is an operational semiconductor equipment analysis UI. It is not a marketing site, portfolio page, or decorative product showcase.

The interface should help operators and engineers quickly understand equipment state, inspect logs, compare trends, and act on analysis results. Prioritize clarity, speed, structure, and consistency over visual novelty.

## Core Priorities

Design screens in this order of importance:

1. Important information is visible early.
2. Current state and next available action are clear.
3. Similar workflows use similar layouts and interaction patterns.
4. Dense data remains scannable.
5. Visual styling supports the work instead of competing with it.

## Existing Patterns First

- Follow the current Next.js App Router structure under `frontend/app`.
- Reuse existing React component patterns under `frontend/components`.
- Prefer established Tailwind utilities, layout conventions, chart libraries, and UI primitives already present in the project.
- Do not introduce a new design system, theme, charting approach, or layout framework unless the task explicitly requires it.
- Keep TypeScript types explicit at public component, hook, and API boundaries.

## Language And Localization

- Use Korean for user-facing frontend copy whenever possible.
- Keep English only where it is necessary or more correct: technical terms, log field names, API names, file names, acronyms, equipment IDs, recipe names, code-like values, established semiconductor domain labels, and vendor/product names.
- Do not translate values that come from logs, backend responses, machine identifiers, paths, enums, or protocol fields.
- Keep Korean UI copy concise, direct, and operational. Prefer labels that help the user act quickly.
- Avoid promotional, self-referential, or explanatory copy such as "this page shows" or "this feature allows."

## Page Purpose

Every page should have an obvious job.

- Dashboards summarize current status, important changes, and actions.
- Log pages support finding, parsing, filtering, and inspecting raw or structured log data.
- Analysis pages support comparison, trend reading, anomaly/error understanding, and report review.
- Prediction and virtual process pages should make inputs, assumptions, and outputs easy to verify.
- Realtime pages should favor stable monitoring and legibility over decorative motion.

Do not build landing-page style screens for core application routes. The first screen should be useful immediately.

## Layout Rules

- Use clear hierarchy: title, primary status or controls, main data, secondary details.
- Prefer structured regions, spacing, alignment, and borders over decorative cards.
- Do not put cards inside cards.
- Use cards only for repeated items, modals, compact summaries, or genuinely framed tools.
- Keep main work areas full-width and operational, not like embedded previews.
- Preserve layout stability across loading, empty, error, and populated states.
- Tables, charts, grids, controls, and counters should have stable dimensions so hover states, labels, or dynamic values do not shift the layout.
- Text must fit within its parent on desktop and mobile. Reflow or shorten copy before letting it overflow.

## Data-Dense UI

- Make dense data readable through grouping, ordering, filters, sticky context, and clear labels.
- Do not hide important operational data merely to create whitespace.
- Preserve scan paths: users should be able to move from summary to detail without relearning the page.
- Use monospace text for raw logs, paths, IDs, timestamps, and code-like values where it improves alignment.
- Empty states should say what is missing and what the user can do next.
- Error states should identify the failed operation and give a practical next step when possible.

## Color Rules

Use a restrained operational palette:

- Neutral backgrounds: `#F9FAFB`, `#F3F4F6`, `#E5E7EB`, `#FFFFFF`
- Primary text: `#111827`
- Secondary text: `#374151`
- Primary emphasis blue: `#17499D`
- Supporting positive emphasis green: `#69B82E`
- Error and warning states: `#DC2626`, `#F59E0B`

Color should communicate state, priority, selection, or action. Do not use color only for atmosphere.

- Blue is for primary actions, selected states, and important links.
- Green is for positive or successful states, used sparingly.
- Red and amber are for errors, alarms, warnings, and risk states.
- Avoid screens dominated by a single hue.
- Avoid heavy gradients, glow effects, blur effects, and decorative color blobs.

## Action Design

- The most important action on a screen should be visually strongest.
- Secondary actions should be quieter but still discoverable.
- Repeated actions should appear in consistent locations and use consistent labels.
- Do not make non-clickable information look like a button.
- Use disabled, loading, and error states intentionally so the user knows whether an action is available.

## Repeated Screen Patterns

Keep recurring operational patterns consistent:

- Lists
- Detail panels
- Filters
- KPI summaries
- Settings
- Alarms
- Job or process status
- Report views
- Log viewers
- Chart panels

When adding a new screen that matches one of these patterns, follow the nearest existing implementation before inventing a new structure.

## Charts And Monitoring

- Charts should prioritize readable axes, legends, units, timestamps, and selected ranges.
- Use chart colors consistently across related views.
- Do not overload a chart with too many series if filtering or grouping would communicate better.
- Realtime views should remain stable while data updates. Avoid animation that makes values harder to read.
- When data is unavailable, show a clear Korean empty state rather than a blank chart.

## Forbidden Patterns

Avoid these unless the user explicitly asks for them:

- Marketing-style hero sections for application routes.
- Decorative gradients, orbs, bokeh, glow, blur, and heavy shadows.
- One-off page-specific visual systems.
- Large ornamental illustrations that compete with operational data.
- Overly spacious layouts that make engineers scroll for core information.
- UI copy that explains the interface instead of labeling the work.
- New libraries for simple layout or styling problems already handled by the stack.

## AI Agent Checklist

Before finishing a frontend change, confirm:

- The screen still feels like an operational tool.
- Korean is used for user-facing copy except where English is necessary.
- Existing project patterns were followed.
- Loading, empty, error, and populated states remain stable.
- Important information and actions are visible without visual clutter.
- Colors communicate state or priority, not decoration.
- The smallest useful frontend verification was run, or the reason it was not run is stated.
