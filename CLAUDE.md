# Direct Booking Website — AI Development Guide

This document is the single source of truth for rebuilding or extending Direct Booking Websites. Read it fully before writing any code.

---

## Project Overview

**[ENTER BUSINESS NAME]** are luxury vacation rental companies managing properties in [ENTER LOCATION HERE]. The site is the primary booking channel, emphasising direct bookings over Airbnb/VRBO to save guests 10–15% in fees.

**Live site:** [ENTER LIVE SITE]
**Tagline:** "[ENTER TAGLINE]"
**Tone:** [ENTER TONE] example Premium, warm, editorial — never generic or corporate

---

## Tech Stack

| Layer | Choice |
|-------|--------|
| Framework | Next.js 16 (App Router) |
| Language | TypeScript (strict) |
| Styling | Tailwind CSS v4 (no config file — uses `@tailwindcss/postcss`) |
| UI Icons | lucide-react |
| Maps | Leaflet + react-leaflet (dynamic import, SSR disabled) |
| Fonts | Google Fonts via `next/font` — Playfair Display + Inter |
| Images | `next/image` with `unoptimized` for external CDN images |
| Deployment | Netlify |
| Property data | [ENTER PROPERTY DATA]Guesty API (via `/api/guesty/listings`) |
| Forms | LeadConnector (embedded iframe) |
| Widgets | Elfsight (script loaded via `next/script` lazyOnload) |

### Key Config Files
- `next.config.ts` — image remote patterns, AVIF/WebP formats, 1-year CDN cache
- `postcss.config.mjs` — `@tailwindcss/postcss` plugin only
- `src/app/globals.css` — all CSS variables, design tokens, utility classes

### Allowed Image Hostnames (next.config.ts)
- `images.unsplash.com`
- `[ENTER LIVE SITE]`
- `assets.guesty.com/**`
- `res.cloudinary.com/**`

---

## Project Structure

```
src/
├── app/
│   ├── layout.tsx              # Root layout: fonts, metadata, Elfsight script
│   ├── page.tsx                # Home page (component composition only)
│   ├── globals.css             # Design tokens, CSS variables, utility classes
│   ├── about/page.tsx
│   ├── contact/page.tsx
│   ├── guidebook/page.tsx
│   ├── press/page.tsx
│   ├── property/[slug]/page.tsx
│   ├── search/
│   │   ├── page.tsx            # Search results + map layout
│   │   └── PropertyMap.tsx     # Leaflet map component (client-only)
│   ├── services/page.tsx
│   ├── stays/page.tsx
│   ├── checkout/               # Booking checkout flow
│   └── api/
│       └── guesty/listings/route.ts  # Guesty API proxy + cache
├── components/
│   ├── Nav.tsx                 # Fixed nav, mega menu, mobile drawer (portal)
│   ├── Hero.tsx                # Full-screen video hero with search bar
│   ├── Footer.tsx
│   ├── AboutCohost.tsx
│   ├── Benefits.tsx
│   ├── FeaturedStays.tsx
│   ├── Location.tsx
│   ├── LocalFavorites.tsx
│   ├── Testimonials.tsx
│   ├── Instagram.tsx
│   ├── SignUp.tsx
│   ├── ThankYou.tsx
│   ├── CheckoutFlow.tsx
│   └── ContactForm.tsx
├── data/
│   └── properties.ts           # Static property data (slug, name, images, etc.)
└── lib/
    └── guesty.ts               # Guesty API client + in-memory cache (1 hour)
```

---

## Design System

See `brand/` folder for full details. Key summary:

### Color Palette
```
--brown-dark:  #1C1410   (primary text, dark backgrounds)
--brown-mid:   #7B5B3A   (secondary text, icons, borders)
--brown-light: #C4A882   (gold accent, labels, borders)
--bg-warm:     #F7F4EF   (page background, card backgrounds)
--bg-warmest:  #FAF8F5   (lightest warm white)
--border-warm: #EDE8DF   (subtle dividers)
```

In Tailwind, use these as arbitrary values: `bg-[#F7F4EF]`, `text-[#1C1410]`, etc.

