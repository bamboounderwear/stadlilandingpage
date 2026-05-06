---
name: saas-ui-system
description: >
  Single source of truth for all UI/UX design decisions in this Bootstrap 5 + SCSS SaaS application.
  Use this skill whenever building, editing, reviewing, or critiquing any frontend template, component,
  or stylesheet — including new pages, forms, cards, buttons, typography, spacing, color, icons, loading
  states, mobile layouts, or data formatting. Also use for design critiques, "make this look good" requests,
  visual hierarchy questions, and accessibility reviews. Trigger on: "add a page", "build a component",
  "style something", "fix the UI", "make it consistent", "update the design", "how should I show X",
  "design a UI", "improve this interface", "this feels off", or any question about visual patterns.
  Never guess — always read this skill first.
---

# SaaS UI System

Design system for a Bootstrap 5 + SCSS SaaS (ticketing platform) deployed on Cloudflare Workers via Hono. Mixed rendering: SSR HTML pages plus JS-heavy components (seatmap, survey builder, real-time updates).

**Stack:** Hono · Bootstrap 5 · SCSS · Bootstrap Icons · Cloudflare Workers

---
 
## Core Philosophy

1. **Bootstrap utilities first** — if a Bootstrap utility class can do it, use it. Never write custom CSS for something Bootstrap already covers.
2. **Customize Bootstrap defaults** — override `$variables` and component defaults in SCSS so Bootstrap's own classes produce the right output. Avoid fighting Bootstrap with overrides.
3. **Custom utilities over custom classes** — when Bootstrap can't cover a pattern, add it to the `$utilities` map. Never write a one-off hardcoded style.
4. **Token-first** — all color and semantic values use `var(--bs-*)`. No hardcoded hex values outside the always-dark zone.
5. **Typography-first** — words carry the interface. Icons are rare, semantic, and always paired with a label. Default to text. See Icon Policy below.
6. **Flat** — no shadows. Elevation = border + background contrast.
7. **Always light/dark safe** — every component works in both modes without modification.
8. **Monochrome buttons** — all buttons are black or white. No blue. No colored button backgrounds. Ever.
9. **Design is problem-solving, not decoration** — every decision serves the user's needs. Avoid trendy effects and unnecessary embellishments.

---

## Design Process

Follow this workflow for every design or redesign task:

1. **Understand the purpose** — What problem does this interface solve? Who uses it?
2. **Establish hierarchy** — What's most important? What should users see first?
3. **Fix at the source** — Before touching anything, identify where the pattern is defined. See "Design at the Source" below.
4. **Implement cleanly** — Bootstrap utilities first, then variables, then utilities map, then custom classes.
5. **Review critically** — Does it follow the principles? Is anything superfluous? Run the Quality Checklist.

---

## Icon Policy — CRITICAL

**Default to text. Icons are the exception, not the rule.**

Icons must be justified individually. Before adding any icon, ask: *"Does this icon communicate something that the text label cannot?"* If the answer is no, omit it.

