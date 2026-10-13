---
name: react-implementation
description: Implement approved design contracts in existing React projects by mapping to established components, data boundaries, and validation practices.
---

# React Implementation Skill

This skill defines reusable implementation techniques for approved design handoffs in React applications.

It does not define the implementing agent's role, approval gate, deliverables, or task scope. The React Implementer Agent owns those responsibilities.

Use repository evidence as the source of engineering conventions. Treat the approved design contract as the source of design facts. The implementation blueprint contains recommendations, not requirements.

## Source of truth and precedence

Read the relevant handoff artifacts, the user's latest instructions, and the existing codebase before implementation. Use this precedence when sources disagree:

1. The user's latest explicit instruction or approved change.
2. Confirmed requirements and evidence in the approved Design Contract and handoff report.
3. The implementation blueprint, only where it does not contradict confirmed requirements.
4. Existing repository conventions for engineering choices the design does not specify.
5. Engineering judgment, clearly identified whenever it affects visible behavior or design.

Do not silently reconcile conflicting design requirements. Identify the conflicting statements and their source, then ask a focused question if the conflict materially blocks a reliable implementation. If it does not block the approved scope, implement only the unambiguous portion and report the limitation.

Treat the handoff as evidence, not as permission to invent unspecified functionality. Do not assume it represents the latest Figma state if the user indicates the design changed after handoff.

## Core principles

- Preserve existing behavior and project conventions outside the approved scope.
- Reuse existing components, data access, API clients, and state-management tools when they fit.
- Separate domain data and transport concerns from presentation.
- Keep unknown design behavior unknown; do not invent product rules, endpoints, authentication, or persistence.
- Prefer focused domain boundaries over generic application frameworks.

## 1. Establish the project profile

Inspect the relevant project files before planning implementation:

- package manifest, scripts, dependencies, and TypeScript configuration
- app entry point, routes, feature/page boundaries, and nearby components
- styling conventions, shared tokens, and asset handling
- existing domain types, fixtures, service/API clients, hooks, and state/query libraries
- relevant tests and browser or visual validation setup

Build a concise profile from evidence. Do not introduce a new architecture merely because a blueprint recommends one.

## 2. Map the design contract to the application

For each approved screen and data requirement, identify:

- the existing page/container that owns it
- presentational components that can be reused
- existing domain data, service, hook, and query boundaries
- required additions and why existing code cannot fulfill them

Create components around meaningful visual or behavioral responsibilities, not every Figma group. Avoid duplicating an existing component to make a single screen fit; extend it only when the change preserves its established responsibility and behavior.

Use typed props to pass data from page/container components to presentational children. A child that only displays profile fields, table rows, metrics, or chart data should not independently fetch the same resource.

## 3. Preserve design fidelity

Translate confirmed design requirements using the repository's existing styling approach. Check the details that materially affect the target screen:

- dimensions, alignment, spacing, and visual hierarchy
- typography, font weight, and line height
- colors, tokens, borders, radii, shadows, and dividers
- Grid/Flex relationships, overflow, scroll regions, and sticky areas where specified
- asset choice, crop, aspect ratio, and placement

Prefer confirmed design tokens and supplied assets when available. Do not assume an asset exists just because it is named in a report; inspect the repository or supplied asset references. Do not replace a confirmed token with a visually similar value without an engineering reason.

Use normal layout flow, Grid, and Flexbox in ways consistent with the project rather than mechanically translating Figma coordinates into page-level positioning.

Do not claim pixel-perfect fidelity unless a rendered comparison was actually performed and supports that conclusion.

## 4. Keep data contracts and fixtures replaceable

When an approved screen requires data and no usable backend contract is available:

- Define a focused TypeScript domain model for the data the UI actually needs.
- Keep mock fixtures out of components. Use the repository's fixture convention; if none exists, keep fixtures in a clearly named mock-data location.
- JSON is suitable for static response-shaped fixtures. TypeScript fixtures are appropriate when the project needs executable fixture helpers or typed scenario generation. Do not add both formats without a concrete need.
- Make fake content unmistakably development/sample data. Never treat displayed Figma sample text as verified real-user or business data.
- Keep separate fixtures by resource or scenario as the app grows; do not create one monolithic fixture for the whole product.
- Do not invent business calculations or data semantics to make a visualization appear plausible.

Use a service/repository function that expresses a domain operation, such as `getCurrentUserProfile`, rather than exposing the fixture path to UI code. Keep the return type stable so its implementation can later retrieve real data without changing display components.

If the external API response differs from the UI domain model, translate the response into the domain model at the service/API boundary. Do not spread transport-specific DTO names through presentation components.

## 5. Load data at the feature boundary

The normal flow is:

```text
Page/container
    ↓
Domain hook or shared async/query hook
    ↓
Domain service/repository
    ↓
Mock fixture now, or selected API client later
    ↓
Typed data passed to presentational children
```

Follow the architecture already established in the repository. Do not create a new hook or service for each screen when multiple screens consume the same domain resource. A domain-specific hook is useful when it gives multiple consumers a meaningful shared interface; otherwise, a page may call an existing generic async hook with a service loader.

