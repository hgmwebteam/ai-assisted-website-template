# Site Map & Page Reference

## Pages

| Route | File | Description |
|-------|------|-------------|
| `/` | `app/page.tsx` | Home — component composition |
| `/search` | `app/search/page.tsx` | Search results + Leaflet map |
| `/property/[slug]` | `app/property/[slug]/page.tsx` | Property detail (slug = Guesty `_id`) |
| `/stays` | `app/stays/page.tsx` | All listings browse page |
| `/checkout` | `app/checkout/` | Booking checkout flow |
| `/about` | `app/about/page.tsx` | About the company |
| `/contact` | `app/contact/page.tsx` | Contact form |
| `/guidebook` | `app/guidebook/page.tsx` | Guest guidebook / virtual concierge |
| `/services` | `app/services/page.tsx` | For hosts / property management |
| `/press` | `app/press/page.tsx` | Press coverage |
| `/local-favorites` | `app/local-favorites/page.tsx` | Local area recommendations |
| `/concierge` | `app/concierge/` | Virtual concierge |

## API Routes

| Route | File | Description |
|-------|------|-------------|
| `GET /api/guesty/listings` | `app/api/guesty/listings/route.ts` | All Guesty listings, trimmed for cards |

Add `?bust=1` to force cache refresh.

## Home Page Component Order

```
<Nav />           Fixed navigation
<Hero />          Full-screen video hero + search bar
<AboutCohost />   Company intro section
<FeaturedStays /> Featured property cards carousel
<Location />      "Where We Are" map/locations section
<LocalFavorites />Local area highlights
<Testimonials />  Guest review carousel
<Benefits />      "Benefits of Booking With Us" (dark section)
<Instagram />     Instagram feed (Elfsight widget)
<SignUp />        Email capture + 10% off offer
<ThankYou />      Post-signup section
<Footer />        Site footer
```

## Assets & Imagery

### Image CDNs
- **Property photos:** `https://assets.guesty.com/` and `https://res.cloudinary.com/`
- **Site photography:** `https://thecohostcompany.com/wp-content/uploads/`
- **Stock:** `https://images.unsplash.com/`

### Logo
```
https://thecohostcompany.com/wp-content/uploads/2025/05/Logos.svg
https://thecohostcompany.com/wp-content/uploads/2025/05/Logos-.png  (favicon)
```

### Hero Video
Location in `/public/video/` — served locally

### Key Photography URLs (thecohostcompany.com/wp-content/uploads/)
```
2025/05/Joshua-Tree-National-Park-.webp       Desert landscape
2025/10/NEW-ABOUT-PHOTO-.webp                 About section hero
2025/05/Hiking-Rock-Climbing.webp.webp        Benefits background
2025/05/Historic-film-set-turned-charming...  Pioneertown/adventure
2025/05/Frame-26.webp                         Location card
```

### Guesty Cloudinary Base
```
https://assets.guesty.com/image/upload/w_1200,q_auto,f_auto/v.../filename.jpg
```

## Static Property Data

Defined in `src/data/properties.ts`. Each property has:
```typescript
{
  slug: string;         // URL slug (used in /property/[slug])
  name: string;         // Display name
  location: string;     // City/area
  guests: number;
  beds: number;
  baths: number;
  images: string[];     // Array of image URLs
  description?: string;
  amenities?: string[];
  priceFrom?: number;
  tags?: string[];
}
```

Featured slugs (shown in nav mega menu by default):
```
"the-outlaw"
"whistling-rock"
"heavens-door"
```

## Guesty Tag System

Tags on Guesty listings map to category filters:

| Display Label | Guesty Tag |
|--------------|------------|
| Featured | (no tag — show all) |
| Romantic Retreat | `Romantic Retreat` |
| Pool | `Pool` |
| Boulders | `Boulders` |
| Unique | `Unique(to-do)` |
| Luxury | `Luxury(to-do)` |

## Third-Party Integrations

| Service | URL | Purpose |
|---------|-----|---------|
| Elfsight platform | `https://elfsightcdn.com/platform.js` | Instagram widget |
| LeadConnector form embed | `https://link.msgsndr.com/js/form_embed.js` | Email capture |
| LeadConnector form iframe | `https://api.leadconnectorhq.com/widget/form/JMOPYLHw05kL4wVTv6wQ` | Embedded form |
| CartoCDN tiles | `https://{s}.basemaps.cartocdn.com/light_all/...` | Map tiles |
| Leaflet CDN (marker fallback) | `https://unpkg.com/leaflet@1.9.4/dist/images/` | Default map marker images |

## Press Features

Featured in (shown in press logos section):
- Condé Nast
- The New York Times
- (others from press page)
