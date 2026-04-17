# CLAUDE.md — Kabul Estates Website

## Project
Build a **premium real estate website** for a Kabul, Afghanistan based real estate business that helps clients **buy, sell, and rent** properties. Every CTA must drive to **WhatsApp**.

**Tech stack:** Plain `HTML + CSS + JavaScript` (no frameworks, no build step). Single-page, responsive, fast.

**WhatsApp number:** `+937XXXXXXXX` — use this placeholder everywhere. All CTAs must link to:
`https://wa.me/937XXXXXXXX?text=<pre-filled context message>`

---

## Design Direction

**Aesthetic:** Luxury, modern, editorial. Think high-end real estate agency in Dubai/London — not generic template.

**Color palette (strict):**
- Primary background: deep black `#0a0f0c`
- Secondary surface: `#0f1612` / `#141c17`
- Primary accent: electric green `#17c47a`
- Deep green: `#0f9f62`
- Dark green: `#0b5c3a`
- Text: cream `#f4f1e8`, muted `#8fa39a`
- Lines/borders: `#27332c`

**Typography:**
- Display: `Fraunces` (serif, variable) — for headlines
- Body: `Manrope` (sans) — for English/Pashto body text
- RTL/Persian script: `Vazirmatn` — used when language is Dari or Pashto
- Load from Google Fonts

**Visual details (required):**
- SVG grain/noise overlay on top (opacity ~0.06, mix-blend: overlay)
- Gradient mesh + subtle grid lines in hero background
- Floating property card in hero (offset, overlapping)
- Generous whitespace, asymmetric section layouts
- Micro-animations on hover (cards lift, arrows slide)
- Staggered fade-up reveals on scroll (IntersectionObserver)
- Floating WhatsApp button bottom-right (pulse animation)

**Rules — do NOT:**
- Use Inter, Roboto, or Arial
- Use purple gradients or generic SaaS aesthetics
- Use stock-photo-looking card images — use CSS gradients with `--img-hue` variables instead for placeholder property images
- Build anything with framework dependencies

---

## Site Structure (single `index.html`)

1. **Fixed nav** — logo, links (Listings, About, Services, Contact), language switcher (EN / دری / پښتو), small WhatsApp CTA button
2. **Hero** — eyebrow "Kabul · Afghanistan", large headline with one accent word in green, subhead, two CTAs (WhatsApp primary, "Browse Listings" ghost), 3 stats row, floating featured-property card
3. **Filter strip** — Type (Buy/Rent/Sell), Area (Wazir Akbar Khan, Shahr-e-Naw, Karte Seh, Taimani, Qala-e-Fatullah, Khair Khana), Bedrooms, Budget, Search button
4. **Listings grid** — 6 property cards. Each card: gradient image area with badge (For Sale / For Rent) + price, title, meta (beds/baths/m²), WhatsApp CTA
5. **Services** — 3 columns: Buy / Rent / Sell. Middle one accented green
6. **About** — two-column: text on left, stat card + decorative shape on right
7. **Final CTA block** — big green box, centered "Chat on WhatsApp" XL button, phone number visible
8. **Footer** — logo, tagline, nav links, copyright
9. **Floating WhatsApp button** — fixed bottom-right, green, gentle pulse

---

## Tri-Lingual Requirement (critical)

Three languages: **English (en)**, **Dari (dr)**, **Pashto (ps)**.

Implementation:
- Every translatable element gets `data-i18n="key"` attribute
- Translations live in `js/i18n.js` as a single object: `{ en: {...}, dr: {...}, ps: {...} }`
- Language switcher in nav: three buttons `EN | دری | پښتو`
- On click: update all `[data-i18n]` elements, set `document.documentElement.lang`, toggle `dir="rtl"` on `<body>` for Dari/Pashto
- Persist selection in `localStorage`
- Default language: English

Required keys (translate ALL three languages, no missing keys):
```
brand, nav_listings, nav_about, nav_services, nav_contact,
hero_eyebrow, hero_title_1, hero_title_2, hero_title_3, hero_sub,
cta_wa_short, cta_wa_hero, cta_wa_big, cta_browse, cta_inquire, cta_talk,
stat_listings, stat_exp, stat_rating,
filter_type, filter_area, filter_beds, filter_budget, filter_search,
opt_buy, opt_rent, opt_sell,
tag_featured, badge_rent, badge_sale, meta_bed, meta_bath,
listings_eyebrow, listings_title,
services_eyebrow, services_title,
service_buy_title, service_buy_text,
service_rent_title, service_rent_text,
service_sell_title, service_sell_text,
about_eyebrow, about_title, about_p1, about_p2,
about_stat_1, about_stat_2, about_stat_3,
contact_eyebrow, contact_title, contact_sub,
foot_tag, foot_rights
```

Use natural, professional translations — not machine-translated word-for-word. Dari and Pashto copy should read like a local agency wrote it.

---

## File Structure

```
/
├── index.html
├── css/
│   └── style.css
├── js/
│   ├── i18n.js      (translations + language switcher logic)
│   └── main.js      (scroll reveal, mobile menu, year stamp)
└── README.md        (short — how to run: just open index.html)
```

---

## Responsive

- Desktop first, then mobile
- Breakpoints: 1024px, 768px, 480px
- Mobile: hamburger menu (animated burger → X), stacked hero, single-column listings, filter strip wraps
- Touch-friendly CTAs (min 44px tap target)

---

## CTAs — WhatsApp behavior

Every CTA link structure:
```
https://wa.me/937XXXXXXXX?text=<URL-encoded contextual message>
```

Examples:
- Hero: "Salaam, I'm interested in a property."
- Card on Shahr-e-Naw apartment: "Interested in Shahr-e-Naw apartment"
- Final CTA: "Salaam, I'd like to discuss a property."

---

## Quality Bar

- No lighthouse errors
- Semantic HTML (`<header>`, `<section>`, `<article>`, `<footer>`)
- All images have alt attributes (or aria-hidden on decorative SVG)
- All buttons/links keyboard accessible
- Smooth scroll on anchor nav
- Clean, commented CSS using CSS variables from `:root`

---

## Build Order

1. Scaffold files and `:root` variables
2. `index.html` structure with all `data-i18n` keys in place
3. `css/style.css` — nav, hero, filter strip, listings, services, about, contact, footer, floating WhatsApp, mobile responsive
4. `js/i18n.js` — all three translation objects complete + switcher logic + RTL toggle + localStorage
5. `js/main.js` — mobile menu, scroll reveal with IntersectionObserver, year stamp
6. Test: open `index.html`, switch all 3 languages, verify RTL flips correctly, verify every WhatsApp link opens with correct pre-filled text
