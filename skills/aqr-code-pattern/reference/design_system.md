# Applying a design system

Apply an existing design system to a project - building new UI, extending features, restyling pages, or auditing design consistency. Preserve functionality and business logic while producing a coherent design rather than pixel-perfect copies.

## Purpose

Apply an existing design system to a project by building new UI, extending existing features, restyling existing pages, or auditing design consistency.

Supported actions:

- **build** - Create new pages, components, or websites.
- **extend** - Add new features while matching the design system.
- **restyle** - Migrate an existing UI toward the design system.
- **audit** - Analyze consistency and report improvements without modifying code.

## General principles

- Preserve functionality and business logic.
- Maintain accessibility and responsive behavior.
- Follow the project's existing architecture and coding conventions.
- Make shared improvements before local fixes.
- Produce a coherent design rather than pixel-perfect copies.
- When safely possible, remove duplicated styles, eliminate obsolete CSS, and consolidate repeated patterns - leave the project cleaner than before.
- When the design system lacks guidance for an area, identify the gap explicitly, make conservative decisions, and stay consistent with the existing design language.

## Workflow

Steps 1-2 prepare for any action. Steps 3-6 apply to build, extend, and restyle actions; audit action skips them. Step 7 applies to all actions.

### 1. Learn the design system

Study all available design assets.

Examples:

- DESIGN.md
- Design guidelines
- CSS/Sass/Less
- Theme files
- Design tokens
- Example HTML/CSS
- Existing component implementations

Extract:

- Color system
- Typography
- Spacing scale
- Layout principles
- Depth and elevation
- Component patterns
- Interaction patterns
- Visual rhythm
- Common page structures

Also infer implicit conventions from the example implementations.

Summarize the design language before making changes.

### 2. Inspect the target project

Identify:

- Framework
- Styling solution
- UI framework
- Existing theme mechanism
- Global styles
- Shared layouts
- Shared components
- Page structure
- Hard-coded visual values
- Technical constraints

Create an inventory of reusable layouts and components.

### 3. Choose an integration strategy

Prefer, in order:

1. Extend the existing UI framework's theme system.
2. Extend the project's existing theme/token files.
3. Introduce a centralized theme/token layer.

Never create a parallel theme system unless unavoidable.

Prefer CSS custom properties for runtime tokens unless the project already has a better-established approach.

### 4. Build a semantic mapping

Map the target project to design-system concepts.

Examples:

- page layout
- navigation
- panel
- card
- toolbar
- dialog
- button
- form
- table
- chart
- metric
- status indicator

Reuse existing design patterns whenever possible.

### 5. Token strategy

Reuse existing semantic tokens whenever appropriate.

Create new tokens only when the design system lacks the required semantic concept.

Examples where new tokens are often appropriate:

- visualization palettes
- chart series
- map layers
- specialized dashboard colors

Never introduce one-off tokens solely for individual pages.

Every new token should have:

- semantic name
- clear purpose
- reusable meaning

### 6. Implementation

Implementation differs by action:

- **Build** - compose new pages from layouts and components.
- **Extend** - add features that match surrounding patterns.
- **Restyle** - migrate existing UI toward the design system in layers.

Audit action skips implementation entirely.

#### Build

- Compose pages using existing layouts and components.
- Follow established spacing and hierarchy.
- Avoid introducing unnecessary visual patterns.

#### Extend

- Match surrounding components and page patterns.
- Integrate naturally into the existing experience.
- Prefer extending shared components over duplication.

#### Restyle

Apply changes in this order:

1. Theme
2. Global styles
3. Layouts
4. Typography
5. Shared components
6. Page-specific components
7. Minor visual refinements

Replace hard-coded values with semantic tokens gradually.

### 7. Verification

First verify textually and visually. Then, depending on the action, either report findings or fix the problems.

#### Textual verification

Inspect the code and tokens:

- Compare project CSS and tokens against the design system's tokens (names, values, usage).
- Confirm hard-coded values were replaced with semantic tokens.
- Check for duplicated or obsolete styles.

#### Visual verification

Read a screenshot of the running project:

- Capture or open a screenshot of the affected pages.
- Interact with the page to show important non-default states (populated data, open menus, expanded panels, error states, etc.) and capture those too.
- Look for bad spacing, alignment, depth, overflow, and other strange breakage.

#### Depending on the action

- Audit action: verify then report findings.
- Other actions: iterate verify and fix until clean. Prefer fixing shared rules over page-specific overrides.

## Success criteria

The resulting interface should:

- clearly belong to the same design system
- feel internally consistent
- preserve application behavior
- minimize visual clashes
- maximize reuse of shared patterns
- minimize design and CSS duplication
- remain maintainable for future development
