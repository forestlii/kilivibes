# KILI VIBES & ADVENTURES Website Design Spec

## Overview

A static, bilingual (EN/ZH) tourism website for KILI VIBES & ADVENTURES, a Kilimanjaro-based tour company founded by Kauzen "Raga Voice" Geofrey. Target: international tourists seeking Kilimanjaro treks, safaris, and day activities in Tanzania.

## Tech Stack

- Pure static HTML/CSS/JS — single HTML file, zero dependencies
- Hosted on Cloudflare Pages (free, global CDN)
- YouTube embeds via iframe
- Photos served directly from local folders

## Visual Design

- **Style**: Instagram-inspired minimalism — large photography, breathing whitespace, clean typography
- **Palette**: Warm earth tones — deep brown (#5c3d2e), golden orange (#c4944a), cream white (#faf8f3)
- **Typography**: System fonts (`-apple-system, BlinkMacSystemFont, "Segoe UI", sans-serif`)
- **Layout**: Single-page scroll, each section takes full viewport or near-full

## Page Sections (Single Page)

### 1. Navigation Bar
- Fixed top, minimal: logo "KILI VIBES" + links to each section
- Language toggle (EN/中文) on the right
- Transparent on hero, solid on scroll

### 2. Hero
- Full-viewport background: embedded YouTube video or gradient fallback
- Large title: "KILIMANJARO" with subtitle "THE ROOF OF AFRICA"
- Single CTA button: "Explore Journeys" → scrolls to treks section

### 3. Grid Navigation (4 squares)
- 2×2 grid of large image cards linking to: Mountaineering, Safari, Day Activities, About Raga Voice
- Each card: gradient background (will be replaced with real photos), category name, brief description

### 4. Featured Treks
- Horizontal cards showing 2-3 highlighted routes (Machame, Lemosho)
- Each card: photo, route name, duration, starting price

### 5. Photo Gallery Strip
- 3-column image strip showing Tanzania landscape/wildlife

### 6. About Snippet
- Short intro to Raga Voice with embedded YouTube performance video
- Quote about his philosophy

### 7. Contact / CTA
- "Ready for Your Adventure?" heading
- WhatsApp button + contact form
- Phone, email, location: Arusha, Tanzania

### 8. Footer
- Minimal: company name, location, copyright

## Content from Source Files

### Mountaineering (8 routes)
Source: `Itineraries/Mountaineering/`
- KILI LEMOSHO 8 DAYS ROUTE.docx
- KILI LONDOROSSI 10 DAY ROUTE.docx
- KILI MACHAME 6 DAY ROUTE.docx
- KILI MARANGU 6DAYS ROUTE.docx
- KILI RONGAI 6 DAY ROUTE.docx
- KILI Umbwe Route.rtf
- Longido Cultural Tour and Mountain Hike.docx
- MOUNT MERU.docx

### Safari (8 itineraries)
Source: `Itineraries/Safari/`
- Elephants, Lions & White Sandy Beaches.docx
- Lake Eyasi Day Trip.docx
- Lake Natron & Oldonyo Lengai.docx
- MKOMAZI NATIONAL PARK.docx
- Northern Circuit 4Days & 3Nights.docx
- Northern Circuit's Great Migration.docx
- The Eastern & Western Usambara.docx
- Tracking the Great Migration.docx

### Day Activities (5 activities)
Source: `Day_Activities/`
- Arusha Farm Walk & Coffee Tour.docx
- Arusha, Ngorongoro, Maji Moto.docx
- Lake Duluti.docx
- Napuru Waterfalls.docx
- Raga Voice.docx

### Photos
- `Photos/Day_Activities/` — 17 photos
- `Photos/Safari/` — 28 photos

## Responsive Behavior

- Desktop: full multi-column layouts, large typography
- Tablet: stacked grids, reduced columns
- Mobile: single column, hamburger menu, touch-friendly tap targets

## Bilingual Strategy

- Default language: English
- Language toggle switches all text content via JS
- Both languages stored in a single JS object, keyed by element ID

## What This Site Does NOT Include

- No booking/payment system (WhatsApp/email for inquiries)
- No CMS or admin panel
- No analytics or tracking
- No third-party dependencies (no jQuery, no frameworks, no Google Fonts)

## File Structure

```
/
├── index.html              # The entire website (single page)
├── photos/                 # Symlink or copy of Photos/ folder
│   ├── day-activities/
│   └── safari/
├── itineraries/            # Symlink or copy of Itineraries/ folder
├── day-activities/         # Symlink or copy of Day_Activities/ folder
└── docs/
    └── superpowers/
        └── specs/
            └── 2026-05-27-kilivibes-website-design.md
```

Original zip files and extracted folders remain in place. HTML references content from those folders.
