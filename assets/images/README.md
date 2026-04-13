# Image Assets

Place downloaded or custom images here. All images are currently served from external CDNs.

## Site Photography (thecohostcompany.com)

Download and store locally here if needed for offline development.

### Hero / About
- `NEW-ABOUT-PHOTO-.webp` — https://thecohostcompany.com/wp-content/uploads/2025/10/NEW-ABOUT-PHOTO-.webp

### Landscapes
- `Joshua-Tree-National-Park-.webp` — https://thecohostcompany.com/wp-content/uploads/2025/05/Joshua-Tree-National-Park-.webp
- `Hiking-Rock-Climbing.webp` — https://thecohostcompany.com/wp-content/uploads/2025/05/Hiking-Rock-Climbing.webp.webp
- `Historic-film-set.webp` — https://thecohostcompany.com/wp-content/uploads/2025/05/Historic-film-set-turned-charming-Old-West-town.webp
- `Frame-26.webp` — https://thecohostcompany.com/wp-content/uploads/2025/05/Frame-26.webp

### Video
- Hero background video stored in `/public/video/`

## Property Photos

Property photos are served dynamically from Guesty via Cloudinary:
- Base URL: `https://assets.guesty.com/image/upload/w_1200,q_auto,f_auto/`
- Also served from: `https://res.cloudinary.com/`
- Use `unoptimized` prop with `next/image` for these URLs

## Image Format Guidelines

- Prefer `.webp` or `.avif` for new photography
- next/image handles format conversion automatically (AVIF → WebP → original)
- For hero/above-fold images: use `priority` or `fetchPriority="high"`
- For below-fold images: default lazy loading
- Always include meaningful `alt` text; use `alt=""` for decorative images
