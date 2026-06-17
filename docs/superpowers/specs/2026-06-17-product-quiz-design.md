# Tedd Grooming — Product Recommendation Quiz

**Date:** 2026-06-17
**Status:** Approved

## Overview

A 5-question hair profile quiz that recommends the best Tedd Grooming product for a new customer. Delivered as a single self-contained HTML file — shareable as a link and embeddable on any website via a `<script>` tag or iframe.

---

## Goals

- Help new customers with zero brand knowledge find the right product
- Drive discovery across all 9 products in the lineup
- Match the Tedd Grooming brand aesthetic

---

## File

`C:\Claude\Game\quiz.html` — standalone, no dependencies, same pattern as `index.html` (the game).

---

## Questions (5)

| # | Question | Options |
|---|---|---|
| 1 | How would you describe your hair thickness? | Fine / Medium / Thick |
| 2 | How does your hair behave naturally? | Straight & smooth / Wavy & a bit frizzy / Curly or coily |
| 3 | What kind of hold do you want? | Light & flexible / Medium — firm by end of day / Strong all-day hold |
| 4 | What finish do you prefer? | Natural matte / Slight shine / High shine / Wet look |
| 5 | What's your biggest hair concern? | Dryness / Frizz / Hair staying in place / Volume |

Each answer carries weighted scores toward each product. Highest total score at the end wins.

---

## Products (9)

| Product | Scent | Hold | Finish | Scent color |
|---|---|---|---|---|
| Classic Pomade | Grapeberry | Control | High shine, wet look | `#c0392b` dark red |
| Moisturizing Fixer | Appleberry | Medium | Natural | `#9b4f7a` grape-pink |
| Hair Mousse | Parfum | Firm | Natural | `#2a9d8f` teal |
| Grooming Fixer | Apple spearmint | Medium | Natural matte | `#4a9e5c` green |
| Styling Clay | Vanilla musk | Firm | Matte | `#b5a97a` khaki-gold |
| Hairspray | Parfum | Strong | Wet look | `#2a9d8f` teal |
| Mattifying Dust | Plain vanilla | Firm | Matte | `#d6cfc0` cream off-white |
| Fixing Pomade (parfum) | Parfum | Strong | Wet look, fiber | `#2a9d8f` teal |
| Fixing Pomade (mixfruit) | Mixfruit | Medium | Wet look, fiber | `#e8921a` yellow-orange |

**Tie rule:** Fixing Pomade parfum and mixfruit share a profile (fiber, wet look). When tied, show both on the result card and let the customer pick by scent preference.

---

## Result Screen

- `· TEDD GROOMING ·` wordmark at top
- "Your match" label (white, uppercase, small)
- Product name (olive-gold `#b5a97a`)
- Hold level + size/format (white)
- Scent pill — colored badge matching the product's scent color (colored background, matching text)
- Divider line
- "Why this works for you" label (grey `#aaa`, uppercase, small)
- Personalised 1–2 sentence explanation (white)
- "Retake quiz" button (gold background, black text)

---

## Visual Design

| Element | Value |
|---|---|
| Background | `#0a0a0a` |
| Card/container | `#141410` with `0.5px solid #2e2c24` border |
| Primary text | `#ffffff` |
| Secondary / muted label | `#aaa` |
| Accent / product name | `#b5a97a` olive-gold |
| Progress bar | `#b5a97a` olive-gold fill on `#1e1e1a` track |
| Selected answer border | `#b5a97a` |
| Selected answer bg | `#1c1a12` |
| Button bg | `#b5a97a` |
| Button text | `#000000` |
| Corner radius | `10px` for cards and buttons |
| Wordmark | `· TEDD GROOMING ·` in white, letter-spacing 2px, uppercase, small |

Transition: subtle fade between questions. No other animations.

---

## Architecture

Single HTML file. Structure:

1. **Intro screen** — Tedd Grooming wordmark + "Find your perfect product" headline + Start button
2. **Question screens (1–5)** — progress bar, question text, 3–4 option buttons, Next button
3. **Result screen** — product card as described above

All state (current question, answers, scores) lives in JS variables. No localStorage, no server calls. Score weights defined as a lookup table — each answer increments scores for relevant products by a defined weight value.

---

## Scoring Weight Logic (to be tuned during implementation)

Each answer maps to a score delta per product. Example signals:

- Thick + strong hold + matte → Styling Clay, Mattifying Dust
- Thick + strong hold + wet look → Hairspray, Fixing Pomade parfum
- Any + medium hold + wet look + fiber → Fixing Pomade mixfruit / parfum
- Fine + light hold + natural → Grooming Fixer, Moisturizing Fixer
- Classic + control hold + shine → Classic Pomade
- Firm + pump + volume → Hair Mousse

Every product must be reachable. No product should be a dead end.

---

## Out of Scope

- Shop Now / buy link (not in v1)
- Social sharing
- Analytics / tracking
- Backend or server
