# Technical Guidelines

## Setup

```bash
# Install
npm install

# Dev server
npm run dev

# Build
npm run build

# Type check
npx tsc --noEmit
```

## Next.js App Router Rules

- All pages are in `src/app/`
- All pages are **Server Components** by default
- Add `"use client"` only when using: useState, useEffect, useRef, event handlers, browser APIs
- Use `dynamic(() => import(...), { ssr: false })` for browser-only libraries (Leaflet, etc.)
- Metadata exported from page files (`export const metadata: Metadata = {...}`)

## TypeScript

- Strict mode enabled
- Never use `any` without `// eslint-disable-next-line @typescript-eslint/no-explicit-any`
- Prefer `type` over `interface` for component props
- Guesty types defined in `src/lib/guesty.ts` — always import `GuestyListingFull` from there

## Tailwind CSS v4

**Important differences from v3:**
- No `tailwind.config.ts` — configuration is in `globals.css` via `@theme inline`
- Custom colors defined as CSS variables in `@theme inline`, not in a config object
- PostCSS plugin: `@tailwindcss/postcss` (not `tailwindcss` directly)
- Arbitrary values work the same: `bg-[#F7F4EF]`, `text-[13px]`, `tracking-[0.22em]`
- Custom utilities defined in CSS, not in plugins

## Image Handling

```tsx
// For static or Next.js-optimised images:
<Image src={url} alt="..." fill className="object-cover" />

// For Guesty/Cloudinary CDN images (they handle optimisation themselves):
<Image src={url} alt="..." fill className="object-cover" unoptimized />

// Always include sizes prop for non-fill images:
<Image src={url} alt="..." width={800} height={600}
  sizes="(max-width: 768px) 100vw, 50vw" />

// For external images NOT in next.config.ts remotePatterns, use <img>:
// eslint-disable-next-line @next/next/no-img-element
<img src={url} alt="" fetchPriority="high" className="..." />
```

## Fonts

```tsx
// In layout.tsx:
import { Playfair_Display, Inter } from "next/font/google";

const playfair = Playfair_Display({
  variable: "--font-playfair",
  subsets: ["latin"],
  weight: ["400", "600"],
  style: ["normal", "italic"],
  display: "swap",
});

// In JSX for Playfair headings:
<h1 className="font-[family-name:var(--font-playfair)]">...</h1>
```

## API Route: Guesty Listings

`GET /api/guesty/listings`
- Returns trimmed listing objects (card-view fields only)
- In-memory cache: 1 hour (shared across requests via `globalThis`)
- CDN cache: `Cache-Control: public, s-maxage=3600, stale-while-revalidate=300`
- Bust cache: `?bust=1`
- Client-side stale-while-revalidate: stored in `localStorage` with timestamp

## Error Handling

- Map errors: always wrap `<PropertyMap>` in `<MapErrorBoundary>` (class component)
- API errors: return `NextResponse.json({ error: message }, { status: 502 })`
- Image errors: `<ImageOff>` lucide icon as fallback
- Empty states: friendly copy with a "Clear filters" / "Start Over" button

## Environment Variables

Store in `.env.local` (never commit):
```
GUESTY_CLIENT_ID=...
GUESTY_CLIENT_SECRET=...
```

Access server-side only (no `NEXT_PUBLIC_` prefix — these are API credentials).

## Scripts

Third-party scripts must use `next/script` with appropriate strategy:
```tsx
// Non-critical widgets and forms:
<Script src="https://elfsightcdn.com/platform.js" strategy="lazyOnload" />
<Script src="https://link.msgsndr.com/js/form_embed.js" strategy="lazyOnload" />
```

## Leaflet Map

```tsx
// Always dynamic import — Leaflet requires `window`
const PropertyMap = dynamic(() => import("./PropertyMap"), { ssr: false });

// In PropertyMap.tsx:
"use client";
import "leaflet/dist/leaflet.css";
// Delete default icon URL getter (required for webpack):
delete (L.Icon.Default.prototype as any)._getIconUrl;

// Tile layer (CartoCDN light):
L.tileLayer(
  "https://{s}.basemaps.cartocdn.com/light_all/{z}/{x}/{y}{r}.png",
  { attribution: "© OpenStreetMap © CARTO", maxZoom: 18 }
).addTo(map);
```

## Portal Pattern (Mobile Drawer)

```tsx
import { createPortal } from "react-dom";

// In component:
const [mounted, setMounted] = useState(false);
useEffect(() => { setMounted(true); }, []);

// In JSX:
{mounted && createPortal(
  <div className="md:hidden">
    {/* overlay + drawer */}
  </div>,
  document.body
)}
```

Required because `backdrop-filter` on the nav creates a stacking context that clips `position: fixed` children — portalling to body bypasses this.
