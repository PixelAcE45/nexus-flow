# Nexus: fluid interaction + motion polish

This project is currently the blank starter, so step 1 brings your `nexus-os-core` code in from GitHub as-is. Steps 2+ are the animation pass only — no layout, color, type, component, or feature changes.

## 1. Import the existing Nexus code
Copy the repo snapshot (routes, `src/components/nexus/*`, all `ui/` components, `styles.css`, assets, config, dependencies) into this project unchanged, and verify it builds and renders exactly as it does today. Nothing visual changes in this step.

## 2. Motion foundation (styles.css)
Add one set of motion tokens next to the existing glass tokens — no color or spacing edits:
- Easing: a quick-response curve for entrances, a standard curve for state changes; durations 140/200/280/360ms.
- Keyframes for spatial page transitions: enter (opacity 0 to 1, translateY 12px to 0, scale 0.985 to 1) and exit (mirrored, moving away).
- Keep the existing `prefers-reduced-motion` block authoritative so everything degrades to instant.
- Motion uses `transform`/`opacity`/`filter` only, with `will-change` on the few animating containers.

## 3. Tab / page switching (app-shell `<main>`)
Replace the current `key={pathname}` instant swap with a short crossfade in place:
- Outgoing view fades and drifts back (scale 0.99, small Y) while incoming fades and rises into position, overlapped so it reads as one continuous move.
- Around 260ms total: quick response, smooth settle, no cinematic slide.
- Sidebar, top bar, tab bar and the glass shell never move or re-mount — only the main content region animates.
- Scroll resets without a visible jump.

## 4. Navigation indicator
Sidebar `NavList` and the mobile tab bar get a travelling indicator:
- One absolutely positioned glass pill per nav container that measures the active item and animates position/size via transform, so it slides from the previous tab to the selected one.
- Active/inactive icon and label colors cross-fade instead of snapping; the icon gets a very small scale settle.
- Hover: subtle background and opacity shift. Press: ~0.98 scale with a quick release.
- Existing `data-[status=active]` styling stays as the fallback for reduced motion and first paint.

## 5. Glass surfaces
Refine the existing `glass-hover` utility rather than adding new looks:
- Hover: ~1px lift, slight brightness increase, marginally brighter border, softer deeper shadow, ~200ms.
- Press: scale 0.985, fast in, smooth return.
- Applies to glass cards, search trigger, profile card, icon tiles. No bounce, no glow.

## 6. Modals, sheets, dropdowns, command palette
Tune animation on the existing Radix/vaul primitives (dialog, sheet, dropdown-menu, popover, select, command palette, quick-create dialog):
- Overlay: dim and backdrop blur fade in together, ~180ms.
- Dialog/command panel: opacity plus 0.97 to 1 scale with a slight rise and soft elevation; exit is a faster reverse.
- Sheet/drawer: slides from its own edge on transform, keeping the glass material through the move, with no layout jump behind it.
- Menus and popovers: emerge from their anchor origin with small scale and opacity, settle ~140ms, close ~110ms.

## 7. Theme switching
Coordinate dark/light so the material changes as one:
- On theme change, `theme-provider` adds a short-lived class to `<html>` enabling a ~280ms transition across background, glass surfaces, borders, text, shadows and accents, then removes it so normal interactions stay snappy.
- The atmosphere image opacity change joins the same timing window.

## 8. Micro-interactions
Consistent, restrained feedback on buttons, toggles, switches, checkboxes, tabs, inputs (focus ring fade plus faint border lift), nav items and cards — all using the shared easing and duration tokens so timing feels uniform.

## Technical notes
- Motion is CSS-driven (tokens, utilities, `data-state` selectors) plus small measured indicator math in a layout effect; no animation library is added.
- No changes to routes, data, component APIs, colors, spacing, or typography.
- Verification: build, then a browser pass across tab switches, sidebar and mobile nav, command palette, quick-create dialog, mobile sheet, and the dark/light toggle, checking for layout shift and console errors.