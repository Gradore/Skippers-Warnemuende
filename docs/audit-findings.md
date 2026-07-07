# Audit-Findings — Lovable-Build vs. Relaunch-Konzept

Datum: 2026-07-07 · Methode: 5 parallele Dimensions-Audits (SEO, Design/CI, Copy/Psychologie, A11y/Performance, Security) über den generierten Code, jedes Finding adversarial verifiziert (39/39 bestätigt, 0 widerlegt). Alle Findings wurden als Fix-Runde an den Lovable-Agenten gesendet.

## 1. [🔴 Hoch · seo] No absolute site URL anywhere: sitemap <loc>, hreflang, canonical, og:url and JSON-LD URLs are all relative
**Datei:** `multiple`

The site never uses its production origin https://skipper-warnemuende.de. In src/routes/sitemap[.]xml.ts the base is empty: `const BASE_URL = "";`, so the sitemap emits `<loc>/speisekarte</loc>` and `<xhtml:link ... href="/en/menu" />` — the sitemap protocol requires fully-qualified URLs in <loc>, and Google ignores hreflang annotations that are not absolute URLs. The same problem is systemic: src/lib/i18n.ts `hreflangLinks()` returns relative hrefs (`{ rel: "alternate", hrefLang: "de", href: dePath }`), every route head uses relative canonicals (e.g. src/routes/index.tsx: `links: [{ rel: "canonical", href: "/" }]`) and relative og:url (`{ property: "og:url", content: "/" }` — the OG spec requires a full URL), src/components/RestaurantJsonLd.tsx has `url: "/"` and `menu: "/speisekarte"` (concept Teil 9.1 mandates `"menu": "https://skipper-warnemuende.de/speisekarte"`), and src/components/Breadcrumbs.tsx puts relative paths into BreadcrumbList `item`. Net effect: the sitemap is invalid, hreflang de/en/x-default (concept Teil 9.2) is non-functional, and canonicals are fragile — this materially hurts SEO.

**Fix:** Add a single exported constant SITE_URL = "https://skipper-warnemuende.de" (e.g. in src/lib/constants.ts) and prefix it everywhere: set BASE_URL = SITE_URL in src/routes/sitemap[.]xml.ts; make hreflangLinks() in src/lib/i18n.ts return `${SITE_URL}${path}`; change every route head's canonical and og:url to absolute URLs; set url, menu and image URLs in RestaurantJsonLd.tsx to absolute; make Breadcrumbs.tsx emit absolute URLs in the BreadcrumbList item field.

## 2. [🔴 Hoch · seo] aggregateRating is injected globally on every page via the root head, violating the concept's explicit AggregateRating rule
**Datei:** `src/routes/__root.tsx`

Concept Teil 9.1 (line 390) is binding: "AggregateRating NUR einbauen, wenn echte Bewertungen auf der Seite selbst angezeigt werden (Google-Richtlinie)". But src/routes/__root.tsx injects the full Restaurant schema on EVERY route: `scripts: [{ type: "application/ld+json", children: JSON.stringify(restaurantJsonLd()) }]`, and src/components/RestaurantJsonLd.tsx unconditionally includes `aggregateRating: { "@type": "AggregateRating", ratingValue: RATING.score.replace(",", "."), reviewCount: RATING.count }`. Testimonials are only visibly rendered on / and /en (TestimonialBand). So /speisekarte, /kontakt, /impressum, /datenschutz, /feiern etc. all carry review markup with no reviews on the page — exactly what Google's structured-data guidelines flag and what the concept forbids.

**Fix:** Make restaurantJsonLd() accept an option like includeRating (default false) and only pass it as true from routes that visibly render the testimonial module (src/routes/index.tsx and src/routes/en.index.tsx). Keep the global Restaurant schema in __root.tsx but without the aggregateRating property.

## 3. [🔴 Hoch · design-ci] Hero violates Anti-KI-Manifest rule 1: left text panel on cream instead of navy, photo is an inset rounded card instead of full-bleed
**Datei:** `src/routes/index.tsx`

Concept Teil 3 rule 1 (UNVERHANDELBAR) mandates: 'Asymmetrischer Split-Hero: Text links auf Navy-Fläche (55 %), rechts randabfallendes Foto der Terrasse/Mole (45 %)'. The Hero component instead renders the whole section on cream: `<section className="relative bg-[color:var(--cream)]">` with an inner container `mx-auto grid max-w-[1200px] gap-8 px-5 py-14 ... md:grid-cols-[minmax(0,55fr)_minmax(0,45fr)]`. The 55/45 column split exists, but (a) the left panel sits on cream with navy headline instead of a navy surface with white text, and (b) the photo is `<PhotoPlaceholder ... className="h-full min-h-[420px]" />` which renders with `rounded-[14px]` (see PhotoPlaceholder.tsx) inside the padded max-w-1200 container — an inset rounded card, not 'randabfallend' (edge-to-edge). This also creates a Teil 2.2 contrast violation: the gold eyebrow `text-[color:var(--gold)]` ('Mittelmole · Warnemünde', text-xs) plus gold wheel and gold star sit on cream — the concept forbids gold text on white/light backgrounds ('Gold-Text auf Weiß ist verboten — zu wenig Kontrast').

**Fix:** Restructure Hero: make the section a full-width grid with no outer max-width/padding on the section itself (md:grid-cols-[55fr_45fr]). LEFT column: background var(--navy) (optionally flat navy→navy-dark, no purple/blue gradient), inner content padded to align with the 1200px page grid (e.g. pl-[max(1.25rem,calc((100vw-1200px)/2))] pr-12 py-24 md:py-[120px]); H1 and 'frisch'-copy in white (text-white / text-white/85), eyebrow stays gold (allowed on navy), OpenStatusChip variant="onNavy", star-rating row in white/gold, secondary 'Speisekarte ansehen' button as white outline (border-white/40 text-white hover:bg-white/10) since navy-outline is invisible on navy. RIGHT column: the PhotoPlaceholder must bleed to the right, top and bottom edges — add a prop (e.g. flush) or className="rounded-none h-full" override to PhotoPlaceholder, remove the surrounding padding/gap for that column and let it touch the section edges; keep the Caveat 'frisch vom Kutter' note overlapping the photo's bottom-left corner. Mobile: photo on top, angeschnitten (e.g. aspect 3/2), navy text block overlapping it from below (negative margin), per concept 'Auf Mobile: Foto oben angeschnitten, Text darüber gelegt'. Keep the existing wheelIn/lineUp animation and reduced-motion guard unchanged.

