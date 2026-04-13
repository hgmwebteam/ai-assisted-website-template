# Design Tokens

All tokens are defined in `src/app/globals.css` under `:root` and `@theme inline`.

## Spacing

```css
--section-y:    96px;   /* Section vertical padding (desktop) */
--section-y-sm: 64px;   /* Section vertical padding (mobile) */
--card-pad:     28px;   /* Card internal padding */
```

Tailwind equivalents used in practice:
- Section: `py-24` (96px) desktop, `py-16` (64px) mobile
- Content max-width: `max-w-[1120px] mx-auto`
- Horizontal padding: `px-6` to `px-10`

## Border Radius

```css
--radius-card:  16px;   /* Cards, modals, dropdowns */
--radius-btn:   6px;    /* Buttons */
```

Tailwind equivalents:
- Cards: `rounded-2xl` (16px)
- Pill buttons/tags: `rounded-full`
- Small UI elements: `rounded-xl` (12px) or `rounded-lg` (8px)

## Shadows

| Use | Value |
|-----|-------|
| Default card | `shadow-sm` |
| Hovered card | `shadow-xl` |
| Map pin | `0 2px 8px rgba(0,0,0,0.15)` |
| Map pin (hovered) | `0 4px 16px rgba(28,20,16,0.40)` |
| Dropdown/modal | `shadow-2xl` |
| Search bar | `shadow-lg` |

## Z-Index Scale

| Layer | Value | Element |
|-------|-------|---------|
| Nav | `z-50` | Fixed navigation |
| Mega menu | `z-40` | Desktop dropdown |
| Dropdown | `z-50` | Small dropdowns |
| Mobile overlay | `z-[90]` | Dark backdrop |
| Mobile drawer | `z-[100]` | Slide-in panel |

## Transitions

| Type | Value |
|------|-------|
| Color/bg | `transition-colors duration-200` |
| Shadow | `transition-shadow duration-300` |
| Transform | `transition-transform duration-300` |
| Opacity | `transition-opacity duration-300` |
| Video fade-in | `transition-opacity duration-700` |
| Nav drawer | `transition-transform duration-300 ease-in-out` |

## Glassmorphism (Nav)

```css
background: rgba(20, 12, 6, 0.85);
backdrop-filter: blur(40px) saturate(180%);
-webkit-backdrop-filter: blur(40px) saturate(180%);
```

On scroll:
```css
background: rgba(0, 0, 0, 0.6);
border-bottom: 1px solid rgba(255,255,255,0.18);
box-shadow: 0 4px 32px rgba(0,0,0,0.18), inset 0 1px 0 rgba(255,255,255,0.18);
```

## Image Aspect Ratios

| Context | Ratio |
|---------|-------|
| Property card hero | `h-[220px]` (fixed height) |
| Mobile menu property card | `h-[180px]` |
| Desktop mega menu card | `min-h-[190px]` |
| Tour strip (mobile) | `1:1` |
| Tour strip (sm+) | `4:3` |

## Breakpoints (Tailwind v4 defaults)

| Prefix | Width |
|--------|-------|
| `sm:` | 640px |
| `md:` | 768px |
| `lg:` | 1024px |
| `xl:` | 1280px |
| `2xl:` | 1536px |

Key breakpoints in use:
- Mobile drawer hidden: `md:hidden`
- Desktop nav shown: `hidden md:flex`
- Map shown: `hidden lg:block`
- 2-column grid: `sm:grid-cols-2`
- 3-column grid: `md:grid-cols-3`
