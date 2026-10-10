# Canvas interaction regression captures

Original synthetic artwork, captured offscreen at 1200 x 800, scale 1, with PhotoCraft's snapshot example. No photographs or externally sourced artwork were used. License: adjacent LICENSE-MIT; upstream UI and branding retain their existing notices.

The before runtime is upstream commit 03f4cc0d. Only test/capture helpers from the proposed patch were copied into that detached checkout, without any runtime fixes. The four new after captures use the runtime source at f5a9f06b, rebased onto e1a13013. The two context-menu after captures were taken at 47dfb2ea before rebasing; the menu fix is identical after rebasing.

| Issue | Input | Before | After |
|---|---|---|---|
| #2849 | Secondary press and release in one raw-input frame at fractional screen coordinates 457.5,363.5 | Layer and selection menus disappear | Both menus stay open |
| #2298 | Brush active, pointer over Left artwork, Ctrl+Alt+Shift+U | Empty remains selected | Left artwork selected; Brush retained |
| #2672 | Brush active, hold Z for more than 200 ms, release | Zoom remains selected | Brush restored |
| #1989 | Point-unit scroll (45,-180), followed by zero-delta End in the same frame | View center stays (300,200) | Center becomes (255,380) |
| #1243 | Live Linear gradient from (200,200) to (201,200), on a 600 x 400 document | 60-pixel ramp with displaced endpoints | 1-pixel ramp at the supplied endpoints |

Reproduction uses the proposed snapshot flags: --quick-click, --pick-under-cursor-at 20,30, --hold-zoom, --scroll-by 45,-180. Engine setup calls use the control method engine.execute with {command,params}; tool changes use ui.set. The capture wrapper checks all setup replies for errors.

The layer-pick fixture is a transparent 200 x 100 document with blue Left artwork in rect (10,10,40,60), orange Right artwork in rect (120,10,40,60), and an empty layer above them. Zoom and scrolling use a 600 x 400 document with a blue raster layer over Background. The gradient's stops are #1269c7 and #f07824 with dither disabled.

These screenshots accompany one code PR. This evidence branch has no separate PR.
