# Logo Assets

## Primary Logo (SVG)
URL: https://thecohostcompany.com/wp-content/uploads/2025/05/Logos.svg
- Used in nav (white, 220×64px display)
- Used in mobile drawer (dark-filtered via CSS)
- Use `unoptimized` with `next/image`

## Mobile Drawer Logo Filter
To render the SVG logo in dark brown on a white background, apply this CSS filter:
```css
filter: invert(1) sepia(1) saturate(2) hue-rotate(340deg) brightness(0.35);
```

## Favicon / App Icon (PNG)
URL: https://thecohostcompany.com/wp-content/uploads/2025/05/Logos-.png
- Used as favicon, shortcut icon, and Apple touch icon in `layout.tsx`

## Logo Usage Rules
- Never stretch or distort
- On dark backgrounds: use white/natural SVG version
- On light backgrounds: apply dark filter or use dark variant
- Minimum display size: 120px wide
- Nav display size: 220×64px (desktop), 160×48px (mobile drawer)