If there is no existing data-fetching library and the API technology is undecided:

- Use the smallest native Promise/`fetch` approach needed for the approved scope.
- A generic async-state hook should receive a loader function, not an endpoint URL. Keep transport and endpoint details inside the service/API layer.
- Handle loading, errors, and stale/unmounted requests according to the project's level of complexity. Do not add artificial delays or silently convert failures into successful empty data.
- Do not add speculative caching, retries, mutation frameworks, or dependency packages.

Avoid naming a fixture-backed loader `useFetch...` if it does not perform a network request; a source-neutral name such as `useAsyncData` communicates its current responsibility.

## 6. Keep authentication separate

- Reuse the existing authentication/session boundary.
- Let that boundary supply identity and authorized request context to the API layer.
- Do not decode tokens in page or presentational components.
- Do not assume token claims constitute a complete or current profile.
- Do not place sensitive profile data in tokens or fixtures intended to represent production credentials.
- Do not invent the current-user endpoint or token mechanism when they are not specified.

## 7. Choose server-state tooling from project evidence

- If the project already uses TanStack Query, Apollo, or another established query/cache library, follow its conventions.
- For a confirmed REST-oriented app with server-state needs and no existing library, TanStack Query may be considered when caching, refetching, shared queries, or mutations justify it.
- For a confirmed GraphQL app using Apollo Client, use Apollo's query/cache model for those operations.
- Do not use Apollo and TanStack Query as competing caches for the same server state.
- If the backend/API choice is undecided, do not install either library merely to anticipate a possible future migration. Keep the domain boundary small and choose after the API and application needs are known.

The goal is to keep transport/cache details out of display components, not to promise that switching REST and GraphQL requires no service-layer changes.

## 8. Implement defined interactions

Implement only behavior supported by the approved handoff or the user's explicit instructions. For each interactive control, establish from evidence:

- what triggers the interaction
- what state or navigation changes
- what result the user sees
- what validation or error behavior is required

Use semantic buttons, links, inputs, and form elements. Reuse real project routes and service abstractions where available.

Do not invent destinations, validation rules, business logic, or server behavior. Do not present a non-functional control as a completed interaction. If behavior is undocumented, ask when it blocks the implementation; otherwise implement the confirmed visual scope and report the unresolved behavior.

## 9. Apply accessibility fundamentals

Follow the project's accessibility conventions and use appropriate fundamentals:

- semantic HTML and a sensible heading hierarchy
- accessible names for interactive controls
- labels for form fields
- keyboard-operable interactions and visible focus states
- sufficient contrast where design evidence allows assessment
- meaningful alternative text for informative images and appropriate handling of decorative images/icons

Do not use clickable generic containers where a semantic control is appropriate. Do not claim WCAG compliance from visual inspection alone; run available automated checks and additional checks where the environment permits.

## 10. Handle responsive behavior honestly

Follow explicit responsive designs and breakpoint requirements when supplied.

If only a desktop design is available:

- do not invent a complete mobile design or attribute it to the designer
- keep the layout reasonably resilient at narrower widths where practical
- distinguish confirmed responsive requirements from basic layout resilience
- check overflow and narrow-width behavior when preview tools are available
- report responsive areas that remain unverified or need design approval

Do not claim mobile support is validated without checking it.

## 11. Keep changes focused

- Follow project naming, file organization, styling, and export conventions.
- Prefer small, coherent changes, existing dependencies/components, and reusable tokens for repeated values.
- Avoid unnecessary dependencies, broad refactors, competing CSS/tokens, unrequested migrations, dead code, and placeholder abstractions.
- Do not disable lint rules or tests to hide defects.
- Do not modify handoff artifacts during routine implementation unless explicitly requested.

## 12. Validate and report accurately

- Run the smallest relevant build, type-check, lint, tests, and browser checks available.
- Inspect at the approved target size when browser tooling is available; check overflow and obvious regressions.
- Verify important contract requirements directly, not through a proxy.
- Distinguish developer self-checks from independent design validation.
- Report checks that could not be run and pre-existing failures; do not claim unrun checks passed.
- Avoid unrelated changes made solely to achieve a clean full-project result.

Use the package manager indicated by the repository and preserve its lockfile. Do not install packages or change dependency versions without a clear need. Do not suppress errors, weaken tests, or overwrite user data to obtain passing checks.

If verification fails, identify whether the failure appears related to the changes or to a pre-existing project/environment issue.

## 13. Implementation self-check

Before reporting completion, confirm the applicable points:

- work stays within the approved scope and preserves project conventions
- confirmed design requirements are represented and unknown behavior was not invented
- existing components, assets, and tokens were reused where appropriate
- specified interactions and accessibility fundamentals were addressed
- available relevant checks were run and reported accurately
- limitations and unverified areas are explicit

Completion means implementation was checked to the extent supported by the environment. It does not imply that design ambiguities, backend integration, independent visual validation, or unverified responsive requirements are resolved.
