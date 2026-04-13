# Color System

## Core Brand Colors

These are the most-used values. Use as Tailwind arbitrary values: `bg-[#F7F4EF]`, `text-[#1C1410]`.

| Name | Hex | Usage |
|------|-----|-------|
| Brown Dark | `#1C1410` | Primary text, dark backgrounds, nav |
| Brown Mid | `#7B5B3A` | Secondary text, icons, borders |
| Brown Light / Gold | `#C4A882` | Accent, section labels, borders, pin bubbles |
| Warm Background | `#F7F4EF` | Page background, input backgrounds |
| Warmest Background | `#FAF8F5` | Lightest warm white sections |
| Border Warm | `#EDE8DF` | Dividers, card borders, subtle lines |

## Full Brand Scale (CSS Variables)

Defined in `globals.css` under `@theme inline`:

### Brand (B) — Terracotta/Brown
```
--color-b-25:   #FCFBF8
--color-b-50:   #FBF5F2
--color-b-100:  #EAE2D8
--color-b-200:  #D4C3B3
--color-b-300:  #C4B3A3
--color-b-400:  #B0A38C
--color-b-500:  #AE8A71
--color-b-600:  #A47B64   ← primary/button color
--color-b-700:  #8D5F52   ← nav CTA, mega menu active, mobile CTA
--color-b-800:  #774D46
--color-b-900:  #633F3D
--color-b-950:  #523633
--color-b-975:  #2D1B18
```

### Neutral (N) — Grays
```
--color-n-25:   #FCFCFC
--color-n-50:   #F7F7F7
--color-n-100:  #F5F5F5
--color-n-200:  #E8E8E8
--color-n-300:  #D6D6D6
--color-n-400:  #A3A3A3
--color-n-500:  #737373
--color-n-600:  #525252
--color-n-700:  #424242
--color-n-800:  #292929
--color-n-900:  #141414
--color-n-950:  #0F0F0F
--color-black:  #0A0A0A
```

## Contextual Usage

| Context | Color |
|---------|-------|
| Page body background | `#F7F4EF` |
| White card background | `#FFFFFF` |
| Card border | `#EDE8DF` |
| Primary text | `#1C1410` |
| Secondary text | `#5A4A3A` |
| Muted text | `#8A7968` |
| Very muted / placeholder | `#9A8A7A` |
| Gold accent / labels | `#C4A882` |
| Mid brown links/icons | `#7B5B3A` |
| Dark CTA button | `#1C1410` |
| Terracotta CTA button | `#8D5F52` |
| Brown mid button | `#7B5B3A` → hover `#5A3E28` |
| Nav background | `rgba(20,12,6,0.85)` with blur |
| Hero overlay | `rgba(0,0,0,0.35)` to `rgba(28,20,16,0.55)` |
| Dark section overlay | `rgba(28,10,4,0.78)` |
| Map pin (default) | bg `#fff`, border `#C4A882`, text `#1C1410` |
| Map pin (hovered) | bg `#1C1410`, border `#1C1410`, text `#fff` |

## Do Not Use

- Purple gradients
- Blue tones
- High-saturation colors
- Pure black (`#000`) — use `#1C1410` instead
- Pure white in dark sections — use `rgba(255,255,255,0.85)` for subtlety
