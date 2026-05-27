# PRD — The Local Folk: Shopify Development Studio Website
**Version:** 1.0  
**Date:** May 2026  
**Build target:** Replit (plain HTML / CSS / JS — no frameworks, no build tools)

---

## 1. Project Overview

A single-page marketing website for **The Local Folk**, an Australian Shopify development studio. The site exists to establish credibility through the weight of past work — a quiet, confident showcase that lets the client roster speak for itself.

The entire homepage is a **split-screen layout** that fills 100% of the viewport with no scrolling. The left panel is static and brand-forward. The right panel is an auto-scrolling rolodex of every store The Local Folk has built, with an industry tag that reveals on hover.

---

## 2. Goals

- Signal quality and specialisation immediately (Shopify dev, Australian, craft-focused)
- Let the volume and variety of clients sell the work without a portfolio grid
- Be dead simple — one page, no nav, no CMS, no dependencies to break
- Be easy for a non-developer to update (client list lives in a plain JS array at the top of the file)

---

## 3. Page Structure

The page has **one view**: a full-viewport, full-height split layout. No scroll. No other pages (for now).

```
┌─────────────────────────┬──────────────────────────────────────┐
│                         │                                      │
│     LEFT PANEL          │         RIGHT PANEL                  │
│     (static, ~38%)      │    (scrolling rolodex, ~62%)         │
│     orange background   │    off-white background              │
│                         │                                      │
└─────────────────────────┴──────────────────────────────────────┘
```

---

## 4. Left Panel — Spec

### Layout
- Fixed width: **38vw** (stays static while right side scrolls)
- Full viewport height
- Flex column layout with content distributed across top / middle / bottom thirds
- Padding: `48px` on all sides

### Background colour
- **`#E8704A`** (warm terracotta-orange, close to the reference image)

### Content (top to bottom)

**Eyebrow — top left**
```
Shopify Development Studio
```
- Font: Inter or DM Sans, weight 400, size 13px, tracking 0.05em
- Colour: rgba(255,255,255,0.75)

**Hero statement — vertical middle**
```
Shops built
to last.
```
- Font: **serif** — use "Playfair Display" (Google Font), weight 400, size clamp(36px, 3.5vw, 52px)
- Colour: `#FFFFFF`
- Line height: 1.1

**Description — bottom**
```
The Local Folk is an Australian Shopify
development studio. We build fast, considered,
conversion-focused stores for brands
that care about craft.
```
- Font: Inter or DM Sans, weight 400, size 14px, line height 1.6
- Colour: rgba(255,255,255,0.8)

