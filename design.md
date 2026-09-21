# Inspection Car — Design & Build Brief

> Reference studied: [Kingspector](https://kingspector.com/) (homepage, booking, locations, one city landing page, about, contact, and blog) on 7 September 2026.
>
> **Use this as inspiration for the information architecture and conversion flow only.** Do not copy Kingspector’s logo, name, photos, icons, pricing, testimonials, customer data, written copy, report sample, or brand claims. Replace every placeholder with the Inspection Car brand and real business data.

## 1. What to build

Build a polished, responsive Indonesian website for **Inspection Car**, an independent used-car inspection service. The site should make a nervous used-car buyer feel protected, informed, and able to book an inspection in a few minutes.

The visual character should be:

- clean, technical, reassuring, and human—not a generic automotive marketplace;
- spacious white/off-white surfaces with a fresh teal accent;
- evidence-led: clear scope, practical report preview, process, pricing, city availability, testimonials, and FAQs;
- original in layout and brand expression, while following the useful service funnel of the reference site.

### Primary conversion

`Choose city → choose package → set inspection date/time → give vehicle and seller details → submit booking → confirmation / WhatsApp hand-off`

### Secondary conversion

`Ask a question on WhatsApp` and `View sample report`.

## 2. Reference analysis: why the structure works

Kingspector leads with an immediate consultation CTA, then removes buying anxiety in this order:

1. Explain what is checked.
2. Make the service process feel simple.
3. Let visitors see packages/prices.
4. Prove credibility with independent inspection, tools, reports, and experienced inspectors.
5. Explain inspection depth.
6. Offer reviews and answers to common objections.

Its broader location page adds a stronger long-form sales story: buyer risks, intended audiences, trust signals, inspection categories, city coverage, and a repeatable booking flow. Its booking page reduces friction through a short multi-step form. Its blog uses city-focused educational articles as organic-search entry points.

Use that **sequence of reassurance**, not the original visual assets or wording.

## 3. Sitemap

```text
/
├─ /layanan                         # optional services overview
├─ /harga                           # all package pricing and comparison
├─ /lokasi
│  ├─ /lokasi/[city-slug]           # SEO/service landing page for each city
│  └─ /lokasi                       # coverage list + national-value landing page
├─ /booking                         # 2-step booking flow
├─ /contoh-laporan                  # anonymised report preview / explanation
├─ /tentang-kami
├─ /kontak
├─ /artikel
│  └─ /artikel/[slug]
├─ /kebijakan-privasi
└─ /syarat-ketentuan
```

Keep global navigation to 5–6 items at most: `Layanan`, `Harga`, `Lokasi`, `Artikel`, `Tentang`, plus a filled `Booking Inspeksi` button. A floating WhatsApp button should remain available but never cover mobile CTAs or form controls.

## 4. Homepage: exact content architecture

### A. Header

- Sticky only after a small scroll; white translucent surface with a subtle bottom border.
- Left: original Inspection Car logomark + wordmark.
- Center/right: primary navigation.
- Far right: `Booking Inspeksi` (teal filled). On desktop, a quieter `Konsultasi` text link may sit beside it.
- Mobile: compact logo, WhatsApp icon/button, menu button. Full-screen menu should include the booking CTA.

### B. Hero — answer the core fear within one screen

Two-column layout: text/CTAs at left, an original editorial photo or abstract diagnostic/report visual at right. Avoid copying the reference’s photo composition.

Suggested content direction:

- Eyebrow: `INSPEKSI MOBIL BEKAS INDEPENDEN`
- H1: `Beli mobil bekas tanpa menebak kondisinya.`
- Supporting text: explain that a qualified independent inspector visits the vehicle and delivers an understandable report before the buyer makes a decision.
- Primary CTA: `Jadwalkan Inspeksi`
- Secondary CTA: `Lihat Contoh Laporan`
- Trust row: `Datang ke lokasi` · `Laporan foto & temuan` · `Objektif untuk pembeli`

Use one honest, verifiable proof line only if actual business data exists (for example, `Tersedia di 7 kota`). Do not invent inspection counts, customer counts, or “#1” claims.

### C. “Yang kami periksa” / inspection checklist

Use a compact, responsive grid of 8–12 check chips or mini-cards. Categories to model:

- exterior and body condition;
- flood / collision indicators;
- interior and features;
- engine and transmission;
- brakes, suspension, tyres, and battery;
- AC and electrical;
- OBD diagnostics;
- document verification.

Add a link to the full inspection checklist. The reference uses a dense horizontal grid; use larger touch targets and clearer category labels.

### D. “Cara kerjanya” — four simple steps

Render four equal cards on desktop, a vertical numbered timeline on mobile:

1. Consult — explain the car/need.
2. Schedule — choose city, time, and package.
3. Inspect — inspector checks the car at its location.
4. Decide confidently — receive report, call summary, and recommended next step.

Icons should be from one original icon set (Lucide is a good choice), in teal-tinted circular containers. Do not use mismatched stock icon styles.

### E. Package and price explorer

Use a single clean pricing section rather than the reference’s dense tabs/carousel behavior.

- Segment tabs: `City` (or vehicle class), if package pricing genuinely varies.
- Show 3–4 packages as cards: package name, ideal customer, price, 5–7 inclusions, and CTA.
- One recommended tier may be gently highlighted.
- A compare drawer/table may show every inspection item.
- Add clear notes for travel surcharges, older vehicles, cancellation/rescheduling, report turnaround, and what is outside the scope.

Every price, vehicle category, and surcharge must be supplied by the business. Never reuse numbers from Kingspector.

### F. Why Inspection Car

Use 4–5 benefit cards—not a carousel. Horizontal carousels tend to hide information and perform poorly on mobile.

Recommended benefit themes:

- Independent and buyer-first
- Detailed, photo-backed report
- Professional inspection equipment
- Inspector comes to the car
- Private, secure results

Each card needs a concrete one-sentence explanation. Provide an optional `How we stay independent` link that explains the policy in a transparent way.

### G. Inspection depth

Make this a teal/dark-ink feature band, visually distinct from white sections.

- Heading such as `See the car beyond the shiny paint.`
- Four areas: Exterior, Interior, Mechanical, Documents.
- Each area has 3–6 example checks.
- Include an anonymised, original report preview thumbnail and `Explore a sample report` link.

The reference uses 155+ points. Inspection Car should state an exact total only if documented internally.

### H. Proof and reports

Do not repeat testimonial carousels. Use a balanced proof section:

- 2–3 verified client quotations with name initial / first name, city, and consent;
- a screenshot-style *original* anonymised report preview;
- one short statement on turnaround time if operationally accurate.

### I. FAQ

Use accessible accordion rows. Include at least:

- Which areas do you cover?
- Can you inspect at a showroom or seller’s house?
- How long does it take?
- What do I receive after inspection?
- Can the service guarantee that a car will not break later?
- What documents are checked?
- Can I cancel or reschedule?

The final answer must be plain-language and bounded: inspection informs a purchase decision; it is not a warranty.

### J. Final CTA + footer

End with a compact colored CTA: `Sudah menemukan mobil incaran? Periksa dulu sebelum deal.` Then `Booking Inspeksi` and `Tanya via WhatsApp`.

Footer: logo/tagline, navigation, city list, contact details, social links, legal links, and copyright. Use a dark ink footer or an off-white footer separated by a strong border; do not duplicate footer blocks.

## 5. Key internal pages

### `/lokasi` and `/lokasi/[city]`

The reference’s location hub is its strongest conversion page. Recreate the underlying pattern with original content:

1. City-aware hero and availability status.
2. Quick city selector or map/list.
3. Buyer pain points and who the service is for.
4. Relevant package card(s) and local booking CTA.
5. Detailed inspection categories.
6. Common local risks only when evidence-based; avoid fear-mongering.
7. FAQs, reviews, and city-specific contact/coverage information.

Each city page must have distinct factual copy: actual coverage boundaries, schedule, travel fee, and local contact details. Give every page a unique H1, title, description, canonical URL, and local-business/service schema.

### `/booking`

Make this a calm, two-step wizard with save-on-step or reliable local draft storage.

**Step 1 — Your inspection**

- City / coverage area
- Preferred date
- Preferred time window
- Package / vehicle class
- Vehicle make, model, year, plate number (optional if unknown)

**Step 2 — Contact and location**

- Buyer name and WhatsApp number
- Seller contact (optional, explain why it helps)
- Inspection address + map link or location picker
- Notes / specific concern
- Consent checkbox for terms and privacy

**Review and submit**

- Show clear order summary and price assumptions.
- State what happens next: confirmation by team, inspector assignment, then report delivery.
- Use inline validation, a visible stepper, Back button, and no reset on validation errors.

Success screen: booking reference, summary, operating-hours expectation, and WhatsApp fallback. Do not make users guess whether the booking succeeded.

### `/harga`

Show packages, compare features, explain optional fees, and repeat policy details. Let visitors select a package, then preserve that selection when they go to `/booking`.

### `/contoh-laporan`

Present an original anonymised sample as: finding severity legend, photo finding, inspection category, recommended action, and estimated urgency. Do not publish personal information, real plate numbers, addresses, or the reference company’s PDF.

### `/tentang-kami` and `/kontak`

About: mission, independence policy, inspector standards, tools/process, and original team images only with consent. Contact: phone/WhatsApp, email, hours, area served, map link, and a short inquiry form. The reference provides basic company story and contact routes; Inspection Car should make the operating promise more specific and verifiable.

### `/artikel`

Use a card grid with article image, category, date, title, teaser, and read link. Editorial topics should answer actual purchase questions: flood signs, OBD meaning, document checks, used-car negotiation, buying remotely, and city-specific buying guides. This supports SEO without filling the site with thin city pages.

## 6. Design system

The reference’s visual language is a light neutral base with turquoise highlights. Use a related but distinct, original palette.

```css
/* Suggested starting tokens — tune to Inspection Car identity */
--bg: #F7F9F9;
--surface: #FFFFFF;
--ink: #102A2E;
--muted: #66777A;
--line: #DDE7E7;
--brand: #16B9B1;
--brand-strong: #0D8F8A;
--brand-soft: #E6F8F6;
--success: #168A59;
--warning: #C77700;
--danger: #BF3B3B;
--radius-sm: 12px;
--radius-md: 20px;
--radius-lg: 28px;
--shadow-card: 0 10px 30px rgba(16, 42, 46, .08);
```

### Typography

- Use `Inter` or `Manrope` for the interface. Use one family, not a mixture of Inter and Open Sans.
- H1: `clamp(2.25rem, 4.2vw, 4.5rem)`, 700–750 weight, tight leading (1.05–1.12).
- H2: `clamp(1.75rem, 2.5vw, 3rem)`, 700 weight.
- Body: 16–18px / 1.55–1.7; never lighter than 400.
- Use a high-contrast ink color for headings. Teal should signal action or key information, not carry paragraphs of text.

### Layout and rhythm

- Max content width: 1200px; 24px mobile gutters; 40px tablet; 56px desktop.
- Section spacing: 72px mobile / 112–144px desktop.
- Cards: white, 1px `--line` border, 16–20px radius, soft shadow only on hover or feature emphasis.
- Use full-bleed background bands sparingly—hero, inspection-depth, final CTA.
- Prefer 2, 3, or 4-column CSS grids over auto-moving content. Any carousel must be pauseable, keyboard usable, and nonessential.

### Buttons and interaction

- Primary: filled teal, white label, minimum 48px tall.
- Secondary: white/transparent, ink or teal outline.
- Tertiary: text link with arrow, visible underline/focus style.
- Hover: subtle lift or darker teal; no large bounce animations.
- Focus: a 3px visible ring that remains clear on all backgrounds.

## 7. Responsive rules

| Breakpoint | Behavior |
| --- | --- |
| `< 640px` | One column; 16–20px page gutter; stack CTAs; pricing cards scroll only if comparison is unavoidable; booking CTA stays reachable. |
| `640–1023px` | Two-column cards where readable; workflow may remain 2×2; hero photo can move below copy. |
| `≥ 1024px` | Two-column hero; 3–4 card grids; generous section rhythm; max-width container. |

On mobile, do not shrink desktop cards until text becomes tiny. Reflow content, keep 44×44px minimum targets, retain FAQ access, and place the booking CTA before long evidence sections as well as at the bottom.

## 8. Accessibility and quality bar

- Semantic landmarks: `header`, `nav`, `main`, `section`, `footer`; exactly one H1 per page.
- Use buttons for UI actions and links for navigation. Tabs and accordions must support keyboard + `aria-selected` / `aria-expanded`.
- All fields have visible labels; messages identify invalid fields in text, not color only.
- Every visual has alt text appropriate to its purpose. Decorative icons use empty alt / `aria-hidden`.
- Meet WCAG AA contrast. Teal on white frequently fails for small body text; use darker teal or ink.
- Respect `prefers-reduced-motion`; avoid autoplays and automatic carousels.
- Optimise images (WebP/AVIF), reserve image dimensions to prevent layout shift, lazy-load below the fold, and target a fast mobile LCP.

## 9. Suggested data model

```text
City:        id, slug, name, coverageAreas, availability, travelPolicy, contact
Package:     id, name, vehicleClass, price, currency, inclusions, exclusions, cityIds
CheckItem:   id, area, label, description, packageIds
Testimonial: id, quote, nameDisplay, city, consent, image
FAQ:         id, question, answer, cityId?
Report:      id, status, categories, findings, severity, photos, recommendation
Article:     id, slug, title, excerpt, coverImage, category, publishedAt, cityId?
Booking:     id, cityId, packageId, schedule, vehicle, buyer, seller?, location, notes, status
```

Keep all pricing, coverage, testimonials, FAQs, and city facts in a CMS or typed content files—not hard-coded across components. This lets the website change safely as the business expands.

## 10. Practical implementation guidance

- Build reusable pieces: `SiteHeader`, `Hero`, `CheckGrid`, `ProcessSteps`, `PricingExplorer`, `BenefitCard`, `InspectionDepth`, `TestimonialCard`, `FaqAccordion`, `FinalCta`, `SiteFooter`, and `BookingWizard`.
- Make the city and package choices drive both the pricing UI and booking data. Do not make a display-only price selector.
- Implement a backend/API validation layer for bookings. Validate and normalize phone numbers server-side; protect against spam; never expose API credentials to the browser.
- Send bookings to a private business inbox/CRM only after the customer explicitly submits. Store only necessary personal data and display the privacy policy alongside the consent checkbox.
- Add analytics events only with the site’s privacy/legal requirements covered: `hero_cta_click`, `package_selected`, `booking_step_completed`, `booking_submitted`, and `whatsapp_click`.
- Use original photographs of inspectors/equipment/cars, licensed stock assets, or abstract illustration. Never scrape or hotlink images from Kingspector.

## 11. Build prompt for another AI model

Copy the prompt below, then replace all `[bracketed]` values with real Inspection Car information.

```text
Build a production-quality responsive Indonesian website for a used-car inspection service named "Inspection Car". Use [framework/stack] and [CSS approach]. The business serves [cities], offers [real package names and prices], and uses WhatsApp [number] for consultation.

The site must feel trustworthy, premium, clear, and buyer-first: off-white background, original teal accent (#16B9B1 as starting point), dark teal ink, one modern sans-serif font, large clean typography, generous whitespace, 20px cards, and subtle borders/shadows. Do not copy Kingspector’s logo, photos, text, testimonials, prices, report sample, or exact layout; create an original brand expression.

Create these routes: /, /layanan, /harga, /lokasi, /lokasi/[city], /booking, /contoh-laporan, /tentang-kami, /kontak, /artikel, /artikel/[slug], /kebijakan-privasi, and /syarat-ketentuan.

Homepage flow:
1. Sticky header with Layanan, Harga, Lokasi, Artikel, Tentang, and filled Booking Inspeksi CTA.
2. Two-column hero: H1 about buying used cars with confidence, short independent-inspection explanation, Jadwalkan Inspeksi + Lihat Contoh Laporan CTAs, and 3 factual trust markers.
3. Grid of inspection checks.
4. Four-step process: consultation, scheduling, on-site inspection, report/decision.
5. Pricing explorer using real packages and clear scope/fees.
6. Four or five benefit cards about independence, report detail, professional tools, on-site service, and privacy.
7. High-contrast inspection-depth section for Exterior, Interior, Mechanical, and Documents with sample report preview.
8. Consent-backed testimonials.
9. Accessible FAQ accordion.
10. Final booking CTA and complete footer.

Create an SEO city landing template that uses factual city coverage, city-specific metadata/schema, package availability, inspection categories, city FAQ, testimonials, and a booking CTA. Avoid duplicated thin SEO pages.

Create a two-step booking wizard:
Step 1: city, preferred date/time, package, vehicle make/model/year/plate.
Step 2: buyer name/WhatsApp, optional seller contact, inspection address, notes, privacy/terms consent.
Then show an order-review state and a clear success screen with booking reference and WhatsApp fallback. Persist selected package/city into booking. Validate all fields accessibly and server-side.

Requirements: desktop/tablet/mobile responsive, semantic HTML, WCAG AA contrast, keyboard-accessible menu/tabs/accordions, visible focus states, reduced-motion support, WebP/AVIF image optimization, no autoplay carousels, loading/empty/error states, and no invented business metrics or fake testimonials. Put content/pricing/cities/FAQs in typed data or CMS-friendly files. Include a short README explaining environment setup and where business data must be replaced.
```

## 12. Pre-launch checklist

- [ ] Brand identity, contact details, service areas, hours, and legal entity are confirmed.
- [ ] Every package price, surcharge, inclusion, and exclusion is approved.
- [ ] Testimonials have customer consent; photos and report sample are anonymised and licensed.
- [ ] Booking submission creates a real confirmation and reaches the correct team.
- [ ] WhatsApp links use the real number and prefilled message, tested on mobile.
- [ ] All city pages have genuine differences and validated service coverage.
- [ ] Forms, keyboard navigation, mobile layout, slow-network state, and 404 state have been tested.
- [ ] Analytics and privacy policy accurately match the deployment.

