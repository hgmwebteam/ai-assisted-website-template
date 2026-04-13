# Mobile Guidelines

## Core Principles

1. **Mobile-first thinking** — design for the smallest screen, then add desktop enhancements
2. **Normal page scroll on mobile** — never lock the page with `h-screen overflow-hidden` on mobile
3. **No dropdowns for core controls** — use +/− counters, not `<select>` elements
4. **Touch targets ≥ 44px** — all buttons and interactive elements
5. **Full-screen drawers** — mobile menus and overlays should be `w-full`, not partial

## Layout Rules

### Page-Level Layout
```tsx
// WRONG — breaks mobile scroll:
<div className="flex flex-col h-screen overflow-hidden">

// CORRECT — normal scroll on mobile, app-shell only on desktop:
<div className="flex flex-col min-h-screen lg:h-screen lg:overflow-hidden">
```

### Scrollable Regions
```tsx
// WRONG — always locked:
<div className="flex-1 overflow-y-auto">

// CORRECT — scroll lock only on desktop:
<div className="lg:flex-1 lg:overflow-y-auto">
```

### Map Panel
The map is always hidden on mobile — only shown on `lg:` breakpoint:
```tsx
<div className="hidden lg:block flex-1 sticky top-0 h-full">
  <PropertyMap ... />
</div>
```

## Navigation

### Fixed Nav Clearance
Every page's first element must clear the 80px fixed nav:
```tsx
<div className="mt-[80px]">...</div>
// or
<section className="pt-[80px]">...</section>
```

### Mobile Drawer Requirements
- Must be portalled to `document.body` — see technical.md for portal pattern
- Wrap portal content in `<div className="md:hidden">` to hide on desktop
- Use `mounted` state to prevent SSR hydration mismatch
- Full width: `w-full` (not `w-[88vw]` or `max-w-[360px]`)
- Slide in from right: `transform: translateX(0/100%)`
- Full height: `top-0 right-0 bottom-0` (fixed)
- z-index: overlay `z-[90]`, drawer `z-[100]`

### Why Portal is Required
The nav has `backdrop-filter: blur(...)` which creates a new CSS stacking context. Any `position: fixed` descendant is positioned relative to the nav (80px tall) rather than the viewport. Portalling to `document.body` bypasses this.

## Touch & Interaction

### Guest Counter (not a select)
```tsx
<div className="flex items-center gap-2">
  <button
    type="button"
    onClick={() => setGuests(g => Math.max(1, g - 1))}
    className="w-5 h-5 rounded-full border border-[#C4A882] flex items-center justify-center cursor-pointer"
    aria-label="Fewer guests"
  >−</button>
  <span>{guests} Guest{guests !== 1 ? "s" : ""}</span>
  <button
    type="button"
    onClick={() => setGuests(g => Math.min(16, g + 1))}
    className="w-5 h-5 rounded-full border border-[#C4A882] flex items-center justify-center cursor-pointer"
    aria-label="More guests"
  >+</button>
</div>
```

### Horizontal Scroll (Category Tabs)
```tsx
<div className="overflow-x-auto scrollbar-hide -mx-4 px-4 sm:mx-0 sm:px-0">
  <div className="flex items-center min-w-max sm:min-w-0">
    {tabs.map(tab => <button key={tab} ...>{tab}</button>)}
  </div>
</div>
```

## Typography on Mobile

| Element | Mobile Size | Desktop Size |
|---------|-------------|--------------|
| Hero H1 | `text-[42px]` min (clamp) | up to `72px` |
| Section H2 | `text-[28px]` min | up to `44px` |
| Body text | `15px` | `15px` |
| UI / card text | `13px` | `13px` |
| Labels | `11px` | `11px` |

Never go below 13px for any readable text on mobile.

## Video (Hero)

iOS Safari requirements:
```tsx
<video
  muted          // Required for autoplay on iOS
  loop
  playsInline    // Required for iOS — prevents fullscreen takeover
  preload="none" // Don't load on mobile data
>
```

Never rely on the `poster` attribute when video has `opacity: 0`. Instead:
```tsx
// Separate always-visible poster image:
<img
  src={posterUrl}
  alt=""
  aria-hidden="true"
  fetchPriority="high"
  className="absolute inset-0 w-full h-full object-cover"
/>
// Video overlaid, fades in on canplay:
<video
  style={{ opacity: videoVisible ? 1 : 0 }}
  className="absolute inset-0 w-full h-full object-cover transition-opacity duration-700"
>
```

## Images on Mobile

```tsx
// Always specify sizes prop for responsive loading:
<Image
  src={url}
  alt="..."
  fill
  sizes="(max-width: 768px) 100vw, (max-width: 1200px) 50vw, 33vw"
  className="object-cover"
/>
```

## Grid Layouts

```tsx
// Cards: 1 col mobile, 2 col sm, 3 col md
<div className="grid grid-cols-1 sm:grid-cols-2 md:grid-cols-3 gap-5">

// Benefits: 1 col mobile, 3 col md
<div className="grid grid-cols-1 md:grid-cols-3 gap-6">

// 2-column split: stacked on mobile
<div className="grid grid-cols-1 md:grid-cols-2 gap-8 items-center">
```

## Spacing Adjustments

```tsx
// Section padding: less on mobile
<section className="py-16 sm:py-24">

// Horizontal padding: tighter on mobile
<div className="max-w-[1120px] mx-auto px-4 sm:px-6 lg:px-10">
```

## Search Bar on Mobile

On the home page hero, the search bar stacks vertically:
- Location (full width)
- Dates (full width)
- Guests (full width)
- Search button (full width, dark)

On the search results page, the search bar wraps:
- Uses `flex-wrap` to allow inputs to wrap
- Each input has `min-w-[160px]` floor

## Mobile Menu Image Cards

Property cards in the mobile drawer are full-width with tall images:
```tsx
<Link className="relative h-[180px] rounded-xl overflow-hidden block">
  <Image fill className="object-cover" />
  <div className="absolute inset-0 bg-gradient-to-t from-black/75 via-black/20 to-transparent" />
  <div className="absolute bottom-0 left-0 right-0 p-3">
    <p className="text-white font-[family-name:var(--font-playfair)] text-[16px]">{name}</p>
    <p className="text-white/70 text-[12px]">{guests} Guests · {beds} Bedrooms</p>
  </div>
</Link>
```
