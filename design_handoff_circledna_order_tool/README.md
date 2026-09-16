# Handoff: CircleDNA Order Conversion Tool — Redesign

## Overview
An internal ops tool that ingests daily D365 + Shopify exports, matches/validates orders, routes them to the correct warehouse/courier, and generates the warehouse-specific export files (HK–SF, MY/US/Intl–FedEx, Manual FedEx/DHL, Issues, Job orders). This handoff covers the **visual redesign** applied on top of the existing, already-working tool — layout (sidebar nav) and color/type system. The underlying business logic (parsing, routing, splitting, validation, exports) is unchanged and is documented only where the UI depends on it.

## About the Design Files
The bundled file (`CircleDNA Order Conversion Tool.dc.html`) is a **Design Component** — a prototyping format (streaming template + a small runtime, `support.js`, not included here) used in this design tool. It is **not meant to be dropped into a production codebase as-is**. Treat it as a high-fidelity reference: recreate the same DOM structure, inline styles, and behavior using the target app's existing stack (React/Vue/plain JS/etc.), or the most suitable framework if this is being built fresh. All business logic in the file is plain, framework-agnostic JavaScript (functions, no JSX) and can be lifted almost verbatim.

## Fidelity
**High-fidelity.** Colors, type, spacing, radii, and copy below are final — implement pixel-accurately.

## Layout
**Sidebar navigation, fixed-width, non-collapsing** (`240px`), full page height, sticky.
- Sidebar background: deep violet `#3B1E6E`, white text.
  - Top: wordmark "CircleDNA" (24px, display font) + "ORDER CONVERSION" eyebrow (11px, uppercase, letter-spacing .08em, color `#D6BBFF`).
  - Nav: 6 vertical items — 1. Upload, 2. Review (badge for held-order count), 3. Resolve, 4. Export, divider, History, Config. Buttons: `10px 14px` padding, `10px` border-radius, `13.5px` weight 600. Inactive text `rgba(255,255,255,.72)`; active text `#fff` with background `rgba(255,150,147,.16)`.
  - Bottom: processing-date label, `11.5px`, `rgba(255,255,255,.6)`, top hairline `rgba(255,255,255,.14)`.
- Main content: flex:1, `36px 40px` padding, `max-width:1400px`, background `#FDFCFE` (page bg).
- Content sections are cards: white background, `1px solid rgba(59,30,110,.10)` border, `12px` radius, `24px` padding, shadow `0 1px 2px rgba(59,30,110,.07)`.

## Screens
1. **Upload** — processing-date input; two drag-and-drop zones (D365 .xlsx, Shopify .csv); "Process orders" primary button (disabled until both files load).
2. **Review** — stat-card grid (D365 lines, ready, held, released, kits per route) + filterable table of all order lines (order/sales order/SKU/qty/country/route/status/issues).
3. **Resolve** — one card per held order: reason list, a note textarea, and a Release/Undo button that overrides status without erasing the original hold reason.
4. **Export** — list of the 7 output files with Download + "Mark dispatched" actions per file.
5. **History** — table of past runs: files generated (with counts) and dispatch timestamps, from localStorage.
6. **Config** — editable routing/SKU/decision reference, collapsible `<details>` sections, Export/Import/Reset JSON buttons.

## Components & states
- **Primary button**: pill (`999px` radius), background `#7B3FE4` (violet), white text, `13px` / weight 600, uppercase, `.10em` letter-spacing.
- **Secondary/outline button**: transparent bg, `1.5px solid #3B1E6E` border, text `#2B1450`.
- **Status pill — Ready**: background `rgba(27,138,90,.13)`, text `#1B8A5A` (green).
- **Status pill — Hold**: background `rgba(214,41,63,.12)`, text `#D6293F` (red/pink).
- **Manual-route pill**: background `#F7F5FB`, text `#756B87`, `1px` border.
- **Tag** (issue reason chip): background `#F7F5FB`, `6px` radius, `10.5px` text, color `#756B87`.
- **Dropzone**: `2px dashed rgba(59,30,110,.22)`, `12px` radius; filled state → solid border `#7B3FE4`, background `#F1EBFC`.
- **Table**: header row background `#F7F5FB`, `11px` uppercase `#756B87` labels, sticky top; rows `8px 10px` padding, `1px` hairline dividers `rgba(59,30,110,.10)`.
- **Toast**: bottom-right, pill, background `#3B1E6E`, white text, fades in/out (`opacity` 0→1, 0.2s).

## Design tokens
| Token | Value |
|---|---|
| Burgundy / sidebar bg | `#3B1E6E` |
| Primary accent (CTA) | `#7B3FE4` |
| Flamingo (accent on dark) | `#D6BBFF` |
| Blush (warm card tint) | `#F1EBFC` |
| Cream (section bg) | `#F7F5FB` |
| Off-white (page bg) | `#FDFCFE` |
| Status — ready (green) | text `#1B8A5A` / bg `rgba(27,138,90,.13)` |
| Status — hold (red/pink) | text `#D6293F` / bg `rgba(214,41,63,.12)` |
| Heading text | `#2B1450` |
| Body text | `#1A1524` |
| Muted text | `#756B87` |
| Border (soft / strong) | `rgba(59,30,110,.10)` / `rgba(59,30,110,.22)` |
| Radius — card / input | `12px` |
| Radius — pill (buttons, tags) | `999px` |
| Font — display/headings | Inter (matches circledna.com's clean sans; no separate display serif) |
| Font — body/UI | Inter |
| Shadow — resting card | `0 1px 2px rgba(59,30,110,.07)` |
| Shadow — elevated (toast) | `0 12px 32px rgba(59,30,110,.16)` |

Note: exact CircleDNA brand hex values weren't extractable from the live site (text-only fetch); the above is a close approximation matched to the site's violet/white palette. Confirm against the brand's real style guide if available.

## Assets
No custom images/icons used — layout relies on color, type, and simple shapes only.

## Files
- `CircleDNA Order Conversion Tool.dc.html` — the full design + all business logic (parsing, matching, routing, splitting, export builders). The `<style>` block near the top of the file (inside `<helmet>`) holds the color-token overrides; everything else uses inline styles referencing those CSS variables (`var(--im8-red-algae)`, `var(--fg1)`, etc. — names are historical, values are the CircleDNA palette above).