## 4. [🔴 Hoch · design-ci] Header opening-status chip wraps to multiple lines and is clipped by the fixed-height top bar
**Datei:** `src/components/OpenStatusChip.tsx`

OpenStatusChip renders `<span className={"inline-flex items-center gap-2 text-sm " + base}><span .../><span>{label}</span></span>` with no whitespace-nowrap. The German label from computeStatus (src/lib/openingHours.ts) is long: 'Jetzt geöffnet – bis 22:00 Uhr' / 'Öffnet heute um 08:00 Uhr' (~27 chars). In Header.tsx it sits in `<div className="mx-auto flex h-16 ... md:h-[72px] items-center justify-between gap-4">` inside `<div className="flex items-center gap-6"><Logo/><div className="hidden md:block"><OpenStatusChip .../></div></div>`. Between md and lg (768–1023px) the desktop nav is hidden but the ReserveButton is shown, so flexbox shrinks the chip's basis; the unprotected label wraps onto up to 3 lines and the fixed h-16/h-[72px] header clips it. This matches the live screenshot.

**Fix:** 1) In OpenStatusChip.tsx add whitespace-nowrap to the label span (and shrink-0 to the dot). 2) Add a compact mode for the header: a `compact` prop that renders a short one-line label (DE: 'Geöffnet bis 22 Uhr' / 'Geschlossen' / 'Öffnet 8 Uhr'; EN analog) — either derive it in computeStatus (return both label and shortLabel) or truncate in the component. 3) In Header.tsx change the wrapper from `hidden md:block` to `hidden lg:block` (or keep md but use the compact variant with min-w-0 + truncate) so the chip never fights the logo and reserve button for space at tablet widths. Keep the full-length label in the hero and footer usages.

## 5. [🔴 Hoch · copy-psych] Invented facts and numbers beyond the allowed real set (4,9 / 77 / 8-22 / 2025)
**Datei:** `multiple`

Anti-KI-Manifest Regel 7 (binding) allows only real numbers: "Nur echte Zahlen: 4,9 . 77 Google-Bewertungen . taeglich 8-22 Uhr . Eroeffnet 2025." The code invents concrete, customer-facing facts nowhere sourced from the concept: (1) src/routes/kontakt.tsx: "VHF Kanal 12 fuer den Hafenmeister; Anmeldung im Bistro" — an invented radio channel that sailors would actually tune to; (2) src/routes/feiern.tsx: meta description "Rueckmeldung binnen 24 h" and step card "Wir melden uns binnen 24 h" — an invented SLA that even contradicts the binding response promise used lower on the same page ("meist innerhalb weniger Stunden"); (3) src/routes/bootsservice.tsx: "Wir melden uns innerhalb eines Werktages." (page intro and form success) — another invented SLA; (4) src/routes/ueber-uns.tsx: invented team members `{ name: "Jana", role: "Service & Herz" }, { name: "Tom", role: "Kueche" }, { name: "Ben", role: "Werkstatt" }` (only Kiwi is attested in the concept); (5) src/routes/index.tsx AboutTeaser: "drei Generationen an Bord" — an invented family claim.

**Fix:** Remove or replace every unverified concrete claim: drop "VHF Kanal 12" (or leave a neutral "Anmeldung im Bistro" until the operator confirms the channel); unify all response-time promises to the binding "Wir melden uns schnellstmoeglich — meist innerhalb weniger Stunden." on /feiern and /bootsservice (page copy, form success and meta); replace the invented team members Jana/Tom/Ben with real names supplied by the operator or a placeholder-free "Das Team" section without fabricated names; delete "drei Generationen an Bord" unless confirmed. Also have the operator confirm the FAQ policy claims (vegan options, oat milk free of charge, dogs inside) before launch.

## 6. [🔴 Hoch · a11y-perf] Gold text/icons on cream background in hero and 404 page (CI rule + WCAG AA violation)
**Datei:** `multiple`

The concept (Teil 2 CI, restated Teil 11 line 508: 'WCAG AA contrast (never gold text on white...)') forbids gold text on white/cream — gold is only allowed on navy or as large icons. The homepage hero is built on cream instead of the specified navy 55% text panel, and puts gold text directly on it. src/routes/index.tsx Hero: `<section className="relative bg-[color:var(--cream)]">` containing `<p className="text-xs font-semibold uppercase tracking-[0.24em] text-[color:var(--gold)]">Mittelmole · Warnemünde</p>`, the gold wheel icon `<span className="text-[color:var(--gold)] hero-wheel">` and the small 16px gold star in the trust line `<span className="text-[color:var(--gold)]"><SkipperIcon name="star" size={16} /></span>`. src/routes/en.index.tsx repeats the identical pattern. src/routes/__root.tsx NotFoundComponent: `<p className="text-xs uppercase tracking-[0.2em] text-[color:var(--gold)]">404</p>` on `bg-[color:var(--cream)]`. #FFAF04 on #FAF6EE is roughly 1.7:1 contrast — far below the 4.5:1 AA minimum for this small uppercase text. Gold on navy elsewhere (testimonial band, location band, footer language switch) is compliant and unaffected.

**Fix:** Either restore the concept's navy hero text panel (gold eyebrow then sits on navy, which is allowed), or recolor the cream-background instances: change the hero eyebrow and 404 eyebrow from text-[color:var(--gold)] to text-[color:var(--sea)] (as already done on /fruehstueck and in BentoGrid) or var(--navy). Apply the same change in src/routes/en.index.tsx. The small gold star icon in the trust line should become navy/sea or be enlarged on a navy chip.

## 7. [🔴 Hoch · a11y-perf] Mobile fullscreen menu is not keyboard/screen-reader accessible: no aria-expanded, no Escape close, no focus trap
**Datei:** `src/components/layout/Header.tsx`

