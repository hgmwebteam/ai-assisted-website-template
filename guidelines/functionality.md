# Functionality Guidelines

## Navigation

### Desktop Nav
- Fixed, full-width, 80px height
- Logo (SVG) left-aligned
- "Stays" mega-menu with hover delay (120ms close timer) showing location pills + property cards
- Dropdown menus for "Things To Do", "For Hosts", "About" (click-activated, close on outside click)
- "Book Now" pill button right-aligned → `/search`
- Scroll listener: style changes after 60px using `requestAnimationFrame`

### Mega Menu (Stays)
- Triggered by mouse hover with 120ms close delay
- Fixed panel below nav (`top: 80px`), full viewport width
- Left: location filter pills (Featured Listings, Joshua Tree, Pioneertown, Yucca Valley, Twentynine Palms, Palm Springs, View All)
- Right: 3 property cards (image + gradient overlay + name/guests)
- Bottom: "Claim 10% OFF" and "Join The Cohost" CTAs
- Properties sourced from `src/data/properties.ts` (static data, 3 featured slugs)

### Mobile Drawer
- Hamburger (3 bars) top-right of nav — `md:hidden`
- Full-screen white panel slides from right (`translateX`)
- Header: logo (dark-filtered) + circular close button
- Featured properties: full-width image cards with gradient + text overlay (h-[180px])
- "View All Listings" → `/stays`
- Quick links: Home, Guidebook, Services, About, Press, Contact
- Portalled to `document.body` with `md:hidden` wrapper

## Search Flow

### Home Page Search Bar
Layout (stacked cards on mobile, inline on desktop):
1. **Location** — text input with `<datalist>` suggestions
2. **Dates** — button that opens dropdown with two date pickers
3. **Guests** — +/− counter (min 1, max 16)
4. **Search button** → pushes to `/search?location=...&checkIn=...&checkOut=...&guests=...`

### Search Results Page (`/search`)
- Reads params from URL on mount via `useSearchParams()`
- Fetches `/api/guesty/listings` (stale-while-revalidate from localStorage)
- Filters: location (fuzzy city match), guests (≥ count), category (tag match)
- Sort: featured / price asc / price desc / most guests
- Category tabs: Featured, Romantic Retreat, Pool, Boulders, Unique, Luxury
- Each category maps to a Guesty tag string
- "Start Over" resets all state and URL params

### Location Suggestions
Always include these 4 options in `<datalist>`:
```
Joshua Tree
Pioneertown
Twentynine Palms
Yucca Valley
```

### Date Picker
- Dropdown anchored below the dates button
- Closes on outside click (mousedown listener)
- Check-in: `min={today}`
- Check-out: `min={checkIn + 1 day}`, auto-closes on selection
- If checkIn changes to be ≥ checkOut, clear checkOut

## Property Cards

### Image Carousel
- Up to 8 images from Guesty `pictures` array
- Prev/next arrow buttons: hidden by default, shown on `group-hover`
- Dot pagination (max 5 dots shown), click to jump
- Current index in local state per card

### Card Data Displayed
- Property name (nickname preferred over title)
- Starting price / night (if available)
- Guest capacity (Users icon)
- Bedrooms (BedDouble icon)
- Bathrooms (Bath icon)
- Link to `/property/${listing._id}`

### Highlighted State
- Card hovering sets `hoveredId` in parent
- `hoveredId` passed to map to highlight the pin
- Highlighted card gets `shadow-xl` instead of `shadow-sm`

## Map Functionality

### Pin Types
1. **Single pin** — individual property with GPS: shows `$price` or `–`
2. **Cluster pin** — multiple nearby properties (zoom-based): shows `$minPrice+` and count
3. **City pin** — properties without GPS coords: shows city name + count

### Clustering Logic
Threshold distance based on zoom:
- zoom ≤ 10: 0.05° (~5km)
- zoom ≤ 11: 0.02°
- zoom ≤ 12: 0.008°
- zoom ≥ 13: no clustering (individual pins)

### Hover Interaction
- Card hover → update map pin to highlighted state (dark bg)
- Only two markers updated (previous + new) — no full rebuild
- Map hover → card scroll not implemented (map is read-only hover target)

### Zoom
- Initial zoom: 10, center: [34.13, -116.31]
- On zoom end: full marker rebuild (re-clusters at new threshold)
- Cluster pin click: `map.setView([lat, lng], zoom + 2)`
- Single pin click: navigate to property page

## Booking / Checkout

### Entry Points
- "Book Now" nav button → `/search`
- Property card → `/property/[id]`
- Property page → `/checkout` (with dates/guests)

### Checkout Flow (`/checkout`)
- Multi-step flow component: `CheckoutFlow.tsx`
- Requires: property ID, check-in, check-out, guest count

## Email Capture (Sign Up)

- Section at bottom of home page
- LeadConnector embedded iframe form
- Offer: 10% OFF first stay
- Form ID: `JMOPYLHw05kL4wVTv6wQ`
- Script: `https://link.msgsndr.com/js/form_embed.js`

## Guesty Property Name Cleaning

Property names in Guesty follow a pattern like "ALB · Cobalt - Alba". The display name is everything after the last ` - `:

```typescript
function cleanName(raw: string): string {
  const idx = raw.lastIndexOf(" - ");
  return idx !== -1 ? raw.slice(idx + 3).trim() : raw.trim();
}
```

## Availability Search (`AvailabilitySearch.tsx`)

Standalone component for embedding a search form with date + guests on property detail or other pages.

## City Key Mapping

Properties without precise GPS are assigned to cities by name matching:

Update these for [ENTER BUSINESS NAME] only if the PMS is GUESTY
```typescript
function cityKey(l) {
  const c = l.address?.city ?? "";
  if (/pioneer/i.test(c)) return "Pioneertown";
  if (/yucca/i.test(c)) return "Yucca Valley";
  if (/joshua/i.test(c)) return "Joshua Tree";
  if (/twentynine|29 palms/i.test(c)) return "Twentynine Palms";
  if (/morongo/i.test(c)) return "Morongo Valley";
  return c || "Hi-Desert";
}
```

City GPS fallback coordinates:
```
Joshua Tree:      [34.1342, -116.3130]
Pioneertown:      [34.1650, -116.5100]
Yucca Valley:     [34.1144, -116.4322]
Twentynine Palms: [34.1356, -116.0542]
Morongo Valley:   [34.0496, -116.7254]
```
