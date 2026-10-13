# Implementation Blueprint — Desktop User Profile

> **Readiness: READY_WITH_CHANGES.** The frame and design-system overviews were inspected. Resolve the page-title font discrepancy before visual sign-off. This is a handoff, not React implementation.

## Confirmed target and source evidence

- Target frame: `Desktop/UserProfile` (`6319:41582`), 1440 x 972.
- Sidebar: 268 x 972. Main region: 1128 x 972 with a 56 px header.
- Banner: 1128 x 85; profile summary follows; two 552 x 378 information cards are arranged horizontally with a 24 px gap.
- Design-system page inspected across its two available overview sections: Typography (`17:2232`) and Color Palette (`18:572`).
- Full system catalog evidence is in `handoff-report.md` and `design-contract.json`.

## Recommended React boundaries

These are implementation recommendations, not confirmed existing project components:

```text
features/
  user-profile/
    UserProfilePage
    ProfileSummary
    ProfileInformationCard
    PersonalInformation
    AddressDetails
shared/
  admin-layout/
    AdminShell
    Sidebar
    Header
    PageBanner
  ui/
    Avatar
    IconButton
    Button
```

The Figma frame contains actual `Sidebar` and `Header` instances. Reuse matching application/library components if they exist, but do not assert they map to the supplied Design System without project or Figma component-link evidence.

## Layout and styling approach

- Use CSS Grid for the sidebar/main shell and the two information cards; use Flexbox for the profile summary, toolbar, and compact label/value groupings.
- Keep the main content in normal document flow. Avoid translating Figma page coordinates into page-level absolute positioning.
- Preserve the captured desktop dimensions as the reference target, while allowing engineering to use responsive container sizing if approved.
- Use the named color variables evidenced in the palette: primary `#5D87FF`, secondary `#49BEFF`, success `#13DEB9`, info `#539BFF`, warning `#FFAE1F`, danger `#FA896B`, black `#2A3547`, and white `#FFFFFF`.
- Apply the documented opacity variants only when the intended variant is known; do not convert variable labels into invented hex colors.
- The exposed shadow token is a drop shadow with color `#466DAE14`, offset `(0,4)`, radius `5`, spread `0`.
- System text scale: Hero H1-H6 56/48/40/32/24/20 px Bold; Heading H1-H6 40/32/24/18/16/14 px Bold; Body XL/L/M/S/XS 20/18/16/14/12 px with Semibold and Regular variants; Caption 14/12 px Medium; Italic 18/14/12 px Bold Italic and Italic variants.
- Confirm the `User Profile` title font before final styling: the screen explicitly uses Plus Jakarta Sans SemiBold 18 px, while the system catalog is DM Sans. The design system also exposes an Inter-based `Body Small` token whose scope is unknown.

## Data and interaction boundary

Treat displayed profile values as sample content. Define typed profile data and data-loading/update services only from product/API requirements. The design does not specify API shape, field validation, edit/save/cancel flow, permissions, persistence, or error states.

Visible controls include Edit buttons, sidebar navigation/disclosures, search, theme, notification, social links, and a power/logout affordance. Their behavior is not specified; wire only behavior confirmed by product requirements.

## Accessibility and responsive checks

- Use semantic landmarks for navigation, header, and main content; maintain a sensible heading hierarchy.
- Give icon-only controls accessible names and keyboard-operable interactions.
- Preserve focus indication and ensure contrast when choosing opacity variants.
- Only desktop profile is evidenced here. Ask whether mobile is in scope before adding mobile behavior; do not invent breakpoints or layout transitions.

## Sign-off questions

1. Confirm Plus Jakarta Sans for the page title or approve DM Sans.
2. Confirm desktop-only scope or provide a mobile profile design.
3. Provide behaviors for Edit and other visible controls, or confirm this is a visual-only handoff.