### What does NOT appear on the left panel
- No navigation
- No CTA button (optional: can add a subtle "Get in touch → eliza@thelocalfolk.com.au" link at the very bottom in small text if desired — include as a commented-out block in the HTML so it's easy to toggle on)

---

## 5. Right Panel — Spec

### Layout
- Width: **62vw**
- Full viewport height
- **Overflow hidden** — clip the scrolling list at the panel edges
- Background: **`#F5F0E8`** (warm off-white / cream)
- Padding left: `48px`
- Padding right: `32px`

### The Rolodex / Scrolling List

The right panel contains a **continuously auto-scrolling vertical list** of client store names. The scroll is seamless (infinite loop) and slow — leisurely, not frantic.

**Typography**
- Font: **"Playfair Display"** (serif), weight 400
- Font size: `clamp(42px, 5vw, 72px)`
- Colour: `#1A1A1A`
- Line height: 1.25
- Each name sits on its own line

**Scroll behaviour**
- Pure CSS animation using `@keyframes` + `transform: translateY()`
- The list is **duplicated in the DOM** (original + clone) so the scroll loops seamlessly without a jump
- Animation duration: `40s` linear infinite (slow, confident pace)
- On hover over any name: `animation-play-state: paused` — the whole list freezes

**Each list item**
- `position: relative` so the industry pill can be absolutely positioned beside it
- On hover: show the industry pill (see §6)
- Cursor: default (it's not a link — unless you add one later)

---

## 6. Industry Pill — Spec

When a user hovers over a client name, a small **blue pill badge** appears to the right of the name (floating, not shifting the layout).

### Appearance
- Background: **`#5B8DEF`** (the blue from the reference)
- Text: white, Inter/DM Sans, weight 500, size 12px, tracking 0.03em
- Border radius: `999px` (fully rounded)
- Padding: `6px 14px`
- Positioned absolutely to the right of the name, vertically centred

### Behaviour
- Default state: `opacity: 0`, `transform: translateX(-6px)` 
- Hover state: `opacity: 1`, `transform: translateX(0)`
- Transition: `opacity 0.2s ease, transform 0.2s ease`
- The pill text = the industry category for that client (e.g. "Fashion", "Food & Bev", "Home & Living")

---

## 7. Client Data Structure

The client list should live in a **plain JS array at the top of the script**, making it easy to update without touching any layout code. Each entry has a `name` and `industry`.

```javascript
const clients = [
  { name: "Studio Name Here",    industry: "Fashion" },
  { name: "Another Brand",       industry: "Food & Bev" },
  { name: "Client Three",        industry: "Home & Living" },
  { name: "Brand Four",          industry: "Beauty" },
  { name: "Store Five",          industry: "Health & Wellness" },
  { name: "Client Six",          industry: "Outdoor & Sport" },
  { name: "Brand Seven",         industry: "Jewellery" },
  { name: "Store Eight",         industry: "Kids & Family" },
  { name: "Client Nine",         industry: "Art & Design" },
  { name: "Brand Ten",           industry: "Homewares" },
  // Add more clients here — the layout handles any number
];
```

> **Note for Replit:** The JS should read this array, render all items into the scroll list, then duplicate the rendered list (append a clone) to enable the seamless loop. Pill text is pulled from the `industry` field automatically.

---

## 8. Fonts

Load from Google Fonts. Add this `<link>` in the `<head>`:

```html
<link rel="preconnect" href="https://fonts.googleapis.com">
<link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
<link href="https://fonts.googleapis.com/css2?family=Playfair+Display:wght@400&family=DM+Sans:wght@400;500&display=swap" rel="stylesheet">
```

- **Serif (Playfair Display):** hero statement on left, all client names on right
- **Sans (DM Sans):** eyebrow, description, industry pills

---

## 9. Colours Reference

| Token | Hex | Usage |
|---|---|---|
| Orange | `#E8704A` | Left panel background |
| Off-white | `#F5F0E8` | Right panel background |
| Dark | `#1A1A1A` | Client names |
| Blue | `#5B8DEF` | Industry pill background |
| White | `#FFFFFF` | Left panel text |
| White muted | `rgba(255,255,255,0.75)` | Eyebrow + description |
| Pill text | `#FFFFFF` | Industry label |

---

## 10. Responsive Behaviour

| Breakpoint | Behaviour |
|---|---|
| Desktop (>900px) | Side-by-side split as described |
| Tablet (600–900px) | Stack vertically: left panel on top (~40vh), right panel below (~60vh). Right panel scrolls normally. |
| Mobile (<600px) | Left panel becomes a header block. Right panel fills the rest of the screen. Font sizes reduce via `clamp()`. |

---

## 11. File Structure

Everything in **one file** for Replit simplicity:

```
index.html   ← all HTML, embedded <style> block, embedded <script> block
```

No external JS libraries. No CSS frameworks. No package.json.

---

## 12. Accessibility Notes

- Left panel text must meet WCAG AA contrast on the orange background — white text at these sizes passes
- The scrolling list should respect `prefers-reduced-motion`: if set, replace the auto-scroll with a static list that the user can scroll manually
- Industry pills must be readable — white on `#5B8DEF` passes AA at 12px bold

```css
@media (prefers-reduced-motion: reduce) {
  .scroll-track {
    animation: none;
    overflow-y: auto;
  }
}
```

---

## 13. Exact Prompt to Paste into Replit

Use this as your starting prompt in Replit AI:

---

> Build a single-page website in one HTML file (no frameworks, no build tools, plain HTML + CSS + JS).
>
> **Layout:** Full-viewport split screen. No page scroll.
> - Left panel: 38vw wide, full height, background `#E8704A`. Static (no scroll).
> - Right panel: 62vw wide, full height, background `#F5F0E8`. Contains an auto-scrolling list.
>
> **Left panel content (top to bottom):**
> - Top-left: small text "Shopify Development Studio" — DM Sans 400, 13px, rgba(255,255,255,0.75), letter-spacing 0.05em
> - Middle: large serif headline "Shops built\nto last." — Playfair Display 400, clamp(36px,3.5vw,52px), white, line-height 1.1
> - Bottom: body copy "The Local Folk is an Australian Shopify development studio. We build fast, considered, conversion-focused stores for brands that care about craft." — DM Sans 400, 14px, rgba(255,255,255,0.8), line-height 1.6
>
> **Right panel — scrolling rolodex:**
> - A vertically scrolling list of client names in Playfair Display, ~clamp(42px,5vw,72px), colour #1A1A1A
> - Scroll is a seamless infinite loop: duplicate the list in the DOM, animate with CSS `transform: translateY` over 40s linear infinite
> - On hover over the entire right panel: pause the animation (`animation-play-state: paused`)
> - Each list item has a hidden blue pill badge (background `#5B8DEF`, white text, DM Sans 500, 12px, border-radius 999px, padding 6px 14px). The pill is positioned absolutely to the right of the name, vertically centred. On hover over that specific list item: fade the pill in (opacity 0→1, translateX -6px→0, transition 0.2s ease)
>
> **Client data:** Store in a JS array at the top of the script:
> ```js
> const clients = [
>   { name: "Studio Name Here", industry: "Fashion" },
>   { name: "Another Brand", industry: "Food & Bev" },
>   // etc.
> ];
> ```
> Render these into the list dynamically. Include the pill for each item using the `industry` field.
>
> **Fonts:** Load Playfair Display (400) and DM Sans (400, 500) from Google Fonts.
>
> **Responsive:** Below 900px, stack the panels vertically (left on top ~40vh, right below). Below 600px, reduce font sizes further.
>
> **Reduced motion:** If `prefers-reduced-motion` is set, disable the auto-scroll animation.
>
> Deliver everything in a single `index.html` file.

---

## 14. Things to Update Before Going Live

1. **Replace placeholder client names** in the JS array with real store names + their industries
2. **Check left panel copy** — adjust the description text to match current positioning
3. **Optional contact link** — uncomment the email link at the bottom of the left panel if desired
4. **Favicon** — add a simple favicon (can be a coloured square with "TLF" initials)
5. **Page title** — set `<title>The Local Folk — Shopify Development Studio</title>`
6. **Deploy** — Replit's built-in hosting works for this; or export and deploy to Netlify via drag-and-drop

---

*PRD written for The Local Folk by Claude · May 2026*
