---
name: figma-inspection
description: Inspect Figma UI designs and related design-system sources using Figma MCP, extract evidence-backed design information, identify ambiguities and conflicts, and prepare structured inputs for a design-to-engineering handoff. Use when analyzing a Figma screen before implementation.
---

# Figma Inspection Skill

This skill defines HOW to perform a reliable Figma inspection.

It does not define the role of the agent.

The Figma Handoff Agent uses this skill to inspect Figma and prepare handoff information.

---

# Inspection Principles

The inspection must be:

- evidence-based
- traceable to source information
- explicit about uncertainty
- separated from engineering recommendations
- free from invented business logic
- free from invented responsive behavior
- free from invented data
- suitable for downstream engineering automation

Never silently turn an assumption into a confirmed design fact.

---

# Inspection Sequence

Perform inspection in this order:

## 1. Establish sources

Record:

- UI Design
- Style Guide if supplied
- Design System if supplied
- additional sources if supplied

For each source, record its URL or reference, intended role, and whether it is:

- accessible
- inaccessible
- partially accessible
- not supplied

Treat the UI Design as the primary evidence for the screens. Use the Style Guide for documented design rules and the Design System for reusable components, variants, tokens, and usage guidance. Do not assume that a UI element uses a library component merely because it looks similar.

---

## 2. Determine scope and inventory screens

Use the supplied UI Design URL to determine the inspection scope:

- If it targets a specific screen/frame, inspect that frame and relevant descendants; do not expand to unrelated frames unless requested.
- If it targets a page or canvas, inventory its top-level frames and sections first, then identify the relevant screens before detailed inspection.
- Do not treat nested layers, decorative groups, or design-system component specimens as independent screens.
- If the user names frames or workflows, use those as the scope. Otherwise, inspect all relevant screens discoverable in the supplied page/canvas.

For page-level scope, group related screens into candidate workflows using explicit names, prototype links, annotations, and visible state/variant evidence. Mark a grouping as estimated when the relationship is inferred rather than explicitly documented. Do not invent business flow or transitions from visual similarity alone.

For each relevant screen, record:

- Figma file and page
- frame name and node ID
- dimensions
- visible purpose
- workflow grouping, if applicable, and its certainty
- platform/variant and state, when evidenced
- inspection status

Use a provided frame node as the primary target when present. Do not assume a frame is primary solely because it appears first.

---

## 3. Establish the primary screen

Identify the screen or screens that anchor the requested scope. For each, record the frame name, node ID, dimensions, visible purpose, and available variants. When the scope is a page/canvas, do not select one frame as the sole primary screen before completing the relevant frame inventory.

---

## 4. Inspect hierarchy

Inspect relevant:

- parent nodes
- child nodes
- instances
- components
- variants
- groups
- repeated structures

Do not dump the entire Figma node tree unless it provides meaningful implementation evidence.

Focus on the information required for handoff.

---

## 5. Inspect layout

Extract, where available:

- widths
- heights
- positions
- gaps
- padding
- alignment
- layout mode
- constraints
- auto-layout information
- wrapping
- sizing behavior

Distinguish:

Confirmed:
direct property evidence

Estimated:
visual approximation

Unknown:
not exposed

Do not reproduce Figma's coordinate system as the React layout architecture.

---

## 6. Inspect typography

Capture:

- family
- size
- weight
- line height
- letter spacing
- color
- opacity

Prefer explicit style information over visual inference.

---

## 7. Inspect visual tokens

Look for:

- variables
- named styles
- colors
- semantic tokens
- spacing tokens
- radius tokens
- typography tokens
- shadows

If these do not exist, report that they are not exposed.

Do not invent a token system.

---

## 8. Inspect components

For component instances determine:

- component identity
- variant
- properties
- states
- repeated usage
- library/source information where available

Do not claim a component comes from a particular library unless the source confirms it.

---

## 9. Inspect assets

Identify:

- icons
- logos
- images
- illustrations
- avatars
- SVGs
- library assets

Record source information only when available.

Do not invent npm packages or file names.

---

## 10. Inspect charts

For each chart determine:

- visual type
- series
- labels
- legend
- axes
- grid
- tooltip
- visible values
- colors
- interaction evidence
- underlying data availability

Separate:

visual evidence

from

data semantics.

Never infer a mathematical/business calculation from visual chart geometry alone.

---

## 11. Inspect states and interactions

Look for explicit states:

- default
- hover
- focus
- pressed
- active
- selected
- disabled
- loading
- error
- empty
- success
- expanded
- collapsed

Record missing states when their absence could cause engineering ambiguity.

---

## 12. Inspect responsive evidence

Look for:

- mobile frames
- tablet frames
- desktop frames
- component responsive variants
- breakpoint information
- layout transformations

If no evidence exists:

mark it Unknown.

Never invent breakpoints.

---

## 13. Cross-check supplied sources

When a Style Guide or Design System is supplied:

- Inventory the relevant pages, sections, components, or tokens in that source before drawing conclusions from it.
- Cross-check applicable rules, tokens, components, and variants against each in-scope UI screen.
- Record the specific source node/page and the UI screen or component affected.
- Distinguish a confirmed mapping (such as an instance linked to a component) from a visual resemblance or other estimated mapping.
- Record material conflicts instead of silently choosing a value. Apply source precedence only when the source relationship and applicability are explicit.
- State which parts of each source were inspected and which were not inspected, inaccessible, or not relevant to the requested scope. Do not imply that a whole file or design system was reviewed when only selected sections were examined.

---

## 14. Track coverage and partial access

Maintain an inspection record for each in-scope screen and each supplied supporting source. Use clear statuses such as inspected, partially inspected, not inspected, inaccessible, or out of scope, and give a short reason for any status other than inspected.

When Figma calls fail, return incomplete data, or access limits prevent further inspection:

- Report the actual failure or limitation without guessing its cause.
- Preserve already collected evidence and identify the affected source, node, or property.
- Mark unavailable facts Unknown and distinguish uninspected content from inspected content.
- Continue with the accessible evidence when it can still support a useful handoff.
- Do not claim complete page, workflow, variant, or design-system coverage if any relevant portion remains uninspected.

---

## 15. Record ambiguities and questions

For each unresolved ambiguity that could affect implementation, record the evidence, certainty, affected screen/component, and likely implementation impact. Capture a focused question or requested design clarification. Do not resolve missing behavior, workflow transitions, responsive rules, data semantics, or source mappings by assumption.

Inspection should continue with available evidence when possible; ambiguity alone is not a reason to abandon the inspection. The handoff agent is responsible for deciding readiness and presenting questions in its deliverables.

---

# Evidence Model

Use exactly three certainty values:

- confirmed
- estimated
- unknown

Every important fact should be traceable to a source.

Recommended evidence shape:

```text
{
  sourceType:
  sourceUrl:
  figmaFileKey:
  nodeId:
  evidenceType:
  certainty:
  note:
}