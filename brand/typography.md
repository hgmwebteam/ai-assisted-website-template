# Typography System

## Fonts

### Display / Headings — Playfair Display
- **Source:** Google Fonts via `next/font/google`
- **Variable:** `--font-playfair`
- **Weights loaded:** 400 (normal), 600 (semibold)
- **Styles:** normal, italic
- **Usage in JSX:** `className="font-[family-name:var(--font-playfair)]"`
- **Use for:** H1, H2, H3, property names, hero text, pull quotes, testimonials, card titles

### Body — Inter
- **Source:** Google Fonts via `next/font/google`
- **Variable:** `--font-inter`
- **Weights loaded:** 400 (regular), 500 (medium)
- **Usage:** default body font (set on `<body>` in globals.css)
- **Use for:** body copy, labels, buttons, UI text, captions

## Type Scale (CSS Variables)

Defined in `globals.css :root`. Use via `style={{ fontSize: "var(--text-h1)" }}` or directly.

| Variable | Value | Usage |
|----------|-------|-------|
| `--text-hero` | `clamp(42px, 6vw, 72px)` | Hero headline |
| `--text-h1` | `clamp(36px, 5vw, 56px)` | Page H1 |
| `--text-h2` | `clamp(28px, 3.5vw, 44px)` | Section headings |
| `--text-h3` | `clamp(20px, 2.5vw, 28px)` | Sub-section headings |
| `--text-h4` | `18px` | Card titles |
| `--text-body-lg` | `16px` | Large body / nav links |
| `--text-body` | `15px` | Standard body text |
| `--text-sm` | `13px` | Secondary info, card metadata |
| `--text-xs` | `11px` | Labels, badges, timestamps |

## Common Tailwind Sizes (used directly)

These appear frequently in the codebase:

| Size | Context |
|------|---------|
| `text-[56px]` | Hero h1 on desktop |
| `text-[42px]` | Section h2 |
| `text-[32px]` | Sub-heading |
| `text-[19px]` | Property card title |
| `text-[16px]` | Nav links, buttons |
| `text-[15px]` | Body text |
| `text-[14px]` | Secondary text |
| `text-[13px]` | Small UI text (search bar, badges) |
| `text-[12px]` | Metadata, captions |
| `text-[11px]` | Section labels |

## Section Labels

Every major section starts with a small label above the heading:

```
font-size: 11px
font-weight: 700
letter-spacing: 0.22em (tracking-[0.22em])
text-transform: uppercase
color: #C4A882
margin-bottom: 16px
```

**CSS class:** `.section-label` (defined in globals.css)
**Examples:** "BOOK YOUR STAY", "WHY BOOK WITH US?", "FEATURED IN"

## Button Typography

All buttons use:
```
font-size: 12px
font-weight: 700
letter-spacing: 0.12em
text-transform: uppercase
```

## Line Heights

| Context | Value |
|---------|-------|
| Headings (h1–h6) | `1.15` |
| Body paragraphs | `1.8` |
| Card titles | `leading-snug` (1.375) |
| Tight UI text | `leading-none` or `1.2` |

## Letter Spacing

| Context | Value |
|---------|-------|
| Section labels | `tracking-[0.22em]` |
| Buttons | `tracking-[0.12em]` |
| Nav CTA | `tracking-wide` |
| Hero label | `tracking-[0.25em]` |
| Headings | `letter-spacing: -0.01em` |

## Anti-Aliasing

Set globally on `body`:
```css
-webkit-font-smoothing: antialiased;
-moz-osx-font-smoothing: grayscale;
```