The concept mandates 'keyboard-navigable menu' and 'aria-expanded on collapsibles' (Teil 11 line 508). The custom hamburger toggle is `<button type="button" aria-label={mobileOpen ? d.nav.menuClose : d.nav.menuOpen} onClick={() => setMobileOpen((v) => !v)} ...>` — it has no aria-expanded and no aria-controls. The overlay itself is a bare conditional `{mobileOpen && (<div className="fixed inset-0 z-50 flex flex-col bg-[color:var(--navy)] ...">` with: no role="dialog"/aria-modal, no Escape-key handler (keyboard users cannot close it without tabbing to the close button), no focus trap and no focus move on open (focus stays on the toggle which is now visually covered; Tab walks into the page content hidden behind the overlay), and no body scroll lock. The desktop 'Für Skipper' dropdown correctly uses Radix DropdownMenu and is fine.

**Fix:** On the hamburger button add aria-expanded={mobileOpen} and aria-controls pointing to the overlay id. For the overlay: either replace the custom div with the existing shadcn Sheet/Dialog component (Radix gives focus trap, Escape, aria-modal and scroll lock for free), or add manually: role="dialog" aria-modal="true", a useEffect keydown listener closing on Escape and returning focus to the toggle, initial focus onto the close button when opened, a focus trap, and document.body overflow lock while open.

## 8. [🟠 Mittel · seo] robots.txt is missing the mandated Sitemap directive
**Datei:** `public/robots.txt`

Concept Teil 9.2 specifies the exact robots.txt content including `Sitemap: https://skipper-warnemuende.de/sitemap.xml`. The actual file contains only:
`User-agent: *
Allow: /
Disallow: /admin`
— the Sitemap line is absent, so crawlers are not pointed at the sitemap.

**Fix:** Append the line `Sitemap: https://skipper-warnemuende.de/sitemap.xml` to public/robots.txt.

## 9. [🟠 Mittel · seo] Legacy 301 map incomplete: /pages/fischrestaurant-hummerkob.php redirect missing and mandated anchor targets dropped
**Datei:** `public/_redirects`

Concept Teil 4.3 ("KRITISCH — vor Launch einrichten, 301!") lists 10 legacy .php URLs. public/_redirects covers 9 of them but omits `/pages/fischrestaurant-hummerkob.php → /` entirely (an unrelated `/pages/posts/* /news 301` rule was added instead). Additionally two targets lose their mandated fragments: the concept requires `/pages/getraenkekarte.php → /speisekarte#getraenke` and `/pages/lage.php → /kontakt#anfahrt`, but the file has `/pages/getraenkekarte.php     /speisekarte          301` and `/pages/lage.php               /kontakt              301` without the anchors.

**Fix:** Add the line `/pages/fischrestaurant-hummerkob.php   /   301` and change the getraenkekarte.php target to /speisekarte#getraenke and the lage.php target to /kontakt#anfahrt (ensure matching anchor IDs exist on those pages).

## 10. [🟠 Mittel · seo] Meta descriptions deviate from the binding Teil 6.3 table on /speisekarte, /kontakt, /bootsservice and /feiern
**Datei:** `multiple`

Teil 6.3 is labelled "pro Seite, verbindlich". Titles all match, and / and /fruehstueck descriptions match, but four descriptions were rewritten: src/routes/speisekarte.tsx has "Unsere Speisekarte: Frühstück, Fisch vom Kutter, Klassiker, Kinderteller, Desserts, Getränke. Direkt am Yachthafen Warnemünde." instead of the mandated "Backfisch frisch aus der Pfanne, Frühstück bis 11 Uhr, Kaffee & Kuchen: die aktuelle Karte vom Bistro am Yachthafen Warnemünde."; src/routes/kontakt.tsx has "Kontakt, Anfahrt und Öffnungszeiten. Direkt am Yachthafen Warnemünde auf der Mittelmole — mit Auto, Bahn oder übers Wasser." instead of "Adresse, Öffnungszeiten, Parken & S-Bahn: So finden Sie uns auf der Mittelmole — 5 Minuten vom Bahnhof Warnemünde."; src/routes/bootsservice.tsx has "Wartung, kleine Reparaturen, Motor- und Segelcheck, Saisonvorbereitung — direkt am Yachthafen Warnemünde. Persönliche Beratung inklusive." instead of "Motor- & Segelcheck, Wartung, kleine Reparaturen direkt am Yachthafen Warnemünde. Jetzt unverbindlich anfragen."; src/routes/feiern.tsx has "Geburtstag, Firmenevent, Familienfeier direkt am Yachthafen Warnemünde. Anfrage stellen, Rückmeldung binnen 24 h, gemeinsam planen." instead of "Geburtstag, Firmenfeier oder Familienessen mit Hafenblick auf der Mittelmole. Jetzt Termin & Angebot anfragen."

**Fix:** Replace the name="description" meta content in speisekarte.tsx, kontakt.tsx, bootsservice.tsx and feiern.tsx with the exact strings from the Teil 6.3 table (and mirror the change in the og:description where present).

## 11. [🟠 Mittel · seo] DB-driven SEO content (menu, event/news details) renders only client-side; event and news detail pages ship a generic static title for every slug
**Datei:** `multiple`

Concept Teil 8.1 (line 316) mandates that SEO-critical content is secured via prerendering. But all Supabase content is fetched with client-side useQuery and no route loader, so the server-rendered HTML contains an empty menu on /speisekarte and /en/menu (the Menu JSON-LD SSRs with empty hasMenuSection) and "Wird geladen …" on detail pages. Worse, src/routes/events.$slug.tsx hardcodes `{ title: `Event — Skipper's Bistro Warnemünde` }` and description "Event am Yachthafen Warnemünde." for every event, and src/routes/news.$slug.tsx hardcodes `{ title: "Beitrag — Skipper's Bistro Warnemünde" }` — head() only receives params, never the loaded row, so every event/news URL has an identical, non-descriptive title/description even after hydration (duplicate titles across all detail pages).

**Fix:** Add TanStack route loaders (queryClient.ensureQueryData) for menu categories/items, event and news rows so SSR HTML includes the content, and derive head() from loaderData: e.g. head: ({ loaderData }) => title `${event.title} — Skipper's Bistro Warnemünde` and description from the event/news teaser; generate the Event JSON-LD from loader data as well.

## 12. [🟠 Mittel · seo] Duplicate Restaurant JSON-LD on EN pages (root DE schema + route EN schema render together)
**Datei:** `src/routes/en.index.tsx`

