# KILI VIBES ADVENTURES — Claude Code Instructions

## Project Overview

Static tourism website for KILI VIBES & ADVENTURES, a Kilimanjaro-based tour company in Arusha, Tanzania. Single-page site targeting international tourists for Kilimanjaro treks, safaris, and day activities.

- **URL:** https://www.kilivibesadventures.com
- **Hosting:** Cloudflare Pages (auto-deploys on push to `main`)
- **Domain:** Namecheap (registrar only, nameservers at Cloudflare)
- **GitHub:** `forestlii/kilivibes`

## Tech Stack

- Single static `index.html` — all HTML, CSS, and JS in one file
- Zero dependencies, no build step
- YouTube embeds via iframe
- Photos served from `Photos/` directories
- 6-language i18n: English (default), Kiswahili, 中文, Français, Nederlands, Italiano

## Design System

- **Palette:** Warm earth tones — brown `#5c3d2e`, gold `#c4944a`, cream `#faf8f3`, dark `#3d2b1f`
- **Font:** System stack (`-apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, sans-serif`)
- **Style:** Instagram-inspired minimalism — large photography, breathing whitespace
- **Layout:** Single-page scroll, full-viewport sections

## Editing Guidelines

- The entire site lives in `index.html`. It is a large file — use targeted `Edit` operations, not full rewrites.
- CSS is inline in a `<style>` block; JS is inline in a `<script>` block at the end.
- When adding/modifying translations, update all 6 language objects (`en`, `sw`, `zh`, `fr`, `nl`, `it`) in the i18n system.
- Photos are referenced by filename from `Photos/Safari/` or `Photos/Day_Activities/`.
- Content detail modals (treks, safaris, activities) follow a consistent structure — copy an existing one as a template.

## Content Structure

- **Mountaineering (8 routes):** Machame, Marangu, Lemosho, Londorossi, Rongai, Umbwe, Longido Cultural Tour, Mount Meru
- **Safari (8 itineraries):** Northern Circuit, Great Migration, Lake Natron, Mkomazi, Usambara, Lake Eyasi, Elephants & Beaches, Beach Extension
- **Day Activities (5):** Arusha Farm Walk, Ngorongoro/Maji Moto, Lake Duluti, Napuru Waterfalls, Raga Voice

## Environment

- **OS:** Windows 11, PowerShell 5.1
- **Shell:** Use PowerShell syntax (backtick for line continuation, `$env:VAR` for env vars)
- **Git operations:** Avoid `git add -A`; stage specific files. Never amend commits unless asked.
- **Model:** DeepSeek v4 Pro via API proxy

## Key Files

| Path | Purpose |
|------|---------|
| `index.html` | Entire website |
| `Photos/Safari/` | Safari photo assets (28 images) |
| `Photos/Day_Activities/` | Day activity photo assets (17 images) |
| `docs/superpowers/specs/2026-05-27-kilivibes-website-design.md` | Design spec |
| `docs/superpowers/plans/2026-05-27-kilivibes-website-plan.md` | Implementation plan |

## Git Conventions

- Branch: `main` (production)
- Commit style: `@ type: description` (e.g., `@ feat: add sw/fr/nl/it translations`)
- Push to `main` triggers Cloudflare Pages deploy automatically