**Never use icons:**
- Next to button labels (buttons are text-only unless it's a recognized universal action like a close ×)
- As decoration next to headings, section titles, or card headers
- To represent abstract concepts (e.g. a lightbulb for "tips", a rocket for "launch")
- In place of text labels — icons alone are ambiguous and inaccessible
- In lists or tables to add visual "interest"

**Only use icons when:**
- The icon is universally understood AND space is critically constrained (e.g. toolbar, pagination prev/next)
- It's a status indicator with no room for a label (e.g. a checkmark in a tight table cell) — and even then, include a `title` or `aria-label`
- It's a close/dismiss button (×) in a modal or alert

**Rules when an icon is used:**
- Always pair with a visible text label or `aria-label`
- Always add `aria-hidden="true"` to the icon element
- Color is always `text-body-secondary` (neutral gray) — never semantic color
- Size matches surrounding text — never oversized for decoration

**In practice:** A page with zero icons is preferred over a page with unnecessary icons. Resist the urge to "liven up" the UI with icons. Typography, spacing, and hierarchy do that job.

---

## Design at the Source — CRITICAL

**Always fix the root, never the instance.** When something looks wrong or needs changing, the instinct to patch a single element with an inline style or one-off class is almost always wrong. Find where the pattern is defined and fix it there so every instance benefits automatically.

### The source hierarchy — fix the highest applicable level

```
1. SCSS variable or Bootstrap $variable override  → affects all components globally
2. Bootstrap component variable (e.g. $card-border-radius)  → affects that component everywhere
3. $utilities map entry in styles.scss            → affects all uses of that utility
4. A shared structural class (.text-label, .btn-ghost, etc.)  → affects all uses of that class
5. A template partial or layout file              → affects all pages using that partial
6. An individual element                          → LAST RESORT, and only if truly one-off
```

**Before touching a single element, ask:** "Is there another place in this project that has the same issue?" If yes, the fix belongs higher up the hierarchy.

### Symptoms that mean you're fixing at the wrong level

| What you're about to do | Where to fix it instead |
|---|---|
| Add `style="font-size: 0.875rem"` to one element | Set `$font-size-sm` or use `fs-6` utility |
| Add `class="rounded-3"` to every card manually | Set `$card-border-radius` in SCSS |
| Copy-paste the same button markup with tweaks | Create a reusable partial or macro |
| Override a color on one component | Check if the token or variable is wrong globally |
| Add `mt-4` to every `<h2>` on a page | Set heading margin in SCSS or a shared class |
| Inline style to fix spacing in one template | Fix the layout partial that all templates share |

### Consistency check before delivering any change

When making any design change, explicitly ask:
1. **Where else does this pattern appear?** List the other places.
2. **Will this change apply to all of them?** If not, move the fix higher.
3. **Am I introducing a new one-off?** If yes, reconsider — add a utility or class instead.

A design change that only fixes one page while leaving identical issues elsewhere is incomplete. Always widen the fix to cover the full pattern.

---

## 1. Bootstrap-First Decision Ladder

Follow this order for **every** styling decision:

```
1. Does a Bootstrap utility class do this?         → Use it. Done.
2. Does a Bootstrap component cover this?          → Use it, customize via variables.
3. Can a $utilities map entry cover this?          → Add it to styles.scss.
4. Does this require a structural custom class?    → Write it, var(--bs-*) tokens only.
5. Is this in the always-dark zone?               → Use established dark values only.
```

### Utility groups — use these instead of custom CSS

**Typography:** `fs-1`–`fs-6` · `fw-light` `fw-normal` `fw-medium` `fw-semibold` `fw-bold` · `fst-italic` · `text-uppercase` · `text-decoration-none` · `text-truncate` `text-nowrap` · `text-start` `text-center` `text-end`

**Color — text (auto light/dark):** `text-body` · `text-body-secondary` · `text-body-tertiary` · `text-body-emphasis` · `text-secondary` · `text-muted` · `text-success` · `text-danger` · `text-warning` · `text-info`

**Color — background (auto light/dark):** `bg-body` · `bg-body-secondary` · `bg-body-tertiary` · `bg-success-subtle` · `bg-danger-subtle` · `bg-warning-subtle` · `bg-info-subtle`

**Opacity:** `text-opacity-25/50/75` · `bg-opacity-10/25` · `opacity-25/50/75`

**Border:** `border` `border-top` `border-bottom` `border-start` `border-end` `border-0` · `border-secondary` `border-success` `border-danger` · `border-opacity-25/50`

**Border radius:** `rounded-1` (0.25rem) · `rounded-2`/`rounded` (0.375rem) · `rounded-3` (0.5rem) · `rounded-4` (0.75rem) · `rounded-pill`

**Layout:** `d-flex` `d-grid` `d-none` `d-block` `d-inline-flex` + responsive variants · `position-sticky` `position-relative` `position-absolute` · `overflow-hidden` `overflow-auto`

**Spacing:** `p-*` `m-*` `px-*` `py-*` `gap-*` (0–10) · `me-*` `ms-*`

---

## 2. Color System

### SCSS variables (source of truth)

```scss
$white: #ffffff;    $black: #020202;
$gray-100: #e8e8e8; $gray-200: #d6d6d6; $gray-300: #c0c0c0;
$gray-400: #929292; $gray-500: #6a6a6a; $gray-600: #484848;
$gray-700: #303030; $gray-800: #1a1a1a; $gray-900: #010101;

// Semantic
$primary:   $gray-900;  // dark: $gray-100
$secondary: $gray-600;  // dark: $gray-400
$success:   #3f7f5f;    // dark: #7ec7a3
$info:      #3b6f8d;    // dark: #7cb0ce  — blue-gray, NOT vibrant blue
$warning:   #a0712c;    // dark: #d7b06b
$danger:    #954947;    // dark: #d28b86
```

### Semantic color discipline — CRITICAL

Colors are functional tools, not decoration. ~90% of UI should be neutral (body-bg / tertiary-bg / body-color). When reaching for a color, ask: **"What data story am I telling, and does this color directly support it?"**

**One semantic family per component.** A card that uses success green must not also use warning orange or info blue.

| Color | Meaning | Use for |
|---|---|---|
| **Primary** (gray-900/100) | Main action / selection | CTAs, active nav, selected state |
| **Success** (#3f7f5f → #7ec7a3) | Positive / complete / on-sale | Published status, positive deltas, confirmations |
| **Warning** (#a0712c → #d7b06b) | Caution / draft / partial | Draft status, low capacity, expiring soon |
| **Danger** (#954947 → #d28b86) | Error / destructive / negative | Cancelled, failed, delete actions |
| **Info** (#3b6f8d → #7cb0ce) | Neutral informational only | Help text, informational banners |

**Semantic colors must NEVER appear on text.** Accents live on dots, borders, backgrounds — never on text labels, headings, or paragraph copy. The only exception is `text-success` / `text-danger` / `text-warning` on small inline status text when space does not allow a badge, and even then use sparingly.

### Button color policy — CRITICAL

**All buttons are black or white. No exceptions.** Buttons never use Bootstrap's default blue or any semantic color background. The only two button fills are black (`$gray-900`) in light mode and white (`$gray-100`) in dark mode — achieved via `btn-primary` with our `$primary` override. Every other button variant is monochrome: outlined or ghost.

| Variant | Light mode | Dark mode | Use for |
|---|---|---|---|
| `btn-primary` | Black bg, white text | White bg, black text | Primary CTA — one per view |
| `btn-outline-secondary` | Transparent, gray border | Transparent, gray border | Secondary actions (Cancel, Export, filters) |
| `btn-ghost` | Transparent, no border | Transparent, no border | Tertiary / inline actions |
| `btn-danger` | Muted red bg | Muted red bg | Destructive confirmation only (delete modals) |

**Rules:**
1. **Never use `btn-primary` with Bootstrap's default blue.** Our `$primary` is `$gray-900` (light) / `$gray-100` (dark). If buttons render as blue, the SCSS variable override is missing — fix the stylesheet, not the markup.
2. **Never use `btn-success`, `btn-warning`, `btn-info`** as button variants. These produce colored buttons. Buttons are always black/white/gray or ghost.
3. **Never use `btn-outline-primary`** — it produces a dark-bordered button that looks identical to `btn-outline-secondary` but with wrong hover. Use `btn-outline-secondary` for all outlined buttons.
4. **One `btn-primary` per view section.** If there are two actions, one is primary (black/white fill) and the other is `btn-outline-secondary` or `btn-ghost`.
5. **`btn-danger` is the only colored button allowed**, and only for destructive confirmation dialogs (e.g. "Delete event" in a modal). Never for inline or casual actions.
6. **Email templates** use inline `background-color: #010101; color: #ffffff;` for CTA buttons — same black/white principle, hardcoded because email has no CSS variables.

### Never use blue for icons

**The info/blue family (`#3b6f8d`) is for informational banners only.** It must never be used to color icons, buttons, borders, or decorative elements. Icons in this system are always `text-body-secondary` (muted gray) or inherited. Blue carries a specific semantic meaning (neutral information) and should not leak into general UI chrome.

**This rule extends to the spectrum blue (`#0140f0`).** The spectrum blue is permitted as a data-viz series color or on marketing surfaces only — never on an icon glyph, a focus ring, or an admin button. "No blue icons" applies to every blue in the system.

### Interaction states — zero-color policy

Hover, focus, and active states use **white/neutral** affordances only. They must never introduce a color that is not already present at rest.

```scss
// CORRECT — neutral affordance
&:hover { background: var(--bs-secondary-bg); }
&:focus { box-shadow: 0 0 0 0.2rem rgba(var(--bs-emphasis-color-rgb), 0.12); }

// WRONG — color introduced on hover
&:hover { color: var(--bs-success); }
&:hover { border-color: var(--bs-info); }
```

This applies universally: buttons, cards, table rows, nav links, form controls. **Text color must never change to a semantic color on hover.**

### Spectrum palette — extended use only

In addition to the core semantic palette above, a **12-color spectrum** is available for three scoped contexts: **data visualization**, **narrative / insight cards**, and **marketing surfaces**. The spectrum is NOT a replacement for the core semantic tokens — it is an extension reserved for surfaces where a richer expressive range is required.

**Precedence — core rules always win.** If a component is subject to a core rule (monochrome buttons, neutral icons, text-first, zero-color hover), the spectrum does NOT override it. A spectrum color never appears on a primary CTA in admin, on a form control border, on an icon glyph, or in a hover state.

**Spectrum colors must NEVER appear on:**
- Buttons, button borders, or button text — admin buttons stay monochrome per the button policy
- Icon glyphs — icons remain `text-body-secondary` per the icon policy
- Body text, headings, or paragraph copy
- Form controls, focus rings, or generic borders
- Admin chrome (nav, sidebar, table headers, toolbars, modals)
- Any hover / focus / active state on an otherwise neutral component

**Spectrum colors ARE allowed on:**
- **Data viz** — chart series, sparkline fills, trend bars, heatmap cells, legend swatches
- **Narrative / insight card tags** — as a 12%-opacity pill background with the full color as the dot and text (Section 19)
- **Marketing surfaces** — public landing pages, campaign banners, event hero moments, email promos, where brand voice is required
- **Expressive status dots** — when a richer set than success / warning / danger is needed (e.g. at-risk orange, forecast violet, outlier magenta)

### Spectrum reference

Each spectrum color carries three parallel meanings. Choose the column that matches the surface you're designing for.

| Name | Hex | UI meaning | Data meaning | Marketing meaning |
|---|---|---|---|---|
| **Red** | `#fe0000` | Destructive · error · danger state | Negative delta · churn · revenue loss · decline | Urgency · scarcity · countdown · limited-time |
| **Orange** | `#fe5619` | Warning · attention-needed · approaching limit | At-risk metric · caution threshold · near-miss | Energy · hype · excitement · warm call-to-action |
| **Lime** | `#ecfc3b` | New badge · highlight · just-updated indicator | Spike · trending anomaly · breakout metric | Disruption · freshness · attention-grab · novelty |
| **Green** | `#44d62b` | Success · confirmed · valid · complete | Positive growth · target met · goal achieved | Momentum · wins · go-signal · results |
| **Teal** | `#00d4a8` | Active state · selected · toggled-on | Benchmark · baseline reference · comparison anchor | Innovation · trust · modern · forward-thinking |
| **Cyan** | `#00b0d8` | Informational · link · hint · guidance | Volume series · engagement metric · secondary KPI | Openness · clarity · accessibility · inviting |
| **Blue** | `#0140f0` | Primary interactive · focus ring · CTA¹ | Primary KPI series · headline metric · lead indicator | Authority · confidence · reliability · trust |
| **Violet** | `#7b2ff7` | Premium feature · pro badge · gated content | Forecast · projection · modeled estimate | Exclusivity · vision · premium tier · aspiration |
| **Magenta** | `#d633ff` | Notification badge · system alert · unread count | Outlier · anomaly flag · unusual pattern | Boldness · creative · standout · differentiation |
| **Rose** | `#f6166e` | Favorite · loyalty · heart · bookmarked | Fan sentiment · engagement score · NPS positive | Passion · fandom · emotion · connection |
| **Silver** | `#eeeeee` | Disabled · muted · divider · placeholder | Empty state · inactive period · no-data fill | Breathing room · minimalism · negative space |

¹ **Blue in the "UI meaning" column is reserved for marketing surfaces ONLY.** Inside admin and dashboard chrome, primary CTAs remain monochrome black/white per the button policy. The "Never use blue for icons" rule applies everywhere, including the spectrum blue — it is never used on icon glyphs, even in marketing contexts.

### Spectrum tokens in SCSS

Define spectrum colors as `$spectrum-*` variables in `styles.scss` and expose them as CSS custom properties so data-viz and marketing components can reference them without hardcoded hex.

```scss
// Raw spectrum tokens — source of truth
$spectrum-red:     #fe0000;
$spectrum-orange:  #fe5619;
$spectrum-lime:    #ecfc3b;
$spectrum-green:   #44d62b;
$spectrum-teal:    #00d4a8;
$spectrum-cyan:    #00b0d8;
$spectrum-blue:    #0140f0;
$spectrum-violet:  #7b2ff7;
$spectrum-magenta: #d633ff;
$spectrum-rose:    #f6166e;
$spectrum-silver:  #eeeeee;

// Expose as CSS custom properties
:root {
  --spectrum-red:     #{$spectrum-red};
  --spectrum-orange:  #{$spectrum-orange};
  --spectrum-lime:    #{$spectrum-lime};
  --spectrum-green:   #{$spectrum-green};
  --spectrum-teal:    #{$spectrum-teal};
  --spectrum-cyan:    #{$spectrum-cyan};
  --spectrum-blue:    #{$spectrum-blue};
  --spectrum-violet:  #{$spectrum-violet};
  --spectrum-magenta: #{$spectrum-magenta};
  --spectrum-rose:    #{$spectrum-rose};
  --spectrum-silver:  #{$spectrum-silver};
}
```

Usage rules:
1. **Reference via token only** — `var(--spectrum-violet)`, never the raw hex, inside any spectrum-sanctioned surface.
2. **Pair with neutral text and backgrounds** — spectrum hues are raw brand colors and are not guaranteed to meet contrast on their own. Text sits on neutral tokens; spectrum lives on dots, bars, fills, and tag pills.
3. **12% opacity backgrounds** — when used as a tag / chip background, wrap with `rgba(...,0.12)` (same convention as narrative cards, Section 19).
4. **Never layer spectrum on spectrum** — no violet text on a green background, no red border on a teal fill. One spectrum color per atomic element.
5. **Semantic token takes priority** — if a surface has both a core semantic token (`$success`, `$danger`, etc.) AND a spectrum equivalent, use the core token for UI state and reserve the spectrum color for data viz / marketing.

### Picking the right column

The same hex can mean different things on different surfaces. Match the column to the surface:

| Surface | Use this column | Example |
|---|---|---|
| Admin chrome (still subject to core rules) | UI meaning | Orange dot = "approaching limit" on a capacity gauge |
| Chart, sparkline, trend bar | Data meaning | Green bar = positive growth; red bar = revenue loss |
| Narrative / insight card tag | UI meaning (mapped via `TAG_LABEL_COLORS`) | Violet tag = forecast insight |
| Public landing page, event hero, promo banner | Marketing meaning | Orange CTA on a limited-time promo hero |
| Email template | Marketing meaning (inline hex, no tokens) | Blue primary CTA in a marketing email only — admin transactional emails stay `#010101` black |

If the surface spans multiple contexts, the stricter interpretation wins — i.e. if in doubt, treat it as admin chrome and use the core semantic palette, not the spectrum.

---

## 3. Light/Dark Safety

**Never use a hardcoded color outside the always-dark zone (Section 9).** If a hex value appears in custom CSS and it's not inside a documented always-dark component, it's a bug.

### Token reference

| Token | Light | Dark | Use for |
|---|---|---|---|
| `--bs-body-bg` | `#e8e8e8` | `#010101` | Page background |
| `--bs-body-color` | `#010101` | `#e8e8e8` | Primary text |
| `--bs-secondary-color` | `#303030` | `#929292` | Muted / secondary text |
| `--bs-tertiary-color` | `#484848` | `#6a6a6a` | Placeholder / tertiary text |
| `--bs-emphasis-color` | `#010101` | `#ffffff` | Maximum contrast |
| `--bs-tertiary-bg` | `#ffffff` | `#080808` | Cards, modals, dropdowns |
| `--bs-secondary-bg` | `#d6d6d6` | `#1a1a1a` | Subtle fills, card headers, hover states |
| `--bs-border-color` | `#d6d6d6` | `#161616` | All borders |
| `--bs-border-color-translucent` | `rgba(2,2,2,0.08)` | `rgba(255,255,255,0.12)` | Overlay borders |

### Dark mode

```html
<html>                       <!-- light (default) -->
<html data-bs-theme="dark">  <!-- dark -->
```

- Use `data-bs-theme` attribute only. Never `.dark` class, never `@media (prefers-color-scheme: dark)`.
- Custom dark overrides: `@include color-mode(dark, true) { }` in SCSS.
- Test every new component in both modes before shipping.

### Elevation — no shadows

No shadows in either mode. Use border + background contrast only.

```scss
// CORRECT
border: 1px solid var(--bs-border-color);
background: var(--bs-tertiary-bg); // visually above body-bg

// WRONG
box-shadow: 0 4px 12px rgba(0,0,0,0.1);
```

Interactive hover on clickable cards: `transform: translateY(-2px)` only — no shadow added.

---

## 4. Typography

**Sentence case everywhere.** No title case. No ALL CAPS prose.

```
CORRECT:  Save changes  /  New event  /  Delete account
WRONG:    Save Changes  /  New Event  /  DELETE ACCOUNT
```

Exception: `.text-label` only — uppercase metadata (`SAT, 15 MAR`, status annotations).

| Element | Size | Weight | Use for |
|---|---|---|---|
| `h1` / `.h1` | 2rem (2.5rem md+) | 700 | Page titles |
| `h2` / `.h2` | 1.5rem | 700 | Section headings |
| `h3` / `.h3` | 1.25rem | 600 | Card headings |
| `h4`–`h6` | 1rem–0.875rem | 600 | Minor headings |
| Body | 1rem | 400 | Body copy |
| `.small` | 0.875rem | 400 | Captions, secondary info |
| `.text-label` | 0.75rem | 700 | Uppercase metadata only |

**Minimum visible text: `small` (0.875rem).** Never go below this.

Use Bootstrap utilities for all type styling — never write `font-size` or `font-weight` in custom CSS.

```html
<!-- CORRECT -->
<h2 class="h4 fw-bold mb-1">Upcoming events</h2>
<p class="small text-body-secondary mb-0">Sorted by date</p>

<!-- WRONG -->
<h2 style="font-size: 1.35rem; font-weight: 700;">Upcoming events</h2>
```

### .text-label (uppercase metadata)

```scss
.text-label {
  display: block;
  font-size: 0.75rem;
  text-transform: uppercase;
  letter-spacing: 0.08em;
  font-weight: 700;
}
```

Use for metadata only — dates, category annotations, table column headers. Never as a heading substitute. Never inside a pill/badge component.

---

## 5. Status Indicators — Dot Pills Preferred

For status communication, the **dot pill** pattern is preferred over colorized badges. It keeps the semantic color isolated to a single dot, leaving text neutral and accessible in both modes.

### Dot pill (preferred)

```html
<!-- Published -->
<span class="d-inline-flex align-items-center gap-2 border rounded-pill px-2 py-1 small">
  <span class="rounded-circle bg-success flex-shrink-0" style="width:.5rem;height:.5rem;"></span>
  Published
</span>

<!-- Draft -->
<span class="d-inline-flex align-items-center gap-2 border rounded-pill px-2 py-1 small">
  <span class="rounded-circle bg-warning flex-shrink-0" style="width:.5rem;height:.5rem;"></span>
  Draft
</span>

<!-- Cancelled -->
<span class="d-inline-flex align-items-center gap-2 border rounded-pill px-2 py-1 small">
  <span class="rounded-circle bg-danger flex-shrink-0" style="width:.5rem;height:.5rem;"></span>
  Cancelled
</span>
```

Rules:
- Dot is the **only** colored element. Pill background and text are always neutral (`bg-body-secondary` or transparent, `text-body-color`).
- Text is **never** uppercase inside a pill. Use sentence case or title case only.
- Text color is **never** a semantic color. Never `text-success`, `text-danger`, etc. in a pill.
- Use `whitespace-nowrap` on pill text — pills must never wrap.

### Bootstrap badge (acceptable for dense contexts)

```html
<span class="badge text-bg-success">Published</span>
<span class="badge text-bg-warning">Draft</span>
<span class="badge text-bg-danger">Cancelled</span>
<span class="badge text-bg-secondary">Archived</span>
```

`text-bg-*` variants handle contrast automatically in both modes. Use in table cells and compact spaces where dot pills would be too wide.

---

## 6. Iconography — Text-First, Semantic-First

**Default answer: no icon.** Add one only when removing it would genuinely degrade comprehension.

> Before adding any icon: *"Does removing this make the UI worse?"* If not — remove it.

### Never use blue/color on icons

Icons are always `text-body-secondary` (gray) or inherit from context. **Never color an icon with a semantic color** (no green check icons, no blue info icons, no orange alert icons). The color lives in the badge dot or the alert background — not on the icon glyph itself.

```html
<!-- CORRECT: icon inherits neutral color -->
<i class="bi bi-trash3 me-1 text-body-secondary" aria-hidden="true"></i>

<!-- WRONG: icon colored with semantic color -->
<i class="bi bi-check-circle text-success" aria-hidden="true"></i>
<i class="bi bi-info-circle text-info" aria-hidden="true"></i>
```

Exception: icons inside `alert-*` components inherit the alert's contextual color — acceptable because the container already establishes the semantic context.

### Where icons are acceptable

| Context | Rule |
|---|---|
| Empty states | One illustrative icon max. `text-body-secondary opacity-50`. |
| Alert messages | Icon inherits alert context color. Always paired with message text. |
| Destructive actions | Trash/warning icon + label. Icon stays neutral gray. |
| Dense toolbars (exception) | Icon-only buttons allowed only here. Must have `aria-label` + tooltip. |

### Text-only (no icon)

Primary buttons · secondary buttons · form labels · page headings · tabs · pills · table column headers · sidebar nav items.

```html
<!-- CORRECT: text-only nav -->
<a href="/events" class="btn btn-ghost text-start">Events</a>

<!-- WRONG: icon-heavy nav -->
<a href="/events" class="btn btn-ghost text-start">
  <i class="bi bi-calendar-event me-2" aria-hidden="true"></i>Events
</a>

<!-- WRONG: icon on primary CTA -->
<button class="btn btn-primary">
  <i class="bi bi-floppy2 me-2" aria-hidden="true"></i>Save changes
</button>
```

**Bootstrap Icons only.** Size with `fs-*` utilities. Always `aria-hidden="true"` on every `<i>`.

---

## 7. Spacing & Border Radius

**Spacing:** Bootstrap spacer scale only. No raw px/rem values in templates. In SCSS: `$spacer * N`.

Common patterns: card padding `p-3` · compact `px-3 py-2` · stack gap `gap-2` or `gap-3` · section separator `mb-4` or `<hr class="my-4">`.

**Border radius:** Use `rounded-*` utilities. No `border-radius` in custom CSS.

| Utility | Value | Use for |
|---|---|---|
| `rounded-1` | 0.25rem | Badge, chip, dot pill |
| `rounded-2` / `rounded` | 0.375rem | Button, input |
| `rounded-3` | 0.5rem | Card, dropdown, panel |
| `rounded-4` | 0.75rem | Modal, bottom sheet |
| `rounded-pill` | 50rem | Dot pill, avatar |

---

## 8. Transitions

Snappy: **100–150ms**. Explicit properties only — never `transition: all`.

```scss
transition: color 0.12s ease, background-color 0.12s ease, border-color 0.12s ease;

// Card hover lift
transition: transform 0.15s ease;
&:hover { transform: translateY(-2px); } // No shadow added

// Spatial panels / drawers
transition: width 0.25s ease, transform 0.25s cubic-bezier(0.16, 1, 0.3, 1);

// AI badge shimmer (exception — long loop, decorative)
@keyframes badge-shimmer {
  0%   { background-position: 200% 0; }
  100% { background-position: -200% 0; }
}
// 2.4s ease-in-out infinite — reserved for .badge-ai only
```

---

## 9. Always-Dark Zone

Intentionally always dark. Do not convert to tokens.

| Component | Classes |
|---|---|
| Seatmap canvas | `.seatmap-canvas` |
| Event sidebar + ticket cards | `.event-sidebar-panel`, `.ticket-card`, `.section-list-item` |
| Event hero | `.event-hero`, `.event-hero-overlay` |
| Fullscreen seatmap overlay | `.seatmap-fs-overlay`, canvas area |

Inside these, use only established dark values from `styles.scss` (`#0b1526`, `#111827`, `#1b2330`, `#2d2f34`). No new hardcoded colors even here. Everything outside this list uses `var(--bs-*)`.

---

## 10. Page Contexts

### Admin / dashboard
`.container-lg` capped width. Sticky header: `position-sticky top-0 bg-body border-bottom`. Text-only sidebar nav using `btn btn-ghost text-start`. Data tables with `table-hover align-middle`.

### Public-facing event pages
Full-width on canvas/map views; `.container-lg` on editorial sections. Always-dark zone components live here. Hero images always use `.event-hero` + `.event-hero-overlay`.

### Auth pages
Centred single-column: `min-vh-100 d-flex align-items-center justify-content-center bg-body`. Card max-width `420px` inline style. No sidebar, no navbar. Form errors via Bootstrap inline validation — no toast for auth errors. CTA button: `btn btn-primary w-100`.

### Email templates
Inline styles only. No Bootstrap classes. Max-width 600px table layout. Background `#e8e8e8`. System sans-serif font stack. Buttons: inline `background-color: #010101; color: #ffffff; border-radius: 6px; padding: 12px 24px`. No shadows, no CSS variables.

---

## 11. Feedback & Overlay Patterns

### Toasts
Bootstrap `.toast` only. `text-bg-success/danger/secondary`. Auto-dismiss 4s for success/info, manual-only for errors. No icons in toasts — color carries the semantic signal.

### Confirmation dialogs
Always Bootstrap `.modal` — never `window.confirm()`. Destructive confirmation uses `btn-danger` for the confirm action and `btn-outline-secondary` for cancel. This is the **only** place a colored button (`btn-danger`) is acceptable.

### Inline validation
Validate on blur. Bootstrap `.is-invalid` / `.is-valid` + `.invalid-feedback` / `.valid-feedback` only. No custom error components.

### Offcanvas
`.offcanvas-end` for desktop panels, `.offcanvas-bottom` with `rounded-top-4` for mobile sheets.

### Tooltips & popovers
Bootstrap only. Always `data-bs-title` in sentence case. Icon-only buttons must have both `aria-label` and a tooltip.

---

## 12. Complex Interaction Patterns

### Sortable lists
Drag handle: `bi bi-grip-vertical text-body-secondary opacity-50`. Dragged item: `opacity-50`. Drop target: `border-primary bg-primary bg-opacity-10` via JS.

### Data tables with bulk actions
```html
<div class="d-flex align-items-center gap-3 px-3 py-2 bg-body-secondary border rounded-3 mb-2">
  <span class="small fw-medium"><span id="selectedCount">3</span> selected</span>
  <div class="vr opacity-25"></div>
  <button class="btn btn-ghost btn-sm">Export</button>
  <button class="btn btn-ghost btn-sm text-danger">Delete</button>
  <button class="btn btn-ghost btn-sm ms-auto">Clear</button>
</div>
```

### Multi-step forms
Step indicator built from utilities — no external library. Active step: `bg-body-emphasis text-body-bg`. Upcoming steps: `border opacity-50`.

### File upload drop zone
Active drag state: add `border-primary bg-primary bg-opacity-10` via JS. No custom CSS needed.

### Real-time updates (SSE/WebSocket)
`aria-live="polite"` on the live region container. Show inline spinner during reconnection only — not during normal data refresh.

---

## 13. Mobile Layout Patterns

### Responsive table → card stack
```html
<div class="d-none d-md-block"><!-- table --></div>
<div class="d-md-none d-flex flex-column gap-2"><!-- card stack --></div>
```

### Collapsible sidebar
`.offcanvas-start` for mobile, in-flow `aside` for desktop. Same nav content in both.

### Bottom sheet
`.offcanvas-bottom` + `rounded-top-4` + `max-height: 85vh`.

### Sticky bottom action bar
```html
<div class="d-md-none position-fixed bottom-0 start-0 end-0 bg-body border-top px-3 py-2"
     style="z-index:1030; padding-bottom: env(safe-area-inset-bottom, 0.5rem);">
  <button class="btn btn-primary w-100">Continue to checkout</button>
</div>
```

### Full-screen modals on mobile
`modal-fullscreen-sm-down` + `modal-dialog-scrollable`.

---

## 14. Images & Media

**Thumbnails:** `.ratio.ratio-16x9` + `object-fit-cover`. Fade-in on load:
```html
<img style="opacity:0; transition: opacity 0.15s ease;" onload="this.style.opacity=1">
```

**QR codes:** Always on white background regardless of theme:
```html
<div class="d-inline-block p-3 bg-white rounded-3 border">
  <img src="/qr/ORDER-123.svg" width="180" height="180" alt="QR code for order #ORD-A4F2B1">
</div>
```

**Avatars:**
```html
<span class="d-inline-flex align-items-center justify-content-center rounded-circle bg-body-secondary fw-semibold small flex-shrink-0"
      style="width:2rem;height:2rem;" aria-label="Alice Smith">AS</span>
```

Always provide descriptive `alt` text. Never `alt=""` on content images.

---

## 15. Loading & Async States

**Skeleton:** Bootstrap `.placeholder` + `.placeholder-glow`. Match the shape of real content.

**Button submitting:** Disable + replace label with `spinner-border-sm` + in-progress text.

**Inline refresh:** Small `spinner-border-sm` inline with content — never a full overlay.

**Drag in progress:** `opacity-50` on dragged item, `border-primary bg-primary bg-opacity-10` on drop target — utility classes added via JS.

---

## 16. Data Formatting

| Data | Format | Example |
|---|---|---|
| Currency | Symbol prefix, 2 decimals, tabular nums | `$1,250.00` |
| Currency (dashboard) | Abbreviated | `$12.4k` / `$1.2M` |
| Date (full) | `Day, DD Mon YYYY` | `Sat, 15 Mar 2025` |
| Date (same year) | `DD Mon` | `15 Mar` |
| Time | 12hr, lowercase, no leading zero | `7:30pm` |
| Date + time | Comma-separated | `Sat, 15 Mar 2025, 7:30pm` |
| Seat label | Uppercase monospace | `Sec A, Row 12, Seat 4` |
| Order reference | Uppercase monospace, prefixed | `#ORD-A4F2B1` |
| Percentage | Integer + `%` | `87%` |
| Capacity | `n / total` | `342 / 500` |

```html
<span class="fw-bold" style="font-variant-numeric:tabular-nums;">$1,250.00</span>
<span class="fw-bold font-monospace text-uppercase small">Sec A, Row 12, Seat 4</span>
<span class="font-monospace small text-body-secondary">#ORD-A4F2B1</span>
```

---

## 17. Custom Utilities in styles.scss

Check before adding to avoid duplication.

| Class | What it does |
|---|---|
| `backdrop-blur-{none\|sm\|\|md\|lg}` | `backdrop-filter: blur(0–16px)` |
| `w-5` through `w-100` (5-step) | Width percentages |
| `h-5` through `h-100` (5-step) | Height percentages |
| `p-6` through `p-10`, `gap-6`–`gap-10` | Extended spacers (4rem–10rem) |

**Adding a utility** — use the `$utilities` map, not a one-off class:
```scss
$utilities: map-merge($utilities, (
  "cursor": (
    property: cursor,
    class: cursor,
    values: (pointer: pointer, default: default, grab: grab, not-allowed: not-allowed)
  ),
));
```

---

## 18. Structural Custom Classes

These require custom CSS because utilities cannot express them. All use `var(--bs-*)` tokens.

| Class | Why it needs custom CSS |
|---|---|
| `.text-label` | Multi-property (size + transform + tracking + weight) |
| `.inline-edit-input` | Stateful — border changes on `:focus` |
| `.public-browse-card` | `:hover` pseudo-class with transform |
| `.btn-ghost` | Custom variant using Bootstrap's `button-variant()` mixin |
| `.event-hero`, `.event-hero-overlay` | Complex layout + gradient (always-dark zone) |
| `.seatmap-*`, `.seatmap-fs-*` | Canvas layout (always-dark zone) |
| `.survey-builder-canvas`, `.question-card` | Structural layout components |
| `.badge-ai` | Shimmer animation via `::before`/`::after` pseudo-elements + `@keyframes` |
| `.narrative-tag`, `.narrative-tag-dot` | Inline-flex pill with colored dot at 12% opacity bg |
| `.narrative-cta` | Compact link with underline-on-hover and arrow affordance |

---

## 19. Component Patterns

Canonical decisions for recurring components. These are not suggestions — they are the resolved patterns for this project.

### Stats dashboard header

Use **separate individual stat cards**, not a single grouped container or segmented block.

Each card contains:
- `text-label` for the metric name
- A large `fw-bold` tabular number (`font-variant-numeric: tabular-nums`)
- A `small text-body-secondary` secondary line for context (e.g. `+12% vs last month`, `3 upcoming`)

```html
<div class="d-flex gap-3">
  <div class="stat-card flex-fill">
    <span class="text-label">Total revenue</span>
    <div class="fw-bold" style="font-size:1.5rem;font-variant-numeric:tabular-nums;">$48.2k</div>
    <div class="small text-body-secondary mt-1">+12% vs last month</div>
  </div>
  <div class="stat-card flex-fill">
    <span class="text-label">Tickets sold</span>
    <div class="fw-bold" style="font-size:1.5rem;font-variant-numeric:tabular-nums;">1,842</div>
    <div class="small text-body-secondary mt-1">+8% vs last month</div>
  </div>
  <div class="stat-card flex-fill">
    <span class="text-label">Active events</span>
    <div class="fw-bold" style="font-size:1.5rem;font-variant-numeric:tabular-nums;">7</div>
    <div class="small text-body-secondary mt-1">3 upcoming</div>
  </div>
</div>
```

### Data tables

Use a **traditional table with column headers**, not a card-stack list.

- `<th>` cells use `text-label` styling (uppercase, tracked, muted)
- Rows use `table-hover align-middle`
- Status communicated via dot pill
- Primary row action (`Manage`) is right-aligned in the last column as `btn-primary btn-sm`
- Secondary/inactive row action uses `btn-outline-secondary btn-sm`

```html
<div class="bg-white border rounded-3 overflow-hidden">
  <table class="table table-hover align-middle mb-0">
    <thead>
      <tr>
        <th>Event</th>
        <th>Date</th>
        <th>Status</th>
        <th>Sold</th>
        <th></th>
      </tr>
    </thead>
    <tbody>
      <tr>
        <td class="fw-medium">Summer Jazz Festival</td>
        <td class="text-body-secondary">21 Jun 2025</td>
        <td>
          <span class="d-inline-flex align-items-center gap-2 border rounded-pill px-2 py-1 small">
            <span class="rounded-circle bg-success flex-shrink-0" style="width:.5rem;height:.5rem;"></span>
            Published
          </span>
        </td>
        <td style="font-variant-numeric:tabular-nums;">342 / 500</td>
        <td class="text-end"><button class="btn btn-primary btn-sm">Manage</button></td>
      </tr>
    </tbody>
  </table>
</div>
```

### Confirmation modal

Surface the affected item in a highlighted info block between the header and the warning copy. Never use `window.confirm()`.

Structure:
1. **Header** — action title (`fw-bold`) + `"This action cannot be undone."` as a `small text-body-secondary` subtitle
2. **Info block** — `border rounded-2 bg-body-secondary px-3 py-2` showing the affected item name
3. **Warning copy** — `small text-body-secondary` explaining consequences
4. **Footer** — `btn-outline-secondary` (Cancel) left of `btn-danger` (destructive action)

```html
<div class="modal-header border-bottom pb-3">
  <div>
    <h2 class="fw-bold mb-1" style="font-size:1rem;">Delete event</h2>
    <p class="small text-body-secondary mb-0">This action cannot be undone.</p>
  </div>
</div>
<div class="modal-body py-3">
  <div class="border rounded-2 px-3 py-2 bg-body-secondary small mb-3">
    <span class="text-label" style="font-size:0.7rem;">Event</span>
    <span class="fw-medium text-body-emphasis">Summer Jazz Festival 2025</span>
  </div>
  <p class="small text-body-secondary mb-0">Deleting this event will permanently remove all 342 ticket sales and associated data.</p>
</div>
<div class="modal-footer border-top pt-3">
  <button class="btn btn-outline-secondary btn-sm">Cancel</button>
  <button class="btn btn-danger btn-sm">Delete event</button>
</div>
```

### Metric cards

Use the `MetricCard` component (`src/admin/ui.tsx`) for all dashboard KPI displays. Each card is a standalone `<article>` wrapped in a responsive grid column (`col-12 col-md-6 col-lg-4` by default).

Anatomy:
- **Title** — `text-label text-body-secondary` (uppercase metadata)
- **Subtitle** — optional top-right secondary text at `0.7rem` (e.g. "Last 30 days")
- **Value** — `fs-3 fw-medium tabular-nums` — large, prominent number
- **Hint** — optional `small text-body-secondary` context line, or a comparison dot pill
- **CTA** — optional `btn-outline-secondary fw-medium` link, pushed to card bottom via `mt-auto`
- **Spark bars** — optional inline bar chart via `sparkBars()`, rendered at card bottom

```html
<article class="col-12 col-md-6 col-lg-4">
  <div class="card border rounded-3 h-100">
    <div class="card-body p-3 d-flex flex-column">
      <div class="d-flex justify-content-between align-items-baseline">
        <span class="text-label text-body-secondary">Revenue</span>
        <span class="text-body-secondary" style="font-size: 0.7rem;">Last 30 days</span>
      </div>
      <div class="fs-3 fw-medium mt-1 tabular-nums">$12.4k</div>
      <!-- Comparison hint as dot pill -->
      <span class="d-inline-flex align-items-center gap-2 border rounded-pill px-2 py-1 mt-1 align-self-start"
            style="font-size: 0.75rem;">
        <span class="rounded-circle bg-success flex-shrink-0" style="width:.4rem;height:.4rem;"></span>
        +12% vs last period
      </span>
    </div>
  </div>
</article>
```

Rules:
- Always use `tabular-nums` on numeric values so digits align across cards.
- Comparison hints use the **dot pill** pattern (Section 5) — dot color is `bg-success` (increase), `bg-danger` (decrease), or omitted (unchanged).
- Grid defaults to 3 columns on `lg`, 2 on `md`, 1 on mobile. Override via `className` prop.
- Cards use `h-100` so rows stay aligned when hint/CTA presence varies.

### AI badge (`.badge-ai`)

The AI badge is used to label any feature powered by AI. It is a transparent badge with an animated shimmer border and text overlay — subtle and monochrome.

```html
<span class="badge badge-ai">AI</span>
```

Implementation (in `styles.scss`):
- **Shimmer border** — `::before` pseudo-element with a `linear-gradient(120deg, …)` masked to the border area using `mask-composite: exclude`. Animates `background-position` over 2.4s.
- **Shimmer text overlay** — `::after` pseudo-element with a translucent gradient sweep over the badge content.
- **Dark mode** — reduces shimmer intensity (`rgba(255,255,255,.25)` border, `.08` overlay) via `color-mode(dark, true)`.
- **Animation** — `@keyframes badge-shimmer` cycles `background-position` from `200% 0` to `-200% 0`.

Rules:
- Use `badge-ai` only — never color the badge with `text-bg-*` or semantic colors.
- Always pair with a visible text label (e.g. "Stadli narrative" next to `<span class="badge badge-ai">AI</span>`).
- The badge text is always "AI" — short, no icon.
- Do not apply shimmer effects to arbitrary elements. Shimmer is reserved for the AI badge and `.ai-builder-zone` containers (see below).

### AI builder zone (`.ai-builder-zone`)

Used to wrap AI-powered builder sections (e.g. the AI segment builder) so they are visually distinct from manual form sections. Applies a subtle shimmer border — the same gradient animation as `badge-ai` but slower (3s) and with a tighter highlight band.

```html
<div class="ai-builder-zone border rounded p-3">
  <div class="d-flex align-items-center gap-2 mb-2">
    <label class="form-label fw-medium mb-0">Describe your segment</label>
    <span class="badge badge-ai">AI</span>
  </div>
  <!-- builder content -->
</div>
```

Rules:
- Always include a `badge-ai` inside the zone so the AI label is explicit.
- Background is `var(--bs-body-bg)` — not `bg-body-tertiary` — to keep the zone visually lifted from surrounding form sections.
- Keep the `border rounded` classes alongside `ai-builder-zone` for the base shape.
- Dark mode reduces shimmer intensity automatically.

### AI insight cards (narrative cards)

Used on the dashboard to display AI-generated insights. Rendered as a 4-card row via `DashboardNarrative` (`src/admin/components/DashboardNarrative.tsx`).

**Section header pattern:**
```html
<div class="d-flex align-items-center gap-2 mb-2">
  <span class="fw-semibold small">Stadli narrative</span>
  <span class="badge badge-ai">AI</span>
  <span class="text-body-secondary" style="font-size: 0.6875rem;">Updated 2h ago</span>
</div>
```

**Card anatomy:**
- **Tag** — `.narrative-tag` pill with a `.narrative-tag-dot` colored dot. Background is the tag color at 12% opacity; text is the full semantic color. Tags map to stable colors via `TAG_LABEL_COLORS`.
- **Title** — `h6 fw-semibold` with `line-height: 1.4`.
- **Body** — `text-body-secondary small` with `line-height: 1.6`.
- **CTA** — `.narrative-cta` link (0.75rem, `color: var(--bs-body-color)`, underline on hover, right arrow `→`).
- **Actions** — like (`bi-heart` / `bi-heart-fill`) and dismiss (`bi-x`) buttons as `btn-ghost btn-sm` in `text-body-secondary`.

```html
<div class="col-12 col-md-6 col-lg-3">
  <div class="card border rounded-3 h-100">
    <div class="card-body p-3 d-flex flex-column">
      <div class="d-flex align-items-center gap-2 mb-2">
        <span class="narrative-tag" style="background: rgba(126,199,163,0.12); color: var(--bs-success);">
          <span class="narrative-tag-dot" style="background: var(--bs-success);"></span>
          Milestone
        </span>
        <span class="d-flex align-items-center gap-1 ms-auto">
          <button class="btn btn-ghost btn-sm p-0 lh-1 text-body-secondary" aria-label="Like insight">
            <i class="bi bi-heart" aria-hidden="true" style="font-size: 0.75rem;"></i>
          </button>
          <button class="btn btn-ghost btn-sm p-0 lh-1 text-body-secondary" aria-label="Dismiss and regenerate">
            <i class="bi bi-x" aria-hidden="true" style="font-size: 1rem;"></i>
          </button>
        </span>
      </div>
      <h6 class="fw-semibold mb-1" style="line-height: 1.4;">Revenue crossed $10k</h6>
      <p class="text-body-secondary small mb-0 flex-grow-1" style="line-height: 1.6;">
        Your total revenue hit a new milestone this week.
      </p>
      <div class="mt-2">
        <a href="/admin/orders" class="narrative-cta">View orders &rarr;</a>
      </div>
    </div>
  </div>
</div>
```

**Tag color palette** (inline styles, not utility classes — these use 12% opacity backgrounds):

| Tag label | Color key | Dot/text | Background |
|---|---|---|---|
| Key trend | `info` | `var(--bs-info)` | `rgba(124,176,206,0.12)` |
| Milestone / Quick win | `success` | `var(--bs-success)` | `rgba(126,199,163,0.12)` |
| Opportunity / Momentum | `purple` | `#8b6fc0` | `rgba(160,132,202,0.12)` |
| Tip | `teal` | `#3ea99f` | `rgba(102,192,181,0.12)` |
| Next step | `warning` | `var(--bs-warning)` | `rgba(210,176,107,0.12)` |

The existing `purple` and `teal` tag hues predate the spectrum palette (Section 2). When extending insight-card tags with new labels, **draw the new color from the spectrum** — e.g. `--spectrum-violet` for forecast tags, `--spectrum-rose` for fan-sentiment tags, `--spectrum-magenta` for outlier/anomaly tags — using the same 12%-opacity background pattern. Do not introduce new one-off hues outside the spectrum.

Rules:
- Insight cards always appear as a group of 4 in a `row g-2` with `col-lg-3` columns.
- Tag colors are stable per label — the same label always gets the same color via `TAG_LABEL_COLORS`.
- The legacy `purple` and `teal` tag hues are **insight-card-only**. The spectrum palette (Section 2) is the sanctioned source for any additional tag colors.
- Like/dismiss icons are the **only** exception to the "no icons" default — they are compact interactive controls in a space-constrained card header.

### AI content loading skeleton

When AI-generated content is loading, show a **shimmer skeleton** that matches the shape of the final content. This replaces Bootstrap's `.placeholder-glow` for AI-specific loading states.

```html
<!-- Skeleton block using tertiary-bg -->
<div style="background: var(--bs-tertiary-bg); border-radius: 0.25rem; height: 0.875rem; width: 50%; margin-bottom: 0.5rem;"></div>
<div style="background: var(--bs-tertiary-bg); border-radius: 0.25rem; height: 0.75rem; width: 90%; margin-bottom: 0.375rem;"></div>
<div style="background: var(--bs-tertiary-bg); border-radius: 0.25rem; height: 0.75rem; width: 100%; margin-bottom: 0.375rem;"></div>
<div style="background: var(--bs-tertiary-bg); border-radius: 0.25rem; height: 0.75rem; width: 70%;"></div>
```

Rules:
- Skeleton fills use `var(--bs-tertiary-bg)` — works in both light and dark mode.
- Match the approximate line count and widths of the real content (vary widths: 50%, 90%, 100%, 70%).
- First line is taller (0.875rem) to represent the title; subsequent lines are 0.75rem for body text.
- Wrap in the same card/grid structure as the final content so layout doesn't shift on load.
- Show the section header (with `badge-ai` and "Generating…" text) above the skeleton so users know what's loading.
- Use `fetch()` to replace the skeleton with real content — never full page reload.

---

## 20. Design Critique Guide

When reviewing or critiquing existing UI, evaluate in this order:

1. **Hierarchy** — Is the most important content prominent? Does size/weight/spacing create clear priority?
2. **Source discipline** — Are styles applied at the right level, or are there one-off overrides that should be SCSS variables or utilities?
3. **Spacing** — Does it follow the Bootstrap spacer scale? Are related items grouped? Is there rhythm?
4. **Typography** — Sentence case throughout? Correct type scale? No hardcoded font sizes?
5. **Color** — Token-based? No hardcoded hex? Semantic colors used functionally, not decoratively?
6. **Icons** — Does every icon pass the "would removing this make it worse?" test? Remove any that don't.
7. **Interactions** — Hover/focus/active states present? No color introduced on hover?
8. **Accessibility** — Semantic HTML? ARIA labels on icon-only controls? Sufficient contrast?

**Critique format:** Specific observation → which principle it violates → concrete fix. Prioritize high-impact issues first.

---

## 21. Quality Checklist

Run this before delivering any design change.

**Source & consistency:**
- [ ] Change applied at the highest applicable level (variable → utility → class → element)
- [ ] All instances of this pattern in the project are covered, not just this one
- [ ] No new one-off inline styles or single-use classes introduced

**Visual:**
- [ ] Clear hierarchy — most important elements are most prominent
- [ ] Spacing uses Bootstrap scale only — no raw px/rem in templates
- [ ] Typography uses utility classes — no hardcoded font-size or font-weight
- [ ] All colors use `var(--bs-*)` tokens — no hardcoded hex outside always-dark zone
- [ ] Spectrum colors (Section 2) used only in sanctioned contexts — data viz, narrative tags, marketing surfaces — and never on buttons, icons, form controls, or hover states
- [ ] Icons justified individually — every icon passes the "does text alone fail here?" test

**Interaction:**
- [ ] Interactive elements have hover/focus/active states (neutral only — no color introduced)
- [ ] Buttons are text-only and monochrome (black/white/ghost)
- [ ] One `btn-primary` per view section
- [ ] Forms use Bootstrap inline validation

**Light/dark:**
- [ ] Component tested and correct in both light and dark mode
- [ ] No `@media (prefers-color-scheme: dark)` — uses `color-mode(dark, true)` mixin

**Accessibility:**
- [ ] Semantic HTML elements used throughout
- [ ] All icons have `aria-hidden="true"`
- [ ] Icon-only controls have `aria-label` and tooltip
- [ ] Descriptive `alt` text on all content images
- [ ] Sufficient contrast (4.5:1 minimum for text)

---

## 22. Common Mistakes

| Wrong | Correct |
|---|---|
| `btn-primary` rendering as blue | Fix SCSS: `$primary: $gray-900` must be set before Bootstrap import |
| `btn-success` / `btn-info` / `btn-warning` on a button | Use `btn-primary` (black/white) or `btn-outline-secondary` |
| `btn-outline-primary` | Use `btn-outline-secondary` |
| Blue button anywhere in the UI | All buttons are black, white, gray, or ghost — never blue |
| `color: var(--spectrum-blue)` on an icon or admin button | Spectrum blue is data-viz / marketing only — never on icons or admin chrome |
| Hardcoded `#fe5619` in a chart config | Use `var(--spectrum-orange)` — always reference the token |
| Spectrum color on a focus ring, form border, or hover state | Interaction states stay neutral — spectrum colors never appear on hover/focus |
| Two spectrum colors layered on one element (e.g. violet text on green bg) | One spectrum color per atomic element; pair with neutral backgrounds |
| New insight-card tag with a one-off hex | Pick from `$spectrum-*` tokens using 12%-opacity background convention |
| `border: 1px solid #2d2f34` outside dark zone | `class="border"` or `var(--bs-border-color)` |
| `background: #fff` or `#1a1a1a` | `class="bg-body-tertiary"` |
| `box-shadow: …` anywhere | Remove. Use border + background contrast. |
| `color: var(--bs-info)` on an icon | Remove. Icons are always neutral gray. |
| `text-success` on icon inside badge | Icon inherits alert color — only ok inside `.alert-*` |
| Hover state that introduces a semantic color | Hover uses `bg-body-secondary` only |
| `font-size: 0.9375rem` in CSS | `class="small"` or `class="fs-6"` |
| `font-weight: 600` in CSS | `class="fw-semibold"` |
| `color: #6a6a6a` in CSS | `class="text-body-secondary"` |
| `border-radius: 12px` | `class="rounded-3"` |
| One-off custom class for a repeated style | Add to `$utilities` map |
| Dot pill text in ALL CAPS | Sentence case or title case only in pills |
| Dot pill text in a semantic color | Pill text always neutral; only the dot is colored |
| Pill background in a semantic color | Pill background always neutral |
| Icon on primary CTA | Text only |
| `Save Changes` (title case) | `Save changes` (sentence case) |
| `window.confirm()` for destructive action | Bootstrap `.modal` |
| `@media (prefers-color-scheme: dark)` | `@include color-mode(dark, true)` |
| `transition: all` | Explicit properties, 0.12s |
| Missing `aria-hidden="true"` on icon | Always add it |
| Empty `alt=""` on content image | Descriptive alt text |
| QR code on dark background | Wrap in `class="bg-white"` |
| Fixing a style on one element when the issue exists elsewhere | Fix at the SCSS variable, component variable, or utility level |

---

## Reference Files

- **`references/components.md`** — Canonical HTML templates for every major component.
- **`references/utilities.md`** — Custom utility classes defined in `styles.scss`.