src/routes/__root.tsx already injects `restaurantJsonLd()` (DE) globally, and src/routes/en.index.tsx and src/routes/en.menu.tsx each add `scripts: [{ type: "application/ld+json", children: JSON.stringify(restaurantJsonLd("en")) }]` in their head(). TanStack Router merges root and route head scripts, so /en and /en/menu emit two competing Restaurant entities (inLanguage "de" and "en") with identical name/address — conflicting duplicate structured data for the same business on one page.

**Fix:** Emit exactly one Restaurant schema per page: either make the root head() locale-aware (pick restaurantJsonLd(detectLocale(pathname))) and remove the extra scripts from en.index.tsx and en.menu.tsx, or remove the Restaurant script from __root.tsx and add the correct-locale schema once per route.

## 13. [🟠 Mittel · seo] Mandated LocalBusiness secondary profile for the Bootsservice section is missing
**Datei:** `src/routes/bootsservice.tsx`

Concept Teil 9.1 (line 389) requires in addition to the Restaurant schema: "LocalBusiness-Zweitprofil für den Bootsservice-Bereich ... (`LocalBusiness` + `additionalType`)". src/routes/bootsservice.tsx contains no JSON-LD at all — only the head() meta and the Breadcrumbs component; no LocalBusiness entity describing the boat service exists anywhere in the codebase.

