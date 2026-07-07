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
| 1 | Design-System (Tokens, Archivo/Inter/Caveat, WaveDivider, SkipperIcon-Set, PhotoPlaceholder, Logo-Interim), Header mit Live-Öffnungsstatus, Footer „Törnplan", 404, Homepage (8 Sektionen), Supabase-Schema + RLS + Seed, Reservierungs-Modal, `/speisekarte` aus DB, robots/sitemap/`_redirects`/Restaurant-JSON-LD | 🔄 läuft |
| 2 | Alle Unterseiten (`/fruehstueck`, `/terrasse`, `/events`, `/news`, `/feiern`, `/bootsservice`, `/bootszubehoer`, `/shop`, `/ueber-uns`, `/kontakt`, Rechtstexte) + Formulare + per-Page-JSON-LD | ⏳ |
| 3 | `/admin` (Supabase Auth, Whitelist: info@skippers-bistro.de, info@gradore.de) + Edge-Function-Mail-Stub | ⏳ |
| 4 | `/en`-Routen + i18n + hreflang | ⏳ |
| 5 | Audit-Runde (SEO/Psychologie/Anti-KI-Manifest) + Fixes | ⏳ |

## Verbindliche Quellen

- [`docs/relaunch-konzept.md`](docs/relaunch-konzept.md) — Masterdokument (CI-Tokens, Copy-Deck, Sitemap, SEO-Blueprint, Supabase-Schema). Hierarchie: Teil 2 (CI) + Teil 3 (Anti-KI-Manifest) unverhandelbar.
- [`docs/website-check.html`](docs/website-check.html) — Audit der Altseite (27/100), Begründung des Relaunches.
- [`docs/lovable-master-prompt.md`](docs/lovable-master-prompt.md) — der exakte an Lovable gesendete Master-Prompt (Teil 11 des Konzepts + Redirects/Meta-Erweiterungen).

## Offene Punkte (vom Kunden einsammeln — Teil 12 des Konzepts)

- Logo-Originaldatei (SVG/AI) → ersetzt die Interim-`<Logo />`-Komponente. **Hinweis:** das gelieferte Logo-Bild (Möwe, Navy/Rot „SKIPPER'S BISTRO") weicht vom im Konzept beschriebenen Steuerrad-Emblem ab — vor Einbau mit Kunde/Agentur klären, welche Datei verbindlich ist.
- Echte Fotos gemäß Shooting-Liste (PhotoPlaceholder-Plates sind mit Motiv-Beschriftungen vorbereitet).
- Finale Speisekarte + Allergene (LMIV), Google-Business-Profil-URL, Rechtstexte (Impressum/Datenschutz juristisch prüfen).
- Entscheidung Reservierungstool Phase 1b (resmio/OpenTable/Quandoo) — Formular ist Fallback.
