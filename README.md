# Skipper's Bistro Warnemünde — Website-Relaunch

Neubau von `skipper-warnemuende.de` auf **Lovable** (React + TypeScript + Tailwind + shadcn/ui + Supabase), nach dem verbindlichen Masterdokument in [`docs/relaunch-konzept.md`](docs/relaunch-konzept.md).

## Lovable-Projekt

| | |
|---|---|
| **Projekt-ID** | `c410c7fe-e1e1-4b85-bc91-5cef9f3254ea` |
| **Workspace** | Manuel's Lovable |
| **Editor** | https://lovable.dev/projects/c410c7fe-e1e1-4b85-bc91-5cef9f3254ea |
| **Preview** | https://id-preview--c410c7fe-e1e1-4b85-bc91-5cef9f3254ea.lovable.app |
| **Ziel-Domain** | skipper-warnemuende.de (Cutover per Launch-Checkliste, Teil 13 des Konzepts) |

## Build-Plan (phasenweise)

| Phase | Inhalt | Status |
|---|---|---|
| 1 | Design-System (Tokens, Archivo/Inter/Caveat, WaveDivider, SkipperIcon-Set, PhotoPlaceholder, Logo-Interim), Header mit Live-Öffnungsstatus, Footer „Törnplan", 404, Homepage (8 Sektionen), Supabase-Schema + RLS + Seed, Reservierungs-Modal, `/speisekarte` aus DB, robots/sitemap/`_redirects`/Restaurant-JSON-LD | ✅ |
| 2 | Alle Unterseiten (`/fruehstueck`, `/terrasse`, `/events`, `/news`, `/feiern`, `/bootsservice`, `/bootszubehoer`, `/shop`, `/ueber-uns`, `/kontakt`, Rechtstexte) + Formulare + per-Page-JSON-LD | ✅ |
| 3 | `/admin` (Supabase Auth, Whitelist: info@skippers-bistro.de, info@gradore.de) + Edge-Function-Mail-Stub, Storage-Galerie | ✅ |
| 4 | `/en`-Routen + i18n + hreflang + EN-Sitemap | ✅ |
| 5 | Audit-Runde: 39 verifizierte Findings ([`docs/audit-findings.md`](docs/audit-findings.md)) — alle in 3 Fix-Runden umgesetzt (u. a. absolute URLs/hreflang/Canonicals, aggregateRating nur bei sichtbaren Reviews, Navy-Split-Hero nach Anti-KI-Regel 3.1, SSR-Loader für DB-Inhalte, Fontsource-Self-Hosting statt Google-Fonts-CDN, globale Focus-Ringe, DB-CHECK-Constraints, DSGVO-Checkbox in allen Formularen) | ✅ |

**Stand nach Abschluss:** Typecheck clean · Lighthouse-relevante Punkte (LCP-Budget, CLS-feste Placeholder, font-display swap, lazy images, keine Third-Party-Scripts vor Interaktion) umgesetzt · Sitemap mit xhtml:link-hreflang + lastmod + Event/News-URLs · 301-Redirect-Map vollständig.

## Verbindliche Quellen

- [`docs/relaunch-konzept.md`](docs/relaunch-konzept.md) — Masterdokument (CI-Tokens, Copy-Deck, Sitemap, SEO-Blueprint, Supabase-Schema). Hierarchie: Teil 2 (CI) + Teil 3 (Anti-KI-Manifest) unverhandelbar.
- [`docs/website-check.html`](docs/website-check.html) — Audit der Altseite (27/100), Begründung des Relaunches.
- [`docs/lovable-master-prompt.md`](docs/lovable-master-prompt.md) — der exakte an Lovable gesendete Master-Prompt (Teil 11 des Konzepts + Redirects/Meta-Erweiterungen).

## Nächste Schritte (Betreiber/Agentur)

1. **Erster Admin-Login:** auf `/admin` mit info@skippers-bistro.de oder info@gradore.de registrieren — die Whitelist vergibt die Admin-Rolle automatisch.
2. **Deploy/Publish:** im Lovable-Editor publishen; danach Custom Domain `skipper-warnemuende.de` aufschalten (Launch-Checkliste Teil 13: DNS, SSL, www→non-www, Redirect-Stichprobe, Search Console).
3. **Resend-API-Key** in den Projekt-Secrets hinterlegen → Mail-Benachrichtigungen für Reservierungen/Anfragen gehen live (Sender-Domain vorher verifizieren).
4. **Google-Business-Profil-URL** gegen die Konstante `GOOGLE_PROFILE_URL` tauschen; GBP-Links (Website, Menü, Reservierung) aktualisieren.

## Offene Punkte (vom Kunden einsammeln — Teil 12 des Konzepts)

- Logo-Originaldatei (SVG/AI) → ersetzt die Interim-`<Logo />`-Komponente. **Hinweis:** das gelieferte Logo-Bild (Möwe, Navy/Rot „SKIPPER'S BISTRO") weicht vom im Konzept beschriebenen Steuerrad-Emblem ab — vor Einbau mit Kunde/Agentur klären, welche Datei verbindlich ist.
- Echte Fotos gemäß Shooting-Liste (PhotoPlaceholder-Plates sind mit Motiv-Beschriftungen vorbereitet).
- Finale Speisekarte + Allergene (LMIV), Google-Business-Profil-URL, Rechtstexte (Impressum/Datenschutz juristisch prüfen).
- Entscheidung Reservierungstool Phase 1b (resmio/OpenTable/Quandoo) — Formular ist Fallback.
