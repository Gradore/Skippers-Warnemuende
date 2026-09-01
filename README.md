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

## CI-Update (Runde 2, 2026-07-07): Farben vom echten Logo abgeleitet

Der Betreiber hat das verbindliche Logo geliefert (Möwe + „SKIPPER'S BISTRO", Navy/Rot). Gesampelte Markenfarben: **Navy #211C5C** (identisch zum bisherigen Token) und **Logo-Rot #E30021**. Der Gold-Akzent aus dem ursprünglichen Konzept wurde vollständig durch das Logo-Rot ersetzt (`--red #E30021`, `--red-deep #B80019`, `--red-soft #FBE5E9`); Kontrast geprüft (Weiß auf Rot 4,9:1 AA). Header auf hell (Paper) umgestellt, damit das Logo (JPG, weißer Hintergrund) sauber sitzt; Footer/Mobile-Sheet nutzen eine weiße Möwen-SVG-Marke; Favicon = Möwe weiß auf Navy. **Abweichung vom Konzept Teil 2.2 ist beabsichtigt und vom Kunden beauftragt** („CI vom Logo ableiten").

Außerdem eingebaut: 5 echte Fotos (Terrasse → Hero + /terrasse + og:image, Gäste → Bento/Feiern, Scampi → Menü-Teaser, Segelcrew → Bootsservice, Online-Shop → /shop) und **Allergene sichtbar an jedem Gericht** (LMIV-Buchstabencodes in Seed-Daten, Anzeige auf /speisekarte, /en/menu und im Homepage-Teaser, Legende druckbar).

## Ausbau-Runde 3 (2026-08-27): Gericht-Bilder, öffentliche Galerie, Draft-Seeds

- **Speisekarten-Verwaltung im Admin jetzt vollständig:** Name, Beschreibung (DE/EN), Preis (Inline-Edit), Allergene (Multi-Select) und **Bild pro Gericht** (Upload JPG/PNG/WebP bis 6 MB in Supabase Storage, Vorschau, „Bild entfernen"; Migration `menu_items.image_url`). Öffentliche Karte zeigt runde Thumbnails nur bei vorhandenem Bild; Homepage-Teaser zieht Bilder aus der DB (Scampi-Foto jetzt DB-gepflegt statt Code-Hack).
- **Öffentliche Galerie `/galerie`** (Konzept F9): Masonry, Kategorie-Filter (Essen/Terrasse/Hafen/Team), barrierefreie Lightbox (Escape/Pfeile/Focus-Trap), mit den 5 vorhandenen Fotos geseedet; Footer-Link DE/EN + „EINBLICKE"-Streifen auf der Startseite; Sitemap/Breadcrumbs/OG gepflegt.
- **Draft-Seeds:** 2 Events + 1 News als unveröffentlichte „[ENTWURF] Beispiel:…"-Vorlagen, damit der Kunde den Workflow im Admin sieht — öffentlich unsichtbar, keine erfundenen Inhalte.
- Nebenbei behoben: Footer-Link-Kontrast nach der CI-Umstellung.

**Hoster der Alt-Domain (per DENIC-WHOIS geklärt, 2026-08-27):**
- Nameserver `docks16.rzone.de` / `shades11.rzone.de` → **rzone.de = STRATO**: Webhosting + DNS der Altseite laufen auf einem Strato-Paket.
- Verwaltendes DENIC-Mitglied: **InterNetX GmbH** (Registrar-Großhändler) → die Domain-Registrierung läuft über einen InterNetX-Reseller, nicht direkt über Strato.
- Domaininhaber-Kontakt: `info@hanse-sound.com` — eine **dritte Partei** (vermutlich der alte Dienstleister). DENIC-Bestätigungslinks/AuthInfo-Mails landen dort!
- **Cutover-Konsequenz:** Strato-Zugang des Kunden beschaffen (DNS-Records dort auf Lovable zeigen lassen) ODER Domain per AuthInfo/KK zum Wunsch-Provider umziehen — dafür Koordination mit dem hanse-sound-Kontakt nötig. Inhaber-E-Mail bei DENIC auf eine Kundenadresse ändern lassen (Ownership sichern). Altes Strato-Hosting erst nach 4 Wochen Puffer kündigen (Launch-Checkliste Teil 13).

## Verbindliche Quellen

- [`docs/relaunch-konzept.md`](docs/relaunch-konzept.md) — Masterdokument (CI-Tokens, Copy-Deck, Sitemap, SEO-Blueprint, Supabase-Schema). Hierarchie: Teil 2 (CI) + Teil 3 (Anti-KI-Manifest) unverhandelbar.
- [`docs/website-check.html`](docs/website-check.html) — Audit der Altseite (27/100), Begründung des Relaunches.
- [`docs/lovable-master-prompt.md`](docs/lovable-master-prompt.md) — der exakte an Lovable gesendete Master-Prompt (Teil 11 des Konzepts + Redirects/Meta-Erweiterungen).

## GEO/AEO-Runde (2026-09-01): Auffindbarkeit in KI-Assistenten (ChatGPT, Gemini, Claude, Perplexity)

- **robots.txt:** explizite Allow-Blöcke für 17 KI-Crawler (GPTBot, OAI-SearchBot, ChatGPT-User, ClaudeBot, Claude-User, Claude-SearchBot, anthropic-ai, Google-Extended, PerplexityBot, Perplexity-User, Bingbot, CCBot, Applebot(-Extended), Meta-ExternalAgent, Amazonbot, DuckAssistBot) + Hinweis auf /llms.txt.
- **/llms.txt** (llmstxt.org-Konvention): maschinenlesbare Fakten (NAP, Koordinaten, Zeiten, 4,9★/77, Anreise) + Seitenliste mit absoluten URLs + EN-Absatz. Nur verifizierte Fakten.
- **/faq + /en/faq:** 8 wörtlich zitierfähige Antworten (Lage, Zeiten, Frühstück, Reservierung, Anlegen per Boot, Hunde, Parken, Zubehör/Service) mit FAQPage-JSON-LD (de/en), hreflang, Footer-Link, Sitemap.
- **Entity-Signale:** `sameAs` (Google-Profil, erweiterbar) im Restaurant-JSON-LD; NAP-Konsistenz geprüft (alles aus einer `CONTACT`-Konstante); Startseite trägt einen crawlbaren Volltext-Entity-Satz.

**Off-Site-To-dos (nur Betreiber/Agentur, siehe Abschlussbericht):** Bing Webmaster Tools + Sitemap (ChatGPT-Suche nutzt Bing), Google Business Profile pflegen (Gemini), TripAdvisor claimen, Hafen-/Ostsee-Portale, Bewertungen sammeln.

## Nächste Schritte (Betreiber/Agentur)

1. **Erster Admin-Login:** auf `/admin` mit info@skippers-bistro.de oder info@gradore.de registrieren — die Whitelist vergibt die Admin-Rolle automatisch.
2. **Deploy/Publish:** im Lovable-Editor publishen; danach Custom Domain `skipper-warnemuende.de` aufschalten (Launch-Checkliste Teil 13: DNS, SSL, www→non-www, Redirect-Stichprobe, Search Console).
3. **Resend-API-Key** in den Projekt-Secrets hinterlegen → Mail-Benachrichtigungen für Reservierungen/Anfragen gehen live (Sender-Domain vorher verifizieren).
4. **Google-Business-Profil-URL** gegen die Konstante `GOOGLE_PROFILE_URL` tauschen; GBP-Links (Website, Menü, Reservierung) aktualisieren.

## Offene Punkte (vom Kunden einsammeln — Teil 12 des Konzepts)

- ~~Logo-Originaldatei~~ ✅ eingebaut (JPG, 1000×278). Für gestochen scharfe Darstellung/Retina später SVG- oder hochauflösende PNG-Version mit Transparenz nachliefern.
- Echte Fotos gemäß Shooting-Liste — 5 Motive bereits eingebaut; Hero-Terrassenfoto liegt nur in 500×308 vor → **höher aufgelöste Version nachliefern**. Restliche Slots weiterhin als beschriftete PhotoPlaceholder.
- Finale Speisekarte + Allergene (LMIV), Google-Business-Profil-URL, Rechtstexte (Impressum/Datenschutz juristisch prüfen).
- Entscheidung Reservierungstool Phase 1b (resmio/OpenTable/Quandoo) — Formular ist Fallback.