### Typography
- **Serif/Display:** `font-[family-name:var(--font-playfair)]` — headings, property names, quotes
- **Sans:** default body font (Inter via CSS variable `--font-inter`)
- **Type scale:** defined as CSS variables (`--text-hero` through `--text-xs`), use `style={{ fontSize: "var(--text-h1)" }}`

### Spacing
- Max content width: `max-w-[1120px] mx-auto`
- Section padding: `py-24` (desktop), `py-16` (mobile)
- Horizontal padding: `px-6` to `px-10`

### Buttons
Use global CSS classes:
- `.btn-primary` — filled brown (`#7B5B3A`), uppercase, 12px, letter-spacing
- `.btn-outline` — outlined brown
- `.btn-ghost-white` — transparent with white border (for dark backgrounds)

Or use Tailwind equivalents matching the same sizing/spacing.

### Cards
Use global `.card` class or replicate: `bg-white rounded-2xl border border-[#EDE8DF] overflow-hidden shadow-sm hover:shadow-xl transition-shadow`

---

## Component Patterns

### Nav (Fixed, Glassmorphism)
- `position: fixed; top: 0; z-index: 50; height: 80px`
- Background: `rgba(20,12,6,0.85)` with `backdrop-filter: blur(40px) saturate(180%)`
- Changes opacity on scroll (after 60px)
- Desktop: logo left, nav links center, "Book Now" button right
- Mobile: hamburger button → full-screen drawer via `createPortal` to `document.body`
- **CRITICAL:** Mobile drawer MUST use `createPortal` to `document.body` — `backdrop-filter` on the nav creates a stacking context that clips `position:fixed` children

### Mobile Drawer
- Rendered via `ReactDOM.createPortal` into `document.body`
- Wrapped in `<div className="md:hidden">` to hide on desktop
- Full width (`w-full`), slides from right with `translateX` transition
- Contains: logo + close button, featured property cards (full-width image cards with gradient overlay), "View All Listings" button, quick nav links
- State: `mounted` flag (set in `useEffect`) prevents SSR hydration mismatch