**Fix:** Add a JSON-LD script on /bootsservice (and /en/boat-service) with @type LocalBusiness, an additionalType hinting boat repair/maintenance, name (e.g. "Skipper's Bootsservice Warnemünde"), and the shared NAP data (Am Bahnhof 2a, 18119 Rostock-Warnemünde, +4938126054246, geo, absolute url https://skipper-warnemuende.de/bootsservice).

## 14. [🟠 Mittel · design-ci] Uniform py-20 on every homepage section violates Anti-KI-Manifest rule 8 (spacing rhythm)
**Datei:** `src/routes/index.tsx`

Concept Teil 3 rule 8 forbids 'Überall dieselben Abstände (py-16 auf allem)' and mandates a rhythm: 'große Luft um den Hero (120 px), enger im Karten-Bereich (64 px), Full-Bleed-Bänder ohne Außenabstand'. In index.tsx every single section uses the identical `py-20` (80px): TestimonialBand, BentoGrid (`px-5 py-20 md:px-8`), MenuTeaser, LocationBand, AboutTeaser, EventsTeaser and ClosingCTA all contain `py-20`; the hero uses `py-14 md:py-24` (96px, not 120px). This is exactly the template-tell pattern the manifest bans, just with py-20 instead of py-16.

**Fix:** Differentiate the vertical rhythm: hero inner padding md:py-[120px] (large air), card-based sections (BentoGrid, MenuTeaser, EventsTeaser) tightened to py-16 (64px), full-bleed navy bands (TestimonialBand, LocationBand, ClosingCTA) keep generous inner padding (py-20/py-24) with zero outer margin (already full-bleed — keep that), AboutTeaser can stay mid-weight. The result must not have the same py value on all sections.

## 15. [🟠 Mittel · design-ci] Header logo emblem rendered in gold — concept restricts the gold logo variant to the favicon only
**Datei:** `src/components/Logo.tsx`

Concept Teil 2.1 logo rules: recoloring is forbidden, 'Ausnahme: einfarbig Weiß (auf Navy-Flächen) und einfarbig Navy (auf hellen Flächen) sowie Gold-Variante nur fürs Favicon.' Logo.tsx renders the Steuerrad emblem gold in every context: `<span className="grid h-9 w-9 place-items-center rounded-full" style={{ color: "var(--gold)" }}><SkipperIcon name="wheel" size={28} /></span>` while the wordmark uses the variant color. In the navy header this produces a two-tone gold+white logo, which is neither the all-white nor the all-navy allowed variant.

**Fix:** Make the emblem follow the variant color: in Logo.tsx set the wheel span's color to the same `color` used for the wordmark (white for variant="light" on navy, var(--navy) for variant="dark" on light surfaces). Reserve the gold wheel exclusively for the favicon (gold #FFAF04 on navy #211C5C).

## 16. [🟠 Mittel · design-ci] WhatsApp CTA: white text on #25D366 fails WCAG AA contrast (~2:1)
**Datei:** `src/routes/index.tsx`

In ClosingCTA: `className="inline-flex h-11 items-center gap-2 rounded-[10px] bg-[#25D366] px-5 text-sm font-semibold text-white hover:opacity-90"`. White 14px text on #25D366 has a contrast ratio of roughly 1.9:1 — far below the 4.5:1 required by WCAG AA, which the concept declares mandatory ('Kontrast-Pflicht (WCAG AA)'). The concept's own analog rule for bright accent surfaces is explicit: on gold buttons always navy text, never white.

**Fix:** Apply the same principle as the gold buttons: keep bg-[#25D366] but set text-[color:var(--navy)] (contrast ~4.9:1), or restyle the WhatsApp CTA as a white-outline button on the navy band (border-white/30 text-white) with the label 'WhatsApp' — matching the adjacent phone button.

## 17. [🟠 Mittel · design-ci] Typo scale deviates from concept 2.3: H1/H2 exceed the clamp() maxima, eyebrows not in Archivo 800 / 0.16em
**Datei:** `src/routes/index.tsx`

Concept Teil 2.3 defines a binding clamp()-based scale: H1 `clamp(2.2rem, 5vw, 3.8rem)`, H2 `clamp(1.6rem, 3.5vw, 2.4rem)`, Eyebrow `0.78rem / letter-spacing 0.16em / uppercase / Archivo 800`. The homepage instead uses fixed Tailwind steps: hero H1 `className="mt-5 text-5xl leading-[1.02] md:text-7xl"` (md = 4.5rem, exceeding the 3.8rem max), all section H2s `text-4xl md:text-5xl` (3rem, exceeding the 2.4rem max), and eyebrows like `className="text-xs font-semibold uppercase tracking-[0.24em] ..."` — 0.75rem Inter 600 with 0.24em tracking instead of 0.78rem Archivo 800 with 0.16em.

**Fix:** Add utilities/base rules in src/styles.css — e.g. `@utility text-display-1 { font-size: clamp(2.2rem, 5vw, 3.8rem); }`, `@utility text-display-2 { font-size: clamp(1.6rem, 3.5vw, 2.4rem); }`, and `@utility eyebrow { font-family: var(--font-display); font-weight: 800; font-size: 0.78rem; letter-spacing: 0.16em; text-transform: uppercase; }` — then replace `text-5xl md:text-7xl` / `text-4xl md:text-5xl` on H1/H2 and the `text-xs font-semibold uppercase tracking-[0.24em]` eyebrow classes across the homepage (and reused page headers) with these utilities.

## 18. [🟠 Mittel · copy-psych] Binding meta descriptions from Copy-Deck 6.3 not used on 4 pages
**Datei:** `multiple`

Teil 6.3 marks per-page titles AND descriptions as "verbindlich". Titles match everywhere, but four descriptions were rewritten: src/routes/speisekarte.tsx has "Unsere Speisekarte: Fruehstueck, Fisch vom Kutter, Klassiker, Kinderteller, Desserts, Getraenke..." instead of "Backfisch frisch aus der Pfanne, Fruehstueck bis 11 Uhr, Kaffee & Kuchen: die aktuelle Karte vom Bistro am Yachthafen Warnemuende."; src/routes/bootsservice.tsx has "...Persoenliche Beratung inklusive." instead of "...Jetzt unverbindlich anfragen." (the CTA was dropped); src/routes/feiern.tsx has "...Anfrage stellen, Rueckmeldung binnen 24 h, gemeinsam planen." instead of "...Jetzt Termin & Angebot anfragen."; src/routes/kontakt.tsx has "Kontakt, Anfahrt und Oeffnungszeiten..." instead of "Adresse, Oeffnungszeiten, Parken & S-Bahn: So finden Sie uns auf der Mittelmole — 5 Minuten vom Bahnhof Warnemuende." The rewrites drop the deck's action verbs and concrete SERP hooks (Backfisch, 5 Minuten vom Bahnhof, unverbindlich anfragen). / and /fruehstueck match exactly.

**Fix:** Replace the meta description strings in the head() of src/routes/speisekarte.tsx, src/routes/bootsservice.tsx, src/routes/feiern.tsx and src/routes/kontakt.tsx with the exact texts from the Copy-Deck 6.3 table (see concept lines 281-285).

## 19. [🟠 Mittel · copy-psych] Live-status line omits the binding kitchen-close information
**Datei:** `src/lib/openingHours.ts`

Blueprint S1 (binding) specifies the hero live-status line as "● Heute geoeffnet . 8-22 Uhr . Kueche bis 21 Uhr". The label builder only produces `openUntil: (t: string) => \`Jetzt geoeffnet – bis ${t} Uhr\`` / `opensAt` / "Heute geschlossen" — the kitchen closing time never appears in the status chip anywhere (hero, header, footer, kontakt all use the same chip), even though `kitchen_close` is already part of the DayHours data model and defaults. "Kueche bis 21 Uhr" is the conversion-relevant part for guests deciding on dinner; footer/kontakt carry it only as a static side note.

**Fix:** Extend computeStatus to include the kitchen close in the open-state label, e.g. DE "Jetzt geoeffnet - bis 22 Uhr . Kueche bis 21 Uhr" (EN "Open now - kitchen until 9pm"), using the day's kitchen_close value, and render it in the hero OpenStatusChip per S1.

## 20. [🟠 Mittel · copy-psych] Testimonial band drops the '77' review count from the binding button copy
**Datei:** `src/routes/index.tsx`

Blueprint S2 (binding) specifies the button "Alle 77 Bewertungen lesen ↗" and the rating unit "von 5 . Google". The code renders `Alle Bewertungen lesen` (the concrete count — the strongest trust number — is missing even though RATING.count = 77 is imported in the file) and `<span className="text-sm text-white/80">/ 5 . Google</span>` instead of "von 5 . Google". The hero trust anchor correctly shows "4,9 bei Google . 77 Bewertungen", so the band is the only place the count is dropped.

**Fix:** Change the band link text to `Alle {RATING.count} Bewertungen lesen` and the unit text to "von 5 . Google" in the TestimonialBand component of src/routes/index.tsx (same for the EN twin in src/routes/en.index.tsx: "Read all 77 reviews").

## 21. [🟠 Mittel · copy-psych] Footer shows German weekday labels and copy fragments on English pages
**Datei:** `src/components/layout/Footer.tsx`

The shared footer hardcodes German weekday names regardless of locale: `<td className="py-1.5 text-white/70">{DAY_LABELS_DE[d]}</td>` — on /en, /en/menu etc. the opening-hours table reads "Montag ... Sonntag" next to otherwise English strings ("Opening hours", "Kitchen until 9pm"). This breaks the concept's requirement of an idiomatic, same-character EN version (Teil 2.5) on every EN page.

**Fix:** Add English day labels (e.g. DAY_LABELS_EN or a labels map inside DICT) and select by `locale` in Footer.tsx; while there, format the hours row for EN ("8am - 10pm") to match the rest of the EN copy.

## 22. [🟠 Mittel · a11y-perf] Invisible keyboard focus on 'Für Skipper' dropdown trigger and on gold/ghost CTAs over navy
**Datei:** `src/components/layout/Header.tsx`

Concept requires 'visible focus states' (Teil 11 line 508). The desktop dropdown trigger removes the focus indicator without replacement: `<DropdownMenuTrigger className="flex items-center gap-1 px-3 py-2 text-sm font-medium text-white/90 hover:text-white outline-none">` — keyboard users get no visual cue at all on this control. Additionally, src/components/ui/button.tsx uses `focus-visible:outline-none focus-visible:ring-1 focus-visible:ring-ring` where --ring is navy (#211C5C); for the gold ReserveButton rendered inside the navy header and the navy closing-CTA bands, a 1px navy ring on a navy background is effectively invisible. Logo.tsx shows the correct pattern (focus-visible:outline-2 outline-[var(--gold)]).

**Fix:** Replace outline-none on the DropdownMenuTrigger with focus-visible:outline-2 focus-visible:outline-offset-2 focus-visible:outline-[var(--gold)] (matching Logo.tsx). For buttons used on navy surfaces, add a visible focus style, e.g. give ReserveButton focus-visible:ring-2 focus-visible:ring-[color:var(--gold)] focus-visible:ring-offset-2 focus-visible:ring-offset-[color:var(--navy)] or switch the global ring width to 2px with a ring-offset so it reads on both cream and navy.

## 23. [🟠 Mittel · a11y-perf] White text on WhatsApp-green button fails WCAG AA contrast (~2:1)
**Datei:** `multiple`

In the ClosingCTA of both src/routes/index.tsx and src/routes/en.index.tsx: `<a href={CONTACT.whatsapp} ... className="inline-flex h-11 items-center gap-2 rounded-[10px] bg-[#25D366] px-5 text-sm font-semibold text-white hover:opacity-90">WhatsApp</a>`. White (#FFFFFF) on #25D366 has a contrast ratio of about 2.0:1; text-sm (14px semibold) requires 4.5:1 under WCAG AA. #25D366 is also outside the strict brand palette (Teil 2/Teil 11 'no substitutes'), so this element breaks both the AA requirement and the CI token rule.

**Fix:** Change the label color to navy: text-[color:var(--navy)] on the green (contrast ~7:1 passes AA), or better, restyle the WhatsApp CTA using palette tokens (e.g. ghost white-border button like the phone CTA next to it, with a WhatsApp icon) so no off-palette background is introduced. Apply in both index.tsx and en.index.tsx.

## 24. [🟠 Mittel · a11y-perf] Sticky mobile bottom bar targets are ~44px, below the mandated 48px, and it covers the footer copyright row
**Datei:** `src/components/layout/MobileBottomBar.tsx`

Concept Teil 11 line 466 mandates 48px touch targets for the mobile UI. Both bar controls use `className="flex flex-1 items-center justify-center gap-2 py-3 text-sm font-semibold"` — py-3 (12px) + 20px line-height yields ~44px hit height. Content overlap is only half-solved: __root.tsx compensates with `<main className="flex-1 pb-20 md:pb-0">`, but the Footer renders after main, so the fixed bar (z-30) permanently overlays the footer's last row (`<div className="... px-5 py-4 text-xs text-white/50 ...">© ... </div>`) on mobile — it can never be scrolled into view.

**Fix:** Add min-h-12 (48px) to both the tel link and the reserve button (e.g. className="flex min-h-12 flex-1 items-center justify-center..."). Move the bottom-bar compensation from <main> to the footer or the page wrapper: e.g. remove pb-20 from main and add pb-20 md:pb-0 to the last footer row (or give the layout div itself the padding) so the copyright/legal row is reachable above the fixed bar.

## 25. [🟠 Mittel · a11y-perf] Web font loading exceeds the hard performance budget: 9 font files instead of max 6, no Archivo 900 preload, render-blocking third-party CSS
**Datei:** `src/routes/__root.tsx`

Teil 10 budget (hart): 'Fonts | max. 3 Familien / 6 Dateien, WOFF2, preload für Archivo 900'; Teil 11 line 507: 'fonts WOFF2 with font-display: swap, preload Archivo 900'. The head loads `href: "https://fonts.googleapis.com/css2?family=Archivo:wght@500;700;800;900&family=Inter:wght@400;500;600;700&family=Caveat:wght@600&display=swap"` — that is 9 weight files (Archivo 500/700 are not even used anywhere: the concept and styles.css use Archivo only at 800/900 for headings and font-black for the logo). display=swap and preconnect are present (good), but Archivo 900 cannot be preloaded from the Google-hosted CSS, so the H1 font arrives late; the stylesheet is also render-blocking third-party CSS on the critical path and an external request fired before any consent (relevant under German GDPR case law on remote Google Fonts).

**Fix:** Preferred (concept): self-host WOFF2 files in /public/fonts — Archivo 800+900, Inter 400/500/600/700, Caveat 600 (6 files) — declare @font-face with font-display: swap in styles.css, and add <link rel="preload" as="font" type="font/woff2" crossorigin> for Archivo 900 in the __root head. Minimum fix if staying on Google Fonts: drop Archivo 500;700 from the css2 URL to get to 7 files and trim Inter weights if feasible.

## 26. [🟠 Mittel · security] No server-side input validation or rate limit — edge function relays arbitrary unvalidated payloads to admin inboxes
**Datei:** `supabase/functions/notify-reservation/index.ts`

The concept mandates server-side validation and rate limiting for anon inserts (Teil 8.2, line 346: "anon insert: for insert with check (true) -- plus Edge-Function-Rate-Limit + Honeypot"; line 349: "Formular-Inputs serverseitig validieren (zod-Schema in Edge Function spiegeln)"; prompt section 5 line 527: "INSERT for anon (with zod-mirrored edge validation)"). Neither exists. Inserts go straight from the browser to PostgREST under the `WITH CHECK (true)` policy, so the zod schemas, honeypot, and time-trap are client-side only and trivially bypassed with a direct API call using the public key; the only server-side constraints are NOT NULLs and `party_size BETWEEN 1 AND 20` — name/phone/note/message text columns have no length limits at all. The notify edge function performs zero validation: `const kind: string = json?.kind ?? "unknown"; const payload: Record<string, unknown> = json?.payload ?? {};` and then emails whatever it received via `renderBody(...)` to `ADMIN_RECIPIENTS = ["info@skippers-bistro.de", "info@gradore.de"]` — an unauthenticated spam relay: anyone with the public publishable key can trigger unlimited emails with attacker-controlled content to the owners' inboxes.

**Fix:** In supabase/functions/notify-reservation/index.ts, mirror the client zod schemas per `kind` (whitelist kind to the three known values; validate/strip payload fields to the exact expected keys with the same max lengths; reject anything else with 400) and add a simple rate limit (e.g. per-IP counter in a Supabase table or upstash-style check, or at minimum cap payload size and reject repeated calls). Additionally add DB-level CHECK constraints in a new migration mirroring the zod limits (e.g. char_length(name) <= 100, char_length(phone) <= 40, char_length(note) <= 500, char_length(message) <= 1000, char_length(contact) <= 200) so direct PostgREST inserts cannot store oversized/garbage rows.

## 27. [🟠 Mittel · security] GDPR consent requirement only half-implemented on every form (checkbox and /datenschutz link never appear together)
**Datei:** `multiple`

Concept GDPR layer (line 529) requires: "GDPR checkbox on all forms with link to /datenschutz". No form satisfies both halves. (a) src/components/reservation/ReservationForm.tsx (used by the reservation modal on DE and EN pages) has no checkbox at all — only a passive hint: `{t.form.privacyHint} <a className="underline" href="/datenschutz">{t.form.privacyLink}</a>` ("Mit dem Absenden akzeptieren Sie unsere Datenschutzhinweise"), and its zod schema has no `gdpr` field. (b) src/routes/bootsservice.tsx, src/routes/feiern.tsx and src/routes/en.boat-service.tsx do have a required checkbox (`gdpr: z.literal(true, ...)`) but the label contains no link to /datenschutz — e.g. bootsservice.tsx: `Ich willige ein, dass meine Angaben zur Bearbeitung der Anfrage verarbeitet werden (Art. 6 Abs. 1 lit. b DSGVO).` with no anchor element.

**Fix:** 1) In src/components/reservation/ReservationForm.tsx add a required GDPR consent Checkbox (zod `gdpr: z.literal(true)` like the other forms) whose label keeps the existing link to /datenschutz, with DE/EN label strings added to DICT in src/lib/i18n.ts. 2) In src/routes/bootsservice.tsx, src/routes/feiern.tsx and src/routes/en.boat-service.tsx, extend the existing checkbox label to include an anchor to /datenschutz (e.g. "… verarbeitet werden. Details in den <a href=\"/datenschutz\">Datenschutzhinweisen</a>." / EN: "… See our <a href=\"/datenschutz\">privacy notice</a>.").

## 28. [🟡 Niedrig · seo] Restaurant JSON-LD deviates from the concept template: acceptsReservations is a boolean and image is empty; currenciesAccepted missing
**Datei:** `src/components/RestaurantJsonLd.tsx`

The concept's Restaurant schema (Teil 9.1) specifies `"acceptsReservations": "https://skipper-warnemuende.de/#reservieren"` (a reservation URL) and `"currenciesAccepted": "EUR"`. The code has `acceptsReservations: true`, `image: []` (an empty array adds nothing and Restaurant rich results want at least one image — can point at the future OG/hero asset once photos exist), and no currenciesAccepted property.

**Fix:** Set acceptsReservations to the absolute reservation URL "https://skipper-warnemuende.de/#reservieren", add currenciesAccepted: "EUR", and either drop the empty image array or populate it once the hero photo exists.

## 29. [🟡 Niedrig · seo] Sitemap has no lastmod and omits published event/news detail URLs
**Datei:** `src/routes/sitemap[.]xml.ts`

Concept Teil 9.2 line 401 requires "alle DE+EN-URLs, lastmod gepflegt (Build-Step generiert)". The handler builds entries with only changefreq/priority — no <lastmod> element is ever emitted — and the static entries list omits the indexable dynamic pages /events/$slug and /news/$slug even though the route is a server handler that could query Supabase for published slugs.

**Fix:** In the sitemap GET handler, add a lastmod field per entry (e.g. deploy date or per-row updated_at) and query Supabase for published events/news slugs to append their URLs (with event_date/published_at as lastmod).

## 30. [🟡 Niedrig · seo] BreadcrumbList missing on /speisekarte and /en/menu
**Datei:** `src/routes/speisekarte.tsx`

Concept Teil 9.1 line 389 mandates "BreadcrumbList auf Unterseiten". The Breadcrumbs component (which emits BreadcrumbList JSON-LD) is used on /fruehstueck, /kontakt, /bootsservice, /feiern and the detail pages, but src/routes/speisekarte.tsx and src/routes/en.menu.tsx render no Breadcrumbs at all — the page starts directly with the hero section.

**Fix:** Add `<Breadcrumbs items={[{ label: "Start", to: "/" }, { label: "Speisekarte" }]} />` above the hero on /speisekarte and the EN equivalent ({ label: "Home", to: "/en" }, { label: "Menu" }) on /en/menu.

## 31. [🟡 Niedrig · design-ci] Menu prices set in font-mono — introduces a fourth typeface outside the Archivo/Inter/Caveat system
**Datei:** `src/routes/index.tsx`

MenuTeaser renders prices as `<span className="font-mono text-lg font-semibold text-[color:var(--navy)]">`. Concept 2.3 defines exactly three fonts (Archivo display, Inter body, Caveat accent); a system monospace font is not part of the CI.

**Fix:** Replace font-mono with the brand system: e.g. `font-semibold text-[color:var(--navy)] [font-variant-numeric:tabular-nums]` (Inter with tabular figures) or Archivo 800 via `style={{ fontFamily: "var(--font-display)" }}`. Apply the same change anywhere else prices use font-mono (check /speisekarte).

## 32. [🟡 Niedrig · design-ci] Eyebrows on light sections use --sea instead of --wood, against the token role table
**Datei:** `src/routes/index.tsx`

Concept 2.2 assigns `--skipper-wood #8B6F47` the role 'Eyebrows im hellen Kontext' and `--skipper-sea` to 'Links, Info-Boxen, sekundäre Buttons, Bootsservice-Bereich'. On the homepage all light-context eyebrows use sea: BentoGrid `<p className="text-xs uppercase tracking-[0.24em] text-[color:var(--sea)]">Ein Haus, vier Gründe</p>`, likewise MenuTeaser ('Aus der Küche'), AboutTeaser ('Über uns') and EventsTeaser ('Was ansteht'). Gold eyebrows on navy bands are correct and should stay.

**Fix:** Change eyebrow color on cream/paper sections from text-[color:var(--sea)] to text-[color:var(--wood)] (BentoGrid, MenuTeaser, AboutTeaser, EventsTeaser, and equivalent eyebrows on subpages). Keep gold eyebrows on navy surfaces.

## 33. [🟡 Niedrig · design-ci] Unused .dark theme block remaps all tokens to off-brand slate/purple oklch values
**Datei:** `src/styles.css`

styles.css contains a full `.dark { --background: oklch(0.129 0.042 264.695); ... --primary: oklch(0.929 0.013 255.508); ... }` block of generic shadcn slate/blue-purple values. No ThemeProvider applies .dark today, but if the class ever lands on the root (e.g. via a future Lovable edit or OS-level tooling) the entire binding CI is replaced — including the rule that the page background is never anything but cream, and colors drift into the forbidden blue/purple range.

**Fix:** Either delete the .dark block entirely (the concept defines no dark mode) or remap it to brand values (background var(--navy-dark), card var(--navy), foreground #FFFDF8, accent var(--gold)) so an accidental dark toggle still renders on-brand.

## 34. [🟡 Niedrig · design-ci] 404 page: small gold text on cream (forbidden contrast combination)
**Datei:** `src/routes/__root.tsx`

NotFoundComponent renders `<p className="text-xs uppercase tracking-[0.2em] text-[color:var(--gold)]">404</p>` on `bg-[color:var(--cream)]`. Concept 2.2: gold only for large text/icons or on navy — 'Gold-Text auf Weiß ist verboten — zu wenig Kontrast' (cream is near-white; #FFAF04 on #FAF6EE is ~1.6:1).

**Fix:** Change the 404 eyebrow color to text-[color:var(--wood)] (the designated eyebrow color in light contexts), or place it on a navy chip if gold must stay.

## 35. [🟡 Niedrig · copy-psych] EN copy slips: 'Moin?' instead of binding 'Moin!', German quotation marks and 24h clock in EN
**Datei:** `src/routes/en.index.tsx`

Concept 2.5 fixes the EN signature line as "Moin! That's how we say hello up north." The code renders `<p className="hand mt-3 text-lg ...">Moin? That's how we say hello up north.</p>` — the question mark turns the greeting into a question. Additionally the EN testimonial cards wrap English quotes in German low-9 quotation marks (`„{q.quote_en ...}"`), the big band rating is rendered as "★ 4.9" (star glyph inside the Archivo number, unlike the DE version's plain "4,9"), and the EN open-status label from openingHours.ts says "Open now — until 22:00" (24h clock) while the EN footer says "Kitchen until 9pm".

**Fix:** In src/routes/en.index.tsx change "Moin?" to "Moin!", use English curly quotes for testimonial text, and render the band number as "4.9" without the star; in src/lib/openingHours.ts format EN times as 12-hour ("until 10pm").

## 36. [🟡 Niedrig · copy-psych] Caveat handwriting note 'frisch vom Kutter nebenan' rendered as a badge, and only conditionally
**Datei:** `src/routes/index.tsx`

Copy-Deck 6.2 specifies for the menu teaser: Note in Caveat handwriting "frisch vom Kutter nebenan". In MenuTeaser the string is rendered as a pill badge, not handwriting: `<span className="mt-3 inline-block rounded-full bg-[color:var(--gold-soft)] px-2.5 py-0.5 text-xs font-semibold ...">frisch vom Kutter nebenan</span>`, and only when `it.is_seasonal` is true — so it can be absent entirely. The `hand` (Caveat) class is only used for "Kiwis Tipp" here; the hero's hand note shortens the phrase to "frisch vom Kutter" (missing "nebenan").

**Fix:** In the MenuTeaser of src/routes/index.tsx render "frisch vom Kutter nebenan" with the `hand` Caveat class as a handwritten note on one dish card (independent of is_seasonal, or guaranteed on the fish dish), keeping the seasonal pill as a separate element if desired.

## 37. [🟡 Niedrig · copy-psych] Micro-deviations from binding copy: menu-teaser CTA, service pillar, footer dash
**Datei:** `multiple`

Three small exact-copy deviations: (1) src/routes/index.tsx MenuTeaser link reads "Ganze Karte" while Blueprint S4 specifies the CTA "Die ganze Karte ansehen"; (2) the Bootsservice bento tile title "Wartung & Reparatur" drops "Saisonstart" from the deck text "Wartung, kleine Reparaturen, Saisonstart: ..." (body sentence matches); (3) src/lib/i18n.ts footer weather line uses an en-dash: "Terrassenbetrieb, wenn's der Wind gut meint – im Zweifel kurz durchklingeln." while the deck (6.2) uses an em-dash "—", the punctuation used consistently elsewhere on the site.

**Fix:** Set the menu-teaser link label to "Die ganze Karte ansehen", extend the service tile copy to include "Saisonstart" (e.g. title "Wartung, Reparatur, Saisonstart"), and replace "–" with "—" in DICT.de.footer.terraceNote.

## 38. [🟡 Niedrig · a11y-perf] Event/News detail images lack loading="lazy" and explicit dimensions
**Datei:** `multiple`

Teil 11 line 507 requires 'all below-fold images loading="lazy"'; Teil 13 requires width/height on all images. In src/routes/events.$slug.tsx and src/routes/news.$slug.tsx the only real <img> tags of the public site render as `<img src={event.image_url} alt={event.title} className="aspect-video w-full rounded-[14px] object-cover" />` — alt text is present and aspect-video prevents CLS, but there is no loading="lazy" (the image sits below breadcrumb + date + H1 (+ teaser), i.e. below the first mobile viewport) and no width/height attributes as a no-CSS fallback.

**Fix:** Add loading="lazy" decoding="async" and explicit width={1280} height={720} attributes to the <img> in both events.$slug.tsx and news.$slug.tsx (the Tailwind classes keep controlling rendered size; the attributes give the browser intrinsic dimensions).

## 39. [🟡 Niedrig · a11y-perf] Menu scrollspy: smooth scrollIntoView ignores prefers-reduced-motion and active tab lacks aria-current
**Datei:** `src/routes/speisekarte.tsx`

The category tab click handler always animates: `refs.current[c.id]?.scrollIntoView({ behavior: "smooth", block: "start" })`. The global CSS reduced-motion block in styles.css does not affect JS-initiated smooth scrolling, so motion-sensitive users still get the animation (concept: 'Respect prefers-reduced-motion'). Additionally the visually highlighted active tab (`active === c.id ? "bg-[color:var(--navy)] text-white" : ...`) exposes no state to assistive tech — no aria-current on the active category link.

**Fix:** Gate the behavior: const reduce = window.matchMedia("(prefers-reduced-motion: reduce)").matches; then scrollIntoView({ behavior: reduce ? "auto" : "smooth", block: "start" }). Add aria-current={active === c.id ? "true" : undefined} to the category <a> elements.