### Hero
- Full-screen (`h-screen`), video background with lazy loading
- Poster image shown immediately via separate `<img fetchPriority="high">` (NOT the video `poster` attribute — that's hidden when video opacity is 0)
- Video fades in on `onCanPlay` event
- Video loaded via `requestIdleCallback` (or `setTimeout` fallback after 2s)
- Centered content: label + h1 + search bar
- Search bar: location text input + date picker dropdown + guests +/- counter + Search button
- All search state lifted; on submit pushes to `/search?location=...&checkIn=...&checkOut=...&guests=...`

### Search Page (`/search`)
- **Mobile:** normal page scroll (`min-h-screen`) — no overflow locks
- **Desktop (lg+):** app-shell layout (`h-screen overflow-hidden`) with sticky map panel
- Left panel: search bar + category filter tabs + results count/sort + scrollable card grid
- Right panel (desktop only): Leaflet map, sticky
- Map wrapped in `MapErrorBoundary` (class component) — prevents Leaflet errors from killing React event delegation
- Listings fetched from `/api/guesty/listings`, cached in localStorage (1 hour, stale-while-revalidate)
- Location input uses `<datalist>` for suggestions

### Property Cards
- Carousel with prev/next arrows (shown on hover) + dot pagination
- Up to 8 images from Guesty
- Shows: name (Playfair), price/night, guest count, bedrooms, bathrooms

---

## Functionality

### Search & Filtering
- **Location:** text input with datalist suggestions: Joshua Tree, Pioneertown, Twentynine Palms, Yucca Valley
- **Dates:** dropdown with two date inputs (check-in / check-out), closes on check-out selection
- **Guests:** +/− counter buttons (min 1, max 16), never a `<select>` dropdown
- **Category tabs:** Featured, Romantic Retreat, Pool, Boulders, Unique, Luxury — filter by Guesty listing tags
- **Sort:** Featured, Price Low→High, Price High→Low, Most Guests

### Guesty API Integration
- Route: `GET /api/guesty/listings`
- In-memory cache: 1 hour (`globalThis`)
- CDN cache: `Cache-Control: public, s-maxage=3600, stale-while-revalidate=300`
- Cache bust: `?bust=1`
- Property name cleaning: strips everything up to and including the last ` - ` (e.g. "ALB · Cobalt - Alba" → "Alba")
- Payload trimmed to card fields only: id, title, nickname, accommodates, bedrooms, bathrooms, propertyType, address (city/lat/lng), prices (basePrice/currency), first 8 pictures, tags, first 30 amenities

### Map (Leaflet)
- Dynamic import with `ssr: false`
- Tile layer: CartoCDN light (`https://{s}.basemaps.cartocdn.com/light_all/{z}/{x}/{y}{r}.png`)
- Custom div icons (price bubble pins with downward caret)
- Clustering at low zoom levels (threshold-based, not library-based)
- City-level aggregate pins for listings without GPS coordinates
- Hover state updates only the two affected markers (no full rebuild)
- Full rebuild only on listings change or zoom

### Property Detail (`/property/[slug]`)
- Slug is the Guesty listing `_id`
- Full listing data fetched from Guesty API

---

## Mobile Guidelines

1. **No `overflow-hidden` on full-page containers at mobile** — use responsive prefix: `lg:overflow-hidden`
2. **`backdrop-filter` on fixed parents clips fixed children** — always use `createPortal` for overlays/drawers inside elements with backdrop-filter
3. **Touch targets:** minimum 44×44px for all interactive elements
4. **Guests control:** always use +/− counter, never a `<select>` (hard to style, inconsistent across devices)
5. **Nav height:** 80px — all page content needs `mt-[80px]` or `pt-[80px]` to clear the fixed nav
6. **Images:** always use `next/image` with `sizes` prop for responsive loading; use `unoptimized` for Guesty/Cloudinary CDN URLs (they handle their own optimisation)
7. **Horizontal scroll tabs:** wrap in `overflow-x-auto scrollbar-hide` with `-mx-4 px-4` to allow edge-to-edge scroll
8. **App-shell layout:** only lock to `h-screen` on `lg:` breakpoint where map panel is shown
9. **Font sizes:** minimum 13px for body text on mobile, never smaller
10. **Video:** always `playsInline muted loop` — required for iOS autoplay

---

## Performance Rules

- Use `requestIdleCallback` to defer non-critical loads (hero video)
- Leaflet and heavy map code: always `dynamic(() => import(...), { ssr: false })`
- Images: AVIF → WebP fallback via `next/image` formats config
- Property listings: stale-while-revalidate from localStorage before API call
- Scripts: Elfsight and LeadConnector always `strategy="lazyOnload"`
- Hero poster: separate `<img fetchPriority="high">` — do NOT rely on video `poster` attribute (hidden when opacity:0)

---

## Content & Copy Tone

- Warm, premium, personal — not corporate
- Desert lifestyle focus: Joshua Tree, Pioneertown, Yucca Valley, Twentynine Palms
- CTAs: "Book Now", "View All Listings", "Explore Stays", "Sign Up & Get 10% OFF"
- Section labels: ALL CAPS, 11px, letter-spacing 0.22em, color `#C4A882`
- Headlines: Playfair Display, sentence case, not all caps

---

## Third-Party Services

| Service | Purpose | Integration |
|---------|---------|-------------|
| Guesty | Property management / live listings | REST API, server-side proxy route |
| LeadConnector | Email capture form | Embedded iframe + `form_embed.js` script |
| Elfsight | Instagram feed widget | `platform.js` script |
| Netlify | Hosting + edge CDN | Deploy from main branch |
| CartoCDN | Map tiles | Leaflet tile layer |
| Cloudinary (via Guesty) | Property photos CDN | `res.cloudinary.com` |

---

## Do Not Do

- Do not add `<select>` for guest count — always use +/− counter buttons
- Do not render mobile drawer inside a `backdrop-filter` parent — use `createPortal`
- Do not lock mobile pages to `h-screen overflow-hidden` — only on `lg:` breakpoint
- Do not use `opacity: 0` on `<video>` and expect `poster` to show — use a separate `<img>`
- Do not use Inter, Arial, Roboto, or system fonts for headings — use Playfair Display
- Do not use purple gradients, blue accents, or generic "AI" aesthetics
- Do not add extra features or abstractions not explicitly requested
- Do not commit `.env` files or API keys
