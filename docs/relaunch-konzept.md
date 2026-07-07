# RELAUNCH-KONZEPT: skipper-warnemuende.de
## Masterdokument für den Neubau mit Claude + Lovable

**Projekt:** Skipper's Bistro · Shop · Bootszubehör · Bootsservice — Mittelmole, Warnemünde
**Erstellt:** Juli 2026 · Gradore Webdesign, Rostock
**Zweck dieses Dokuments:** Vollständige Bau-Anleitung. Jede Claude-Instanz und jeder Lovable-Run soll mit diesem Dokument allein die Website in konsistenter Qualität bauen, erweitern und pflegen können. Bei Widersprüchen gilt: **dieses Dokument > Bauchgefühl > generische Best Practices.**

---

# TEIL 1 — PROJEKT-STECKBRIEF

## 1.1 Ausgangslage (Kurzfassung aus dem Audit)

| Faktor | Ist-Zustand |
|---|---|
| Aktuelle Website | WebsiteBaker-CMS (~2010), XHTML, 22–60+ Sek. Ladezeit, 17 CSS-Dateien, kein Schema, keine Sitemap/robots.txt, kein Canonical, kein Open Graph |
| Gesamt-Score Audit | 27/100 (Design 28 · Funktionen 25 · Technik 18 · Psychologie 22 · SEO 35 · Personas-Fit 40) |
| Google Business | **4,9 ★ / 77 Bewertungen** — der stärkste ungenutzte Asset |
| Betrieb | Eröffnet 01.08.2025. Täglich 8–22 Uhr. Bistro + Online-Shop-Versprechen + Bootszubehör (Ladengeschäft) + Bootsservice |
| Adresse | Am Bahnhof 2a, 18119 Warnemünde (Standort: Mittelmole am Yachthafen) |
| Telefon | Reservierung 0381 260 54 246 · Mobil/WhatsApp 0172 400 72 02 |
| E-Mail | info@skippers-bistro.de (Achtung: abweichende Domain — beim Relaunch klären, ob info@skipper-warnemuende.de eingerichtet wird; bis dahin bestehende Adresse verwenden) |

## 1.2 Projektziele (messbar)

1. **Reservierungen:** 30 % aller Reservierungen laufen 6 Monate nach Launch online/über WhatsApp (heute: 0 %).
2. **Sichtbarkeit:** Top-5-Ranking für „Bistro Warnemünde", „Frühstück Warnemünde", „Restaurant Yachthafen Warnemünde" innerhalb von 6 Monaten; Top-10 für „Bootsservice Rostock/Warnemünde".
3. **Performance:** LCP < 1,5 s mobil, CLS < 0,05, Lighthouse Performance ≥ 95.
4. **Internationalisierung:** EN-Version live zum Start (Kreuzfahrt-Saison!).
5. **Selbstpflege:** Team kann Speisekarte, Events und News ohne Entwickler ändern (Supabase-gestütztes Mini-CMS).

## 1.3 Die eine Kernbotschaft

> **„Das Bistro direkt am Yachthafen — wo die Boote festmachen und der Backfisch aus der Pfanne kommt."**

Alles auf der Seite zahlt auf diese eine Positionierung ein: **Wir sind nicht am Hafen. Wir sind AUF der Mole, IM Hafen.** Kein Wettbewerber in Warnemünde kann diesen Satz sagen. Zweite Ebene (für Skipper): der Vierklang **Bistro · Shop · Zubehör · Service** — „ein Anlaufpunkt für alles".

---

# TEIL 2 — MARKEN-DNA & CORPORATE IDENTITY

## 2.1 Logo (bestehend — wird 1:1 übernommen)

- **Bestandteile:** Steuerrad-Emblem (Kreis mit 8 Speichen/Griffen) + Wortmarke „SKIPPER'S" in kräftigen Versalien + Unterzeile „BISTRO · SHOP · BOOTSZUBEHÖR · BOOTSSERVICE".
- **Original-Farbe:** dunkles Marineblau auf Weiß/Creme.
- **Regeln:**
  - Logo NIEMALS neu zeichnen, verzerren, mit Effekten (Schatten, Verlauf, Outline) versehen oder umfärben — Ausnahme: einfarbig Weiß (auf Navy-Flächen) und einfarbig Navy (auf hellen Flächen) sowie Gold-Variante nur fürs Favicon.
  - Mindest-Schutzraum: Höhe des Steuerrads rundum.
  - Im Header: Wort-Bild-Marke horizontal. Im Footer & als Ladeanimation: Steuerrad-Emblem solo erlaubt.
  - Favicon/App-Icon: Steuerrad solo, Gold (#FFAF04) auf Navy (#211C5C).
- **Asset-Beschaffung:** Vor dem Build vom Kunden die Logo-Originaldatei anfordern (SVG/AI/hochauflösendes PNG mit Transparenz). Falls nur JPG existiert: freistellen und als SVG nachbauen lassen (identisch, keine Interpretation!). Quelle des Ist-Zustands: `https://skipper-warnemuende.de/media/einbild/s159_banner.jpg`.

## 2.2 Farbsystem (verbindliche Tokens)

Abgeleitet aus Logo (Navy), bestehender Website-CI (Gold-Akzent aus custom.css: #ffaf04) und der realen Architektur des Gebäudes (Holzfassade am Wasser). **Diese Hexwerte sind verbindlich — keine „ähnlichen" Töne verwenden.**

| Token | Hex | Rolle | Verwendung |
|---|---|---|---|
| `--skipper-navy` | `#211C5C` | Primär (Logo-Blau) | Header, Footer, Headlines, Primär-Buttons, Preise |
| `--skipper-navy-deep` | `#15113F` | Primär dunkel | Verläufe mit Navy, Hero-Hintergründe, Hover-States |
| `--skipper-gold` | `#FFAF04` | Akzent (max. 10 % Flächenanteil!) | CTAs, aktive Zustände, Sterne, Preishighlights, Hover-Unterstreichungen |
| `--skipper-gold-soft` | `#FFE8AA` | Akzent hell | Badge-Hintergründe, dezente Hinweisflächen |
| `--skipper-cream` | `#FAF6EE` | Haupthintergrund | Seitenhintergrund („Segeltuch") — NIE reines #FFF als Seitenhintergrund |
| `--skipper-paper` | `#FFFDF8` | Kartenhintergrund | Cards, Formulare, Menü-Einträge |
| `--skipper-sand` | `#C9B28A` | Sekundär warm | Divider-Details, Bildrahmen, Zitat-Hintergründe (Holzfassade-Referenz) |
| `--skipper-wood` | `#8B6F47` | Sekundär dunkel | Eyebrows im hellen Kontext, Fotounterschriften |
| `--skipper-sea` | `#327893` | Tertiär (Petrol) | Links, Info-Boxen, sekundäre Buttons, Bootsservice-Bereich |
| `--skipper-ink` | `#241F33` | Text | Fließtext auf hellen Flächen |
| `--skipper-ink-soft` | `#5B566E` | Text gedämpft | Nebentexte, Captions |
| `--skipper-line` | `#E6DDCC` | Linien | Borders, Trennlinien, Tabellenlinien |

**Kontrast-Pflicht (WCAG AA):** Navy auf Cream ✓, Ink auf Cream ✓, Gold nur für große Texte/Icons oder auf Navy (Gold-Text auf Weiß ist verboten — zu wenig Kontrast). Weißer Text auf Gold verboten; auf Gold-Buttons immer Navy-Text.

## 2.3 Typografie

| Rolle | Font | Schnitte | Regeln |
|---|---|---|---|
| Display/Headlines | **Archivo** (Google Fonts) | 800, 900 | Versalien nur für Eyebrows/Labels; Headlines in Sentence case; `letter-spacing: -0.01em`; passt zur kompakten, kräftigen Logo-Anmutung |
| Body | **Inter** | 400, 500, 600, 700 | 16–18 px Grundgröße, `line-height: 1.65` |
| Akzent (sparsam!) | **Caveat** (Google Fonts) | 600 | NUR für handschriftliche Mini-Notizen: „frisch vom Kutter", „Kiwis Tipp", Preis-Anmerkungen auf der Karte. Max. 1× pro Viewport. Gibt den Agentur-Charme, wird bei Überdosierung kitschig. |

Typo-Skala: `clamp()`-basiert. H1 `clamp(2.2rem, 5vw, 3.8rem)`, H2 `clamp(1.6rem, 3.5vw, 2.4rem)`, H3 `1.15–1.3rem`, Eyebrow `0.78rem / letter-spacing 0.16em / uppercase / Archivo 800`.

## 2.4 Bildsprache (entscheidend für den „Nicht-KI-Look")

- **Nur echte Fotos vom Standort.** Kein Stock, keine KI-Bilder. Die Location liefert alles: Holzfassade, Masten, Alter Strom, Terrasse, Backfisch, Kaffeetassen mit Hafenblick.
- **Shooting-Liste (an Kunden):** siehe Teil 12.
- **Look:** natürliches Licht, goldene Stunde für Hero-Material; leicht entsättigt-warm (kein HDR, keine Instagram-Filter); Menschen mit echten Momenten (Hände, die Fisch wenden; Gäste im Gespräch) statt gestellter Blicke in die Kamera.
- **Formate:** Hero 3:2 und 4:5 (mobile), Food quadratisch/4:5, Ambiente 3:2. Export als AVIF + WebP-Fallback, max. 200 KB above-the-fold, `width/height`-Attribute Pflicht.
- **Platzhalter-Politik während des Builds:** Farbflächen in `--skipper-sand`/`--skipper-navy` mit Bildbeschriftung („FOTO: Terrasse, goldene Stunde, Blick Alter Strom") — NIEMALS Unsplash-Generika einbauen, die dann „aus Versehen" live gehen.

## 2.5 Tonalität & Sprache (Copy-DNA)

- **Stimme:** echt norddeutsch. Herzlich, direkt, trocken-humorvoll, null Marketing-Sprech. Der bestehende Ton („Landratten", „kurzer Schnack", „echt norddeutsch") ist Gold — beibehalten und ausbauen.
- **Do:** „Moin" als Begrüßung ist erlaubt und erwünscht. Kurze Sätze. Konkrete Sinnlichkeit („knuspriger Backfisch, dampfend aus der Pfanne"). Selbstbewusst ohne Angeberei.
- **Don't:** „Herzlich willkommen auf unserer Homepage", „kulinarische Genussmomente", „Wohlfühlatmosphäre", Superlativ-Stapel, Ausrufezeichen-Inflation, generisches „Qualität und Frische".
- **Ansprache:** „Sie" für Fließtext (Zielgruppe 45+ stark vertreten), aber unkompliziert. Buttons dürfen knapp sein („Tisch reservieren", „Karte ansehen", „Anliegen schildern").
- **EN-Version:** kein 1:1-Übersetzen. Gleicher Charakter, idiomatisch: “Moin! That's how we say hello up north.”

---

# TEIL 3 — ANTI-KI-DESIGN-MANIFEST
### (Die Regeln, damit die Seite nach Design-Agentur aussieht — nicht nach Template oder KI)

KI-generierte Websites erkennt man an Mustern. Diese Muster sind hier **explizit verboten**, mit definierter Alternative:

| # | Verboten (KI-/Template-Tell) | Stattdessen (Skipper's-Weg) |
|---|---|---|
| 1 | Zentrierter Hero: Headline mittig + Subline mittig + 2 Buttons mittig | **Asymmetrischer Split-Hero:** Text links auf Navy-Fläche (55 %), rechts randabfallendes Foto der Terrasse/Mole (45 %), das unter dem Text „hervorschaut". Auf Mobile: Foto oben angeschnitten, Text darüber gelegt. |
| 2 | Lila/Blau-Verläufe, Glassmorphism, Neon-Glows | Flächige Markenfarben, Papier-/Segeltuch-Textur nur als 2 %-Noise auf Cream, harte klare Kanten mit `border-radius: 14px` max. |
| 3 | Emoji als Feature-Icons (🚀✨💡) | **Eigenes Mini-Icon-Set** (Stroke 2px, Navy): Steuerrad, Fisch, Kaffeetasse, Tau/Knoten, Schraubenschlüssel, Anker, Positionslicht. 7 Icons reichen. Als Inline-SVG. |
| 4 | Drei identische Feature-Cards nebeneinander mit Icon-Titel-Text | Die 4 Säulen (Bistro/Shop/Zubehör/Service) als **asymmetrisches Bento-Raster**: Bistro groß mit Foto (2×2), die anderen drei kompakt — die Gewichtung entspricht der realen Umsatzbedeutung. |
| 5 | `Inter` für alles / `Space Grotesk`+Gradient-Kombi | Archivo Black für Display (Verwandtschaft zur Logo-Typo), Inter nur Body, Caveat-Handschrift als seltenes Gewürz (Regel 2.3). |
| 6 | Generische Section-Reihenfolge Hero→Features→Testimonials→CTA→Footer ohne Bruch | Mind. **zwei Full-Bleed-Brüche**: (a) das „Logbuch"-Band (Bewertungen als Zitate im Stil handgeschriebener Logbucheinträge auf Navy), (b) der Lage-Abschnitt mit voller Karte + „Position: 54°10,8'N 12°05,3'O" als nautisches Detail. |
| 7 | Fake-Zahlen-Countern („500+ happy customers") | Nur echte Zahlen: **4,9 ★ · 77 Google-Bewertungen · täglich 8–22 Uhr · Eröffnet 2025.** Verlinkt aufs echte Google-Profil. |
| 8 | Überall dieselben Abstände (`py-16` auf allem) | Rhythmus: große Luft um den Hero (120 px), enger im Karten-Bereich (64 px), Full-Bleed-Bänder ohne Außenabstand. Weißraum ist Gestaltungsmittel, nicht Standardwert. |
| 9 | Scroll-Animationen auf allem (fade-in-up überall) | **Eine** orchestrierte Ladeanimation im Hero (Steuerrad dreht 90° ein, Headline-Zeilen staggern 80 ms). Sonst nur Hover-Mikrointeraktionen (Karten heben 3 px, Links bekommen Gold-Unterstreichung, die von links „einläuft"). `prefers-reduced-motion` respektieren. |
| 10 | Lorem ipsum / austauschbare Headlines („Willkommen bei uns") | Jede Headline besteht den Test: *Könnte sie auch auf der Website eines anderen Restaurants stehen? Dann neu schreiben.* Copy-Deck in Teil 6 ist verbindlich. |
| 11 | Footer als Linkfriedhof in 4 gleichen Spalten | Footer erzählt: Mini-Karte der Molenspitze, Öffnungszeiten als „Törnplan", Wetter-Hinweis-Zeile („Terrasse geöffnet, wenn's der Wind gut meint"), dann erst Rechtliches. |
| 12 | Alles perfekt symmetrisch/steril | **Signature-Element:** eine durchgehende, dezente Wellenlinie (SVG, 1px, `--skipper-sand`) als Section-Divider — wie eine Wasserlinie, die sich durch die Seite zieht. Plus: Fotos dürfen 2–3° gedreht mit „Tape"-Ecke im Logbuch-Band erscheinen. |

**Abnahme-Test vor Launch:** Zeige die Startseite 3 Personen für 5 Sekunden. Frage: „Sieht das nach Baukasten aus?" Bei einem „Ja" → Signature-Elemente schärfen, nicht Effekte hinzufügen.

---

# TEIL 4 — INFORMATIONSARCHITEKTUR

## 4.1 Sitemap & URL-Struktur

```
/                          Startseite (DE)
/speisekarte               Speise- & Getränkekarte (dynamisch aus Supabase)
/fruehstueck               SEO-Landingpage „Frühstück in Warnemünde"
/terrasse                  SEO-Landingpage „Essen mit Hafenblick / Terrasse"
/events                    Veranstaltungen (Liste + Detail /events/[slug])
/feiern                    Gruppen & private Feiern (Anfrage)
/bootsservice              Leistungen + Anfrage-Formular
/bootszubehoer             Sortiment im Laden (Kategorien-Übersicht, kein Checkout in Phase 1)
/shop                      Brücke: erklärt Online-Shop, verlinkt extern ODER „Phase 2"-Teaser
/ueber-uns                 Team, Geschichte, das Gebäude
/news                      Aktuelles (Liste + /news/[slug])
/kontakt                   Kontakt, Anfahrt, Karte, Parken, ÖPNV
/impressum  /datenschutz
/en/...                    Englische Spiegelstruktur (mind.: /, /menu, /breakfast, /contact, /boat-service)
```

**URL-Regeln:** klein, Bindestriche, keine Umlaute (`/fruehstueck`), keine Dateiendungen, keine Parameter. Trailing-Slash-frei, konsistent.

## 4.2 Navigation

- **Header (sticky, Navy):** Logo links · Speisekarte · Events · Feiern · Bootsservice · Über uns · Kontakt · [DE/EN-Switch] · **CTA-Button Gold: „Tisch reservieren"** (immer sichtbar, auch mobil als Sticky-Bottom-Bar mit Telefon + Reservieren).
- Zubehör & Shop laufen unter einem Dropdown/„Für Skipper" zusammen, um die Hauptnav gastro-fokussiert zu halten (Gäste : Skipper ≈ 80 : 20).
- **Mobile:** Fullscreen-Overlay-Menü auf Navy, große Touch-Ziele (min. 48 px), Öffnungszeiten-Status live im Menü („● Jetzt geöffnet — bis 22 Uhr").

## 4.3 Redirect-Map (KRITISCH — vor Launch einrichten, 301!)

Die alten .php-URLs sind bei Google indexiert. Ohne Redirects verliert die Domain ihr gesamtes (kleines, aber vorhandenes) Ranking-Kapital:

```
/index.php                                  → /
/pages/news.php                             → /news
/pages/events.php                           → /events
/pages/speisekarte.php                      → /speisekarte
/pages/getraenkekarte.php                   → /speisekarte#getraenke
/pages/kontakt.php                          → /kontakt
/pages/lage.php                             → /kontakt#anfahrt
/pages/fischrestaurant-hummerkob.php        → /   (falls Partnerseite: externer Link im Footer)
/pages/impressum.php                        → /impressum
/pages/datenschutz.php                      → /datenschutz
/pages/posts/*                              → /news
```

---

# TEIL 5 — SEITEN-BLUEPRINTS

## 5.1 STARTSEITE (Section-by-Section, verbindlich)

### S1 · Hero (Split, asymmetrisch — Regel 3.1)
- **Links (Navy-Fläche, 55 %):** Eyebrow „MITTELMOLE · WARNEMÜNDE" (Gold) → H1 (siehe Copy-Deck 6.1) → Subline → 2 CTAs: primär Gold „Tisch reservieren", sekundär Ghost „Speisekarte ansehen" → darunter Live-Status-Zeile: „● Heute geöffnet · 8–22 Uhr · Küche bis 21 Uhr" (grüner Punkt wenn offen, berechnet client-seitig).
- **Rechts (45 %):** Foto Terrasse/Molenblick, randabfallend, leichte Navy-Vignette am Übergang.
- **Trust-Anker unten im Hero:** „★ 4,9 bei Google · 77 Bewertungen" → verlinkt auf Google-Profil (echte Zahl, per Hand quartalsweise aktualisieren oder Places-API Phase 2).
- Ladeanimation: Steuerrad-Icon dreht 90° ein, H1-Zeilen staggern (Regel 3.9).

### S2 · Bewertungs-Band „Aus dem Logbuch unserer Gäste" (Full-Bleed, Navy)
- 3 echte Google-Zitate (gekürzt, sinngemäß, mit Vorname + Monat), als leicht gedrehte „Logbuch-Karten" (Cream) mit Caveat-Handschrift-Überschrift. Beispiele der echten Reviews: warme Begrüßung, „riesige Portion Dorschfilet", „kommen definitiv wieder".
- Rechts: großes „4,9" in Archivo Black Gold + „von 5 · Google" + Button „Alle 77 Bewertungen lesen ↗".

### S3 · Die 4 Säulen als Bento (Regel 3.4)
- Grid: **Bistro** (2×2, mit Foto, Text + Link „Zur Karte") · **Bootszubehör** (1×1, Icon Tau) · **Bootsservice** (1×1, Icon Schraubenschlüssel) · **Shop** (1×2 flach, „bald online" ODER Link, je nach Status — nie ein totes Versprechen!).
- Jede Kachel: Eyebrow, 1 Satz Nutzen, Ziel-Link. Hover: 3 px Lift + Gold-Linie läuft unter dem Linktext ein.

### S4 · Speisekarten-Teaser „Was heute aus der Pfanne kommt"
- 3 Signature-Gerichte als Foto-Cards (Backfisch, Frühstücksteller, Scampi) mit echten Preisen aus Supabase. Handschrift-Note an einem Gericht: „Kiwis Tipp" (Regel 2.3).
- CTA: „Die ganze Karte ansehen".

### S5 · Lage als Erlebnis (Full-Bleed — Regel 3.6)
- Volle Breite: eingebettete Karte (statisches Map-Bild mit Marker, klickbar → Google Maps; KEIN iframe-Cookie-Problem vor Consent).
- Overlay-Card: „So finden Sie uns" — 3 Zeilen: 🚶 5 Min. vom Bahnhof · 🅿 Großparkplatz 2 Min. · ⛵ Gastlieger: direkt am Steg.
- Nautisches Detail: „Position 54°10,8′ N · 12°05,3′ O" klein in Sand — das Detail, das Segler lieben und Agentur-Liebe zeigt.

### S6 · Über-uns-Teaser
- 2-Spalter: Foto Team/Gebäude + 3 Sätze Geschichte („Seit August 2025…") + Link „Mehr über uns".

### S7 · Events/News-Teaser (nur wenn Einträge existieren — sonst Section automatisch ausblenden!)
- Max. 2 kommende Events als schmale Zeilen mit Datum-Badge (Navy-Kachel mit Tag/Monat).

### S8 · Abschluss-CTA (Navy)
- „Der Tisch mit dem besten Blick auf den Alten Strom wartet." + Reservieren-Button + Telefonnummer groß + WhatsApp-Button.

### S9 · Footer (Regel 3.11)
- 3 Zonen: (1) „Törnplan" = Öffnungszeiten-Tabelle + Live-Status, (2) Kontakt + Mini-Lageskizze (SVG der Molenspitze!), (3) Rechtliches + Sprach-Switch + dezente Social-Icons. Wellenlinie als oberer Abschluss.

## 5.2 SPEISEKARTE `/speisekarte`
- Sticky Kategorie-Tabs (Frühstück · Snacks · Fisch · Fleisch & mehr · Kinder · Desserts · Getränke), scrollspy-aktiv.
- Eintrag: Name (Archivo 700) · Beschreibung 1 Zeile · Preis rechts (tabellarische Ziffern) · Badges (V = vegetarisch, 🌶 etc.) · Allergene als dezente Fußnoten-Ziffern → aufklappbare Legende (Pflicht: LMIV!).
- Daten aus Supabase (`menu_items`), Reihenfolge per `sort_order`. „Saison-Karte"-Flag für Wochenangebote mit Gold-Badge „Diese Woche".
- Druck-Stylesheet (`@media print`) — Gäste drucken Karten wirklich aus.
- Kein PDF als Primärformat! PDF optional als Download-Link generiert aus denselben Daten.

## 5.3 FRÜHSTÜCK `/fruehstueck` (SEO-Landingpage)
- H1 „Frühstück in Warnemünde — direkt am Yachthafen". Hero-Foto Frühstücksteller mit Hafen im Bokeh.
- Inhalte: Frühstückszeiten (bis 11 Uhr), 2–3 Teller im Detail mit Preisen, Kaffee-Angebot, Terrassen-Hinweis, FAQ-Block (5 Fragen: reservieren? Hunde? vegan? Parken? bis wann?) → FAQPage-Schema.
- Interne Links: → Speisekarte, → Reservierung, → Terrasse.

## 5.4 BOOTSSERVICE `/bootsservice`
- Leistungsliste (Wartung, kleine Reparaturen, Motor-/Segelcheck, Saisonvorbereitung) als klare Karten mit „ab"-Preisen sofern kommunizierbar, sonst „auf Anfrage".
- **Anfrage-Formular:** Name, Telefon/E-Mail, Bootstyp (Segel/Motor/Sonstiges), Liegeplatz (frei), Anliegen (Textarea), Wunschzeitraum, DSGVO-Checkbox. → Supabase `service_requests` + E-Mail-Notification (Edge Function).
- Vertrauens-Block: Foto Werkstatt/Steg, „Persönliche Beratung inklusive".

## 5.5 FEIERN `/feiern`
- Zielgruppe Persona 09. Kapazität, Anlässe (Geburtstag, Firmenevent, Familienfeier), Ablauf („Anfragen → Wir melden uns binnen 24 h → gemeinsames Vorgespräch").
- Kurzformular: Datum, Personenzahl, Anlass, Kontakt. → `event_requests`.

## 5.6 KONTAKT `/kontakt`
- Alles auf einer Seite: Karte, Adresse, Telefon (click-to-call), WhatsApp-Deeplink (`https://wa.me/491724007202?text=Moin!%20Ich%20h%C3%A4tte%20gern%20einen%20Tisch...`), E-Mail (echter mailto, KEINE JS-Verschleierung), Öffnungszeiten-Tabelle, Anfahrt in 3 Tabs (Auto+Parken / Bahn+S-Bahn / übers Wasser für Gastlieger!). Der dritte Tab ist das Detail, das keiner der Wettbewerber hat.

## 5.7 ÜBER UNS, NEWS, EVENTS
- Über uns: Geschichte (Eröffnung 2025, die Idee „alles für Skipper unter einem Dach"), Team-Fotos mit Vornamen, das Gebäude (Architektur-Absatz — der Holzbau ist erzählenswert).
- News/Events: Liste (Karte: Bild, Datum, Titel, Teaser) + Detailseiten. Events mit Event-Schema (Datum, Ort) → Google-Event-Snippets.

---

# TEIL 6 — COPY-DECK (fertige Texte, DE)

> Verbindlich. Anpassungen nur im selben Ton. Jeder Text hat den „Könnte-das-woanders-stehen?"-Test bestanden (Regel 3.10).

## 6.1 Hero-Headline — 3 geprüfte Varianten (A/B-fähig)

**🏆 VARIANTE A — Autorität/Vertrauen (EMPFOHLEN als Launch-Version):**
> **H1:** Moin auf der Mittelmole.
> **Sub:** Frischer Fisch, ehrliches Frühstück und der beste Blick auf den Alten Strom — im Bistro direkt am Yachthafen. Von Gästen mit 4,9 Sternen bewertet.
> *Trigger: Social Proof + Ortsidentität. Das „Moin" ist Marke.*

**🔥 VARIANTE B — FOMO/Sinnlichkeit:**
> **H1:** Wenn der Backfisch zischt, ist der beste Platz am Wasser schnell weg.
> **Sub:** Terrasse an der Molenspitze, Küche bis 21 Uhr — sichern Sie sich Ihren Tisch mit Hafenblick.
> *Trigger: Verknappung (Terrassenplätze real begrenzt) + Appetit-Auslöser.*

**🎯 VARIANTE C — Neugier/Positionierung:**
> **H1:** Das einzige Bistro, an dem Sie mit dem Boot vorfahren können.
> **Sub:** Mittelmole Warnemünde: Essen, Bootszubehör und Service — dort, wo andere nur gucken.
> *Trigger: Information Gap + Alleinstellung. Stark für Kampagnen/Social.*

## 6.2 Weitere Kern-Texte

- **Bewertungs-Band Headline:** „Aus dem Logbuch unserer Gäste"
- **4-Säulen Intro:** „Ein Haus, vier Gründe festzumachen." — Bistro: „Frühstück ab 8, Backfisch bis in den Abend — und dazwischen der beste Kaffee der Mole." · Zubehör: „Leine gerissen, Schäkel weg? Die kleinen Retter für den Bordalltag — sofort zum Mitnehmen." · Service: „Wartung, kleine Reparaturen, Saisonstart: Wir bringen Ihr Boot in Schuss, während Sie Fisch essen." · Shop: je nach Status.
- **Speisekarten-Teaser:** „Was heute aus der Pfanne kommt" + Note (Caveat): „frisch vom Kutter nebenan"
- **Lage:** „Am Ende der Mole. Am Anfang vom Urlaub." — „5 Minuten vom Bahnhof, 2 vom Großparkplatz — oder Sie machen einfach direkt am Steg fest."
- **Abschluss-CTA:** „Der Tisch mit dem besten Blick auf den Alten Strom wartet."
- **404-Seite:** „Männüber Bord? Diese Seite ist nicht mehr an Deck." + Button „Zurück zur Startseite" — auch Fehlerseiten tragen die Marke.
- **Footer-Wetterzeile:** „Terrassenbetrieb, wenn's der Wind gut meint — im Zweifel kurz durchklingeln."
- **Reservierungs-Bestätigung (Formular-Success):** „Anfrage ist an Bord! Wir melden uns schnellstmöglich — meist innerhalb weniger Stunden."

## 6.3 Meta-Titles & Descriptions (pro Seite, verbindlich)

| Seite | Title (≤ 60 Z.) | Description (≤ 155 Z.) |
|---|---|---|
| / | Skipper's Bistro Warnemünde — direkt am Yachthafen, Mittelmole | Frühstück, frischer Fisch & Kaffee mit Blick auf den Alten Strom. Täglich 8–22 Uhr. ★ 4,9 bei Google. Jetzt Tisch reservieren. |
| /speisekarte | Speisekarte — Skipper's Bistro Warnemünde | Backfisch frisch aus der Pfanne, Frühstück bis 11 Uhr, Kaffee & Kuchen: die aktuelle Karte vom Bistro am Yachthafen Warnemünde. |
| /fruehstueck | Frühstück in Warnemünde am Hafen — Skipper's Bistro | Frühstücken mit Blick auf Boote und Alten Strom: täglich ab 8 Uhr auf der Mittelmole. Terrasse, fairer Preis, 4,9 Sterne. |
| /bootsservice | Bootsservice Warnemünde / Rostock — Wartung & Reparatur | Motor- & Segelcheck, Wartung, kleine Reparaturen direkt am Yachthafen Warnemünde. Jetzt unverbindlich anfragen. |
| /feiern | Feiern am Hafen — Skipper's Bistro Warnemünde | Geburtstag, Firmenfeier oder Familienessen mit Hafenblick auf der Mittelmole. Jetzt Termin & Angebot anfragen. |
| /kontakt | Kontakt & Anfahrt — Skipper's Bistro Warnemünde | Adresse, Öffnungszeiten, Parken & S-Bahn: So finden Sie uns auf der Mittelmole — 5 Minuten vom Bahnhof Warnemünde. |


---

# TEIL 7 — FUNKTIONS-SPEZIFIKATION

| # | Funktion | Phase | Detail |
|---|---|---|---|
| F1 | **Online-Reservierung** | 1 | Zwei Stufen: (a) Launch: eigenes Anfrage-Formular (Datum, Zeit-Slots, Personen, Name, Telefon, Anmerkung) → Supabase `reservations` + E-Mail an Betrieb; Antwort binnen definierter Zeit wird kommuniziert. (b) Empfehlung Phase 1b: externes Buchungstool mit Echtzeit-Bestätigung (resmio / OpenTable / Quandoo — Betreiber entscheidet nach Gebührenmodell) als Embed; Formular bleibt Fallback. |
| F2 | **Öffnungszeiten-System** | 1 | Zentrale Konfiguration in Supabase `settings` (JSON: Regelzeiten + Ausnahmen/Feiertage/„PAUSE"-Betriebsferien). Überall aus EINER Quelle: Header-Status, Footer-Törnplan, Kontakt, Schema.org. Nie wieder hart codierte Zeiten. |
| F3 | **Speisekarten-CMS** | 1 | Tabellen `menu_categories`, `menu_items` (name, description, price, badges[], allergens[], is_available, is_seasonal, sort_order). Admin-Bereich (Login) mit einfacher Tabelle: Preis ändern = Inline-Edit. Verfügbar-Toggle für „heute aus". |
| F4 | **Events & News** | 1 | `events` (title, slug, date, time, description, image, is_published), `news` analog. Admin-CRUD. Startseiten-Teaser blendet sich bei 0 Einträgen aus. |
| F5 | **Service-/Feier-Anfragen** | 1 | Formulare → `service_requests`, `event_requests` + Edge-Function-Mail. Spam-Schutz: Honeypot + Zeitfalle (kein sichtbares Captcha!). |
| F6 | **WhatsApp-Kontakt** | 1 | `wa.me`-Deeplink mit vorbefülltem Text, als Button im Kontakt + schwimmend NUR auf Mobilgeräten (dezent, Navy, kein grüner Fremdkörper). |
| F7 | **Zweisprachigkeit DE/EN** | 1 | i18n via Sprachdateien (JSON) + `/en/`-Routen. `hreflang`-Tags. Menü-Daten: EN-Felder optional in denselben Tabellen (`name_en`, `description_en`); Fallback DE. |
| F8 | **Bewertungs-Modul** | 1 | Launch: kuratierte echte Zitate (manuell gepflegt in `testimonials`) + Link zum Google-Profil. Phase 2: Google Places API für Live-Rating-Zahl. |
| F9 | **Galerie** | 1 | Masonry-Grid `gallery_images` (Kategorien: Essen/Terrasse/Hafen/Team), Lightbox, Lazy Loading. |
| F10 | **Admin-Bereich** | 1 | `/admin` hinter Supabase Auth (E-Mail-Login, nur Whitelist). KEINE öffentlichen Admin-Links im Frontend (Learning aus Audit!). Bereiche: Karte, Events, News, Anfragen-Inbox, Öffnungszeiten, Zitate, Galerie. |
| F11 | Online-Shop | 2 | Bewusst Phase 2 (eigenes Projekt: Sortiment, Versand, Payment). Bis dahin: `/shop` erklärt ehrlich den Stand + „Sortiment im Laden"-Übersicht + Anruf-CTA. Kein totes Versprechen wie heute. |
| F12 | Gutscheine | 2 | PDF-Gutschein-Verkauf (starker Weihnachts-Umsatz bei Gastro). |
| F13 | Newsletter | 2 | Double-Opt-in, DSGVO-sauber; Anlass: Saisonstart/Events. |

---

# TEIL 8 — TECHNIK-STACK & DATENMODELL

## 8.1 Stack (Lovable-Standard)

- **React 18 + TypeScript (strict) + Vite + Tailwind CSS + shadcn/ui + Supabase + React Query (TanStack).**
- Formulare: `react-hook-form` + `zod`. Kein Redux, kein CSS-in-JS.
- Routing: React Router; SEO-kritische Inhalte werden zusätzlich über `react-helmet-async` (Title/Meta/JSON-LD) + Prerendering (Lovable-Hosting rendert; zusätzlich statisches `sitemap.xml`/`robots.txt` in `/public`) abgesichert.
- Hosting: Lovable-Deploy; Custom Domain `skipper-warnemuende.de` aufschalten; alte Server-Redirects (Teil 4.3) via `_redirects`/Hosting-Konfiguration ODER — falls DNS beim Alt-Hoster bleibt — dort als 301 einrichten.

## 8.2 Supabase-Schema (mit RLS — Pflicht auf ALLEN Tabellen)

```sql
-- Öffentliche Lese-Tabellen (SELECT: anon erlaubt, Schreiben: nur authentifizierte Admins)
menu_categories (id uuid pk, name text, name_en text, sort_order int, is_active bool default true)
menu_items (id uuid pk, category_id fk, name text, name_en text, description text, description_en text,
            price numeric(6,2), badges text[], allergens text[], is_available bool default true,
            is_seasonal bool default false, sort_order int)
events (id uuid pk, title text, title_en text, slug text unique, event_date date, event_time text,
        description text, description_en text, image_url text, is_published bool default false)
news (id uuid pk, title text, slug text unique, published_at timestamptz, teaser text, body text,
      image_url text, is_published bool default false)
testimonials (id uuid pk, author text, month_label text, quote text, quote_en text, sort_order int, is_active bool)
gallery_images (id uuid pk, url text, alt text, category text, sort_order int)
settings (key text pk, value jsonb)   -- opening_hours, holiday_notice, phone, whatsapp, social

-- Eingangs-Tabellen (INSERT: anon erlaubt mit Validierung, SELECT: nur Admin)
reservations (id uuid pk, created_at, date date, time_slot text, party_size int,
              name text, phone text, note text, status text default 'new', honeypot_check bool)
service_requests (id uuid pk, created_at, name text, contact text, boat_type text,
                  berth text, message text, preferred_period text, status text default 'new')
event_requests (id uuid pk, created_at, name text, contact text, requested_date date,
                party_size int, occasion text, message text, status text default 'new')

-- RLS-Muster:
--  public read:   create policy "public read" on menu_items for select using (is_available is not null);
--  admin write:   using (auth.role() = 'authenticated')  -- Admin-Whitelist via Supabase Auth
--  anon insert:   for insert with check (true)  -- plus Edge-Function-Rate-Limit + Honeypot
```

**Sicherheits-Learnings aus dem Audit umsetzen:** kein Admin-Link im Frontend · keine PII in URLs/localStorage · Formular-Inputs serverseitig validieren (zod-Schema in Edge Function spiegeln) · E-Mails als normale `mailto:` (Spam-Risiko < Usability-Schaden der JS-Verschleierung).

---

# TEIL 9 — SEO-BLUEPRINT

## 9.1 Strukturierte Daten (JSON-LD, auf JEDER Seite im `<head>`)

**Restaurant-Schema (global, Werte aus `settings` speisen):**
```json
{
  "@context": "https://schema.org",
  "@type": "Restaurant",
  "name": "Skipper's Bistro",
  "image": ["https://skipper-warnemuende.de/img/hero-terrasse.jpg"],
  "url": "https://skipper-warnemuende.de",
  "telephone": "+4938126054246",
  "email": "info@skippers-bistro.de",
  "servesCuisine": ["Fisch", "Deutsch", "Frühstück", "Kaffee"],
  "priceRange": "€€",
  "currenciesAccepted": "EUR",
  "address": {
    "@type": "PostalAddress",
    "streetAddress": "Am Bahnhof 2a",
    "addressLocality": "Rostock-Warnemünde",
    "postalCode": "18119",
    "addressCountry": "DE"
  },
  "geo": { "@type": "GeoCoordinates", "latitude": 54.1800322, "longitude": 12.0883336 },
  "openingHoursSpecification": [{
    "@type": "OpeningHoursSpecification",
    "dayOfWeek": ["Monday","Tuesday","Wednesday","Thursday","Friday","Saturday","Sunday"],
    "opens": "08:00", "closes": "22:00"
  }],
  "acceptsReservations": "https://skipper-warnemuende.de/#reservieren",
  "menu": "https://skipper-warnemuende.de/speisekarte",
  "hasMap": "https://maps.google.com/?cid=<GBP-ID>",
  "sameAs": ["<Google-Business-Profil-URL>", "<Instagram sobald vorhanden>"]
}
```
**Zusätzlich:** `BreadcrumbList` auf Unterseiten · `Menu`/`MenuSection`/`MenuItem` auf /speisekarte (aus Supabase generiert!) · `FAQPage` auf /fruehstueck · `Event` auf Event-Detailseiten · `LocalBusiness`-Zweitprofil für den Bootsservice-Bereich mit `@type: "BoatRepair"`-nahem Typ (`AutoRepair` existiert, für Boote: `LocalBusiness` + `additionalType`).
**AggregateRating NUR einbauen, wenn echte Bewertungen auf der Seite selbst angezeigt werden** (Google-Richtlinie) — mit dem Testimonial-Modul erfüllt.

## 9.2 Technische SEO-Dateien

```
/public/robots.txt
  User-agent: *
  Allow: /
  Disallow: /admin
  Sitemap: https://skipper-warnemuende.de/sitemap.xml

/public/sitemap.xml   → alle DE+EN-URLs, lastmod gepflegt (Build-Step generiert)
```
- Canonical self-referencing auf jeder Seite · `hreflang` de/en/x-default · Open Graph + Twitter Cards mit eigenem OG-Bild (1200×630: Terrassenfoto + Logo + „Bistro am Yachthafen Warnemünde") · `lang`-Attribut korrekt je Route.

## 9.3 Keyword-Zielkarte (aus dem Alt-Meta-Keywords-Feld destilliert — die Liste war strategisch richtig, nur falsch eingesetzt)

| Keyword-Cluster | Ziel-URL | Intent |
|---|---|---|
| bistro/restaurant warnemünde, restaurant am hafen/yachthafen warnemünde, essen mit meerblick | / + /terrasse | Besuch |
| frühstück warnemünde/in warnemünde | /fruehstueck | Besuch, morgens, mobil |
| fisch essen warnemünde, backfisch warnemünde | /speisekarte (+ Food-Bilder mit Alt-Texten!) | Besuch |
| kaffee warnemünde, café warnemünde | / (S4) + /speisekarte#kaffee | Besuch |
| feiern rostock hafen, restaurant firmenfeier warnemünde | /feiern | Anfrage |
| bootsservice warnemünde/rostock, bootszubehör warnemünde | /bootsservice, /bootszubehoer | Anfrage/Kauf |
| restaurant near cruise port warnemünde, breakfast warnemünde | /en/, /en/breakfast | Besuch (EN!) |

## 9.4 Lokales SEO
- Google Business Profile: Website-Link aktualisieren, Attribute pflegen (Terrasse, Hunde erlaubt?, Reservierung-Link), Speisekarten-Link setzen, wöchentlich 1 Foto-Post in Saison.
- NAP-Konsistenz (Name/Adresse/Telefon) zwischen Website, GBP, Impressum — inkl. Klärung der E-Mail-Domain-Inkonsistenz.
- Einträge: TripAdvisor claimen, marinas.info/Hafenführer MV, Ostsee-Tourismusportale, „Warnemünde erleben".

---

# TEIL 10 — PERFORMANCE-BUDGET (hart)

| Metrik | Budget |
|---|---|
| LCP (mobil, 4G) | < 1,5 s |
| CLS | < 0,05 (alle Bilder mit width/height, Fonts mit `font-display: swap` + Fallback-Metriken) |
| JS initial | < 180 KB gzip |
| Bild above-the-fold | < 200 KB (AVIF/WebP, `fetchpriority="high"` fürs Hero-Bild) |
| Fonts | max. 3 Familien / 6 Dateien, WOFF2, preload für Archivo 900 |
| Lighthouse | ≥ 95 / ≥ 95 / ≥ 95 / 100 (Perf/A11y/BP/SEO) |
| Externe Skripte | 0 vor Consent (Karte als statisches Bild, Buchungstool lazy nach Interaktion) |


---

# TEIL 11 — LOVABLE MASTER-PROMPT (copy-paste-fertig)

> **Modus:** New Project (Neubau von Null). Diesen Prompt komplett in Lovable einfügen. Danach Feature-Erweiterungen nur noch als schlanke Folge-Prompts (Muster am Ende von Teil 11). Vor dem Einfügen prüfen: Logo-SVG hochgeladen? Fotos vorhanden oder Platzhalter-Politik akzeptiert?

```
## 1. Context & Goal
Skipper's Bistro Warnemünde – marketing & reservation website for a harbour bistro with an attached boat-supply shop and boat service, located directly ON the Mittelmole pier at Warnemünde yacht harbour (Baltic Sea, Germany).
Industry: Gastronomy / Marine services | Target Audience: tourists, breakfast seekers, families, sailors/boat owners, cruise passengers (EN!) | Stage: Production
Primary Goal: Within 5 seconds a visitor must understand: "Bistro with the best harbour view in Warnemünde, open daily 8–22, rated 4.9 stars – reserve a table now." Secondary goal: boat owners can request service/see supplies.
Language: German is the primary language of ALL UI copy. English version via i18n (routes under /en). Never output Lorem ipsum – use the exact German copy provided below.

## 2. Visual Identity
Color Palette (STRICT – use these exact tokens as CSS variables / Tailwind config, no substitutes):
Primary #211C5C (skipper-navy) | Primary-dark #15113F | Accent #FFAF04 (skipper-gold, max ~10% of any viewport, CTAs only) | Accent-soft #FFE8AA | Background #FAF6EE (cream – never pure white page background) | Card #FFFDF8 | Warm secondary #C9B28A (sand) / #8B6F47 (wood) | Tertiary #327893 (sea/petrol, links & info) | Text #241F33 / muted #5B566E | Lines #E6DDCC
Typography: Headings – "Archivo" (weights 800/900, sentence case, letter-spacing -0.01em) | Body – "Inter" (400/500/600/700, 16–18px, line-height 1.65) | Handwritten accent – "Caveat" 600, used at MOST once per viewport for small notes like "frisch vom Kutter".
Design Language: "Modern maritime editorial" – flat brand colors, warm cream paper feel, clear 14px-max border radius, generous whitespace rhythm (hero airy, cards denser). Explicitly FORBIDDEN: purple/blue gradients, glassmorphism, emoji as icons, centered-everything hero, fake counters, stock photos.
UI Style: Asymmetric Split-Hero (navy text panel 55% left, full-bleed photo right); Bento grid for the 4 pillars (Bistro large 2x2 with photo, Zubehör 1x1, Service 1x1, Shop 1x2); two full-bleed navy bands ("Logbuch" testimonials with slightly rotated cream cards; location band with coordinates detail "54°10,8′ N · 12°05,3′ O").
Signature element: a thin 1px SVG wave line in #C9B28A used as the section divider throughout ("waterline"), plus a custom inline-SVG icon set (2px stroke, navy): ship wheel, fish, coffee cup, rope knot, wrench, anchor, position light. Build these SVGs – do not use emoji or generic icon fonts.
Animations: ONE orchestrated hero load (ship-wheel icon rotates in 90°, H1 lines stagger 80ms), hover micro-interactions only elsewhere (cards lift 3px, links get a gold underline sliding in from left). Respect prefers-reduced-motion.
Logo: an uploaded SVG/PNG asset (navy ship-wheel emblem + "SKIPPER'S" wordmark). Never redraw, recolor (except all-white on navy), distort or add effects. Favicon: wheel emblem solo, gold on navy.

## 3. Feature Specification
Build the following features. Implement ALL of them completely – no placeholders:

### Layout & Navigation
- Sticky navy header: logo left; nav: Speisekarte, Events, Feiern, Bootsservice, Über uns, Kontakt; dropdown "Für Skipper" (Bootszubehör, Shop); DE/EN switch; gold CTA button "Tisch reservieren" always visible.
- Live open/closed status chip computed client-side from opening hours in Supabase settings ("● Jetzt geöffnet – bis 22 Uhr").
- Mobile: fullscreen navy overlay menu, 48px touch targets; sticky bottom bar with [📞 Anrufen] [Tisch reservieren].
- Footer in 3 zones: "Törnplan" (opening-hours table + live status), contact block with a small custom SVG sketch of the pier tip + weather line "Terrassenbetrieb, wenn's der Wind gut meint – im Zweifel kurz durchklingeln.", legal links + language switch. Wave divider on top.

### Homepage (exact section order)
1. Split-Hero: Eyebrow "MITTELMOLE · WARNEMÜNDE" (gold) / H1 "Moin auf der Mittelmole." / Sub "Frischer Fisch, ehrliches Frühstück und der beste Blick auf den Alten Strom — im Bistro direkt am Yachthafen. Von Gästen mit 4,9 Sternen bewertet." / CTAs: gold "Tisch reservieren" + ghost "Speisekarte ansehen" / live status line / trust anchor "★ 4,9 bei Google · 77 Bewertungen" linking to the Google profile.
2. Full-bleed navy testimonial band "Aus dem Logbuch unserer Gäste": 3 quote cards (cream, rotated -2°/1°/-1°, Caveat headline) fed from Supabase testimonials + big gold "4,9 / 5 · Google" + link "Alle Bewertungen lesen ↗".
3. Bento grid "Ein Haus, vier Gründe festzumachen." with the 4 pillars and the German copy provided (Bistro/Zubehör/Service/Shop).
4. Menu teaser "Was heute aus der Pfanne kommt": 3 featured menu_items with real prices from Supabase; one Caveat note "Kiwis Tipp".
5. Full-bleed location band "Am Ende der Mole. Am Anfang vom Urlaub.": static map image with marker (click opens Google Maps – no iframe before consent), overlay card with 3 arrival facts (5 Min. Bahnhof / Parkplatz 2 Min. / Gastlieger: direkt am Steg), small sand-colored coordinates line.
6. About teaser (photo + 3 sentences + link).
7. Events/News teaser – renders ONLY if published entries exist, otherwise the section is hidden entirely.
8. Closing CTA band (navy): "Der Tisch mit dem besten Blick auf den Alten Strom wartet." + reserve button + large phone number + WhatsApp button.

### Pages
- /speisekarte: sticky category tabs with scrollspy (Frühstück, Snacks, Fisch, Fleisch & mehr, Kinder, Desserts, Getränke); items from Supabase with name, one-line description, right-aligned tabular price, badges (V, vegan, 🌶 as styled text-badges not emoji-icons), allergen footnote numbers with collapsible legend; "Diese Woche" gold badge for is_seasonal; print stylesheet.
- /fruehstueck: SEO landing page, H1 "Frühstück in Warnemünde — direkt am Yachthafen", breakfast hours (until 11:00), 2–3 dishes w/ prices, FAQ accordion (5 questions) with FAQPage JSON-LD.
- /events + /events/[slug], /news + /news/[slug]: card lists + detail pages, Event JSON-LD on details.
- /feiern: capacity & occasions info + request form (date, party size, occasion, contact, message).
- /bootsservice: service cards (Wartung, kleine Reparaturen, Motor-/Segelcheck, Saisonvorbereitung) + request form (name, contact, boat type select, berth, message, preferred period, GDPR checkbox).
- /bootszubehoer: category overview of in-store assortment (Leinen, Karabiner, Fender, Pflege, Elektrik-Kleinteile, Sicherheit) – informational, with "sofort zum Mitnehmen" positioning and call CTA. No checkout.
- /shop: honest status page – headline "Der Skipper's Online-Shop legt bald ab.", assortment teaser, call CTA. No dead promises.
- /ueber-uns, /kontakt (map, click-to-call, real mailto, WhatsApp deeplink https://wa.me/491724007202 with prefilled German text, opening hours table, arrival info in 3 tabs: Auto+Parken / Bahn / Übers Wasser für Gastlieger), /impressum, /datenschutz (placeholder legal text clearly marked for the owner's lawyer to replace).
- Custom 404: "Männüber Bord? Diese Seite ist nicht mehr an Deck." + home button.
- English versions under /en for: home, /en/menu, /en/breakfast, /en/boat-service, /en/contact – idiomatic English, same design, hreflang tags.

### Reservation (Phase 1 form)
- Modal + dedicated section: date picker (today+90d), time-slot select (11:30–20:30, 30-min steps), party size 1–12 (larger → hint to call), name, phone, note; zod validation; honeypot field + min-3s time trap instead of captcha.
- Insert into Supabase reservations; success state: "Anfrage ist an Bord! Wir melden uns schnellstmöglich — meist innerhalb weniger Stunden." plus clear notice that the reservation is confirmed only after callback/e-mail.

### Admin (/admin, Supabase Auth, email whitelist)
- Sections: Speisekarte (inline price edit, availability toggle, sort), Events, News, Anfragen-Inbox (reservations/service/event requests with status new→done), Öffnungszeiten editor (regular hours + exception dates), Zitate (testimonials), Galerie upload.
- NO admin links anywhere in the public frontend.

## 4. Technical Constraints
Stack: React 18 + TypeScript (strict mode) + Tailwind CSS + shadcn/ui + Supabase
- Use React Query (TanStack Query) for all data fetching – no raw fetch() calls in components
- All forms: react-hook-form + zod validation
- Component architecture: keep components under 150 lines; extract logic into custom hooks (useOpeningStatus, useMenu, useReservation)
- File naming: PascalCase components, camelCase hooks (useXxx.ts), kebab-case utilities
- i18n: lightweight JSON dictionaries (de.json/en.json) + route-based locale; menu tables carry optional name_en/description_en with German fallback
- SEO: react-helmet-async per route (title/meta/canonical/OG/hreflang) + JSON-LD components (Restaurant global with real data: address Am Bahnhof 2a, 18119 Rostock-Warnemünde; geo 54.1800322/12.0883336; phone +4938126054246; opens 08:00 closes 22:00 all days; menu URL; acceptsReservations). Static /public/robots.txt (Disallow: /admin, Sitemap ref) and /public/sitemap.xml with all DE+EN routes.
- Performance: hero image via <img fetchpriority="high"> with explicit width/height; all below-fold images loading="lazy"; fonts WOFF2 with font-display: swap, preload Archivo 900; no external scripts before user interaction; target LCP < 1.5s mobile.
- Accessibility: WCAG AA contrast (never gold text on white; navy text on gold buttons), visible focus states, keyboard-navigable menu & accordion, alt text on every image, aria-expanded on collapsibles.
- Supabase schema required: exactly the tables/fields listed below.

Supabase Schema:
Table: menu_categories (id uuid pk, name text, name_en text, sort_order int, is_active bool default true)
Table: menu_items (id uuid pk, category_id uuid fk, name text, name_en text, description text, description_en text, price numeric, badges text[], allergens text[], is_available bool default true, is_seasonal bool default false, sort_order int)
Table: events (id uuid pk, title text, title_en text, slug text unique, event_date date, event_time text, description text, description_en text, image_url text, is_published bool default false)
Table: news (id uuid pk, title text, slug text unique, published_at timestamptz, teaser text, body text, image_url text, is_published bool default false)
Table: testimonials (id uuid pk, author text, month_label text, quote text, quote_en text, sort_order int, is_active bool default true)
Table: gallery_images (id uuid pk, url text, alt text, category text, sort_order int)
Table: settings (key text pk, value jsonb)
Table: reservations (id uuid pk, created_at timestamptz default now(), date date, time_slot text, party_size int, name text, phone text, note text, status text default 'new')
Table: service_requests (id uuid pk, created_at timestamptz default now(), name text, contact text, boat_type text, berth text, message text, preferred_period text, status text default 'new')
Table: event_requests (id uuid pk, created_at timestamptz default now(), name text, contact text, requested_date date, party_size int, occasion text, message text, status text default 'new')
Seed data: create the 7 menu categories, 12 realistic menu items in Skipper's tone (e.g. "Skipper's Frühstücksteller" 12,90 / "Backfisch, frisch aus der Pfanne" 16,50 / "Scampi in Knoblauch-Olivenöl" 18,90 / Currywurst / Kaffee-Positionen), 3 testimonials (paraphrased from real Google reviews: warm welcome & dog bowl; huge cod portion with asparagus; "great food, fair price, we'll be back"), opening hours setting (Mon–Sun 08:00–22:00, kitchen until 21:00).

## 5. GDPR & Security Layer
- Enable Row Level Security (RLS) on ALL Supabase tables – no exceptions
- Public tables (menu, events, news, testimonials, gallery, settings): SELECT for anon; INSERT/UPDATE/DELETE only for authenticated
- Inbox tables (reservations, service_requests, event_requests): INSERT for anon (with zod-mirrored edge validation), SELECT/UPDATE only for authenticated
- Auth: Supabase Auth email/password, admin whitelist; /admin route-guarded
- No PII in localStorage or URL params; no third-party embeds before consent (map = static image link); GDPR checkbox on all forms with link to /datenschutz
- API keys: anon key client-side only; mail notifications via Edge Function (secrets server-side)

## 6. Credit Optimization
- Reuse existing shadcn/ui components (Button, Card, Dialog, Form, Tabs, Accordion, Table, Badge, Sheet) – do not recreate
- Extract shared logic into hooks; one shared <SectionShell> and <WaveDivider> component instead of repeated section markup
- Build the icon set as ONE SkipperIcon component with a name prop
- Composition over duplication; one component = one responsibility

---

## STATUS REPORT (Mandatory – output this AFTER implementation)

| Check | Status | Notes |
|---|---|---|
| All features implemented | ✅ / ⚠️ / ❌ | |
| All pages present incl. /en routes | ✅ / ❌ | |
| RLS enabled on all tables | ✅ / ❌ | |
| Seed data inserted | ✅ / ❌ | |
| JSON-LD + sitemap + robots present | ✅ / ❌ | |
| TypeScript errors | None / [list] | |
| shadcn/ui components used | [list] | |

Gap Analysis: [What's missing from MVP? What issues were found?]
Testing Steps: [5 concrete steps: open homepage on 375px, submit a reservation, edit a price in /admin, switch to /en, run Lighthouse]
```

### Folge-Prompt-Muster (Feature Extension Mode — für spätere Erweiterungen)
```
## Context
Existing project: Skipper's Bistro website (see master concept). 
Task: Add [Feature] to the existing app.
IMPORTANT: Do NOT modify existing components, hooks, styles or the color tokens unless explicitly required.
## Feature Specification
[...]
## Integration Points
- Connects to: [Komponente/Seite/Tabelle]
- New Supabase table needed: [yes/no – if yes, WITH RLS]
## Credit Optimization: reuse shadcn/ui + existing hooks.
## STATUS REPORT: same table as master prompt.
```

---

# TEIL 12 — CONTENT-CHECKLISTE FÜR DEN KUNDEN (vor Etappe 2 einsammeln)

**Fotos (Shooting, goldene Stunde + Mittag):**
- [ ] Hero: Terrasse mit Blick auf Alten Strom/Masten (quer 3:2 UND hoch 4:5)
- [ ] Gebäude außen: Holzfassade mit Booten davor, 2 Perspektiven
- [ ] Food: Backfisch (Pfanne + Teller), Frühstücksteller, Scampi, Kaffee mit Hafen im Hintergrund, 2 weitere Signature-Gerichte — je quadratisch/4:5
- [ ] Team: 1 Gruppenbild locker + 2 Arbeitsmomente (Küche/Tresen); Vornamen fürs Web freigeben (inkl. „Kiwi" 😉)
- [ ] Innenraum: 3 Motive (Tresen, Fensterplatz, Detail)
- [ ] Zubehör-Regal & Service/Steg-Situation je 2 Motive
- [ ] Einverständniserklärungen abgebildeter Personen

**Fakten & Freigaben:**
- [ ] Logo-Originaldatei (SVG/AI, sonst höchstauflösendes PNG)
- [ ] Vollständige aktuelle Speise-/Getränkekarte mit Preisen + Allergenen (LMIV!)
- [ ] Öffnungszeiten inkl. Küchenschluss + Winter-/Pausenregelung
- [ ] Entscheidung Reservierungstool (eigenes Formular vs. resmio/OpenTable/Quandoo) + ggf. Account
- [ ] Klärung E-Mail-Domain (info@skippers-bistro.de behalten oder @skipper-warnemuende.de einrichten?)
- [ ] Shop-Status ehrlich definieren (Phase 2 Zeithorizont) — bestimmt Copy auf /shop
- [ ] Status „Fischrestaurant Hummerkob" (Partner? Footer-Link ja/nein?)
- [ ] Impressums-/Datenschutzdaten (Betreiber, Rechtsform, USt-ID) — Rechtstexte extern prüfen lassen
- [ ] Google-Business-Profil-Zugang (Website-Link, Menü-Link, Reservieren-Link aktualisieren)
- [ ] Instagram-Account vorhanden/geplant? (Persona 10)
- [ ] 5–8 Lieblings-Gästezitate freigeben (aus Google, sinngemäß gekürzt)

---

# TEIL 13 — LAUNCH-CHECKLISTE

**Vor Live-Gang:**
- [ ] Alle Seiten auf 375/768/1280/1920 px geprüft, echte Geräte (iPhone + Android)
- [ ] Lighthouse mobil ≥ 95/95/95/100; LCP < 1,5 s auf gedrosseltem 4G
- [ ] Formulare: Testeinträge kommen in Supabase UND per Mail an; Honeypot greift
- [ ] Admin: Preis ändern, Event anlegen, Öffnungszeiten-Ausnahme testen
- [ ] JSON-LD mit Google Rich Results Test validiert (Restaurant, Menu, FAQ, Event)
- [ ] hreflang/canonical geprüft; OG-Vorschau in WhatsApp/LinkedIn getestet
- [ ] 404-Seite, /impressum, /datenschutz final (juristisch geprüft!)
- [ ] Alle Bilder: alt-Texte, width/height, AVIF/WebP, lazy unterhalb Fold

**Cutover:**
- [ ] DNS auf Lovable-Hosting; SSL aktiv; www → non-www 301
- [ ] **301-Redirect-Map aus Teil 4.3 live und stichprobengeprüft**
- [ ] sitemap.xml in Google Search Console eingereicht; alte Property übernehmen
- [ ] Google Business Profile: neue Links (Website, Menü, Reservierung)
- [ ] Altes CMS: Admin-Zugänge stilllegen, Backup ziehen, Hosting kündigen (nach 4 Wochen Puffer)

**Nach Launch (Woche 1–4):**
- [ ] Search Console: Indexierung + Crawling-Fehler täglich prüfen
- [ ] Erste echte Reservierung end-to-end verfolgen (Antwortzeit!)
- [ ] Team-Einweisung Admin (30-Min-Screencast aufnehmen)
- [ ] Bewertungs-Routine: QR-Code „Bewerten Sie uns" auf Bon/Tischaufsteller

---

# TEIL 14 — ROADMAP PHASE 2 (nach stabilem Betrieb, ~Monat 3–6)

1. **Online-Shop** (eigenständiges Projekt: Sortiment, Lager, Versandlogik, Payment — Stripe; Anbindung an /bootszubehoer-Kategorien).
2. **Gutschein-Verkauf** (PDF-Generierung, Q4-Umsatzhebel).
3. **Live-Google-Rating** via Places API statt manueller Pflege.
4. **Echtzeit-Reservierung** (falls Phase 1 mit Formular startete): resmio/OpenTable-Embed + Tischplan.
5. **Newsletter** (Double-Opt-in; Anlässe: Saisonstart, Heringswochen, Hanse Sail).
6. **Content-Ausbau SEO:** /terrasse ausbauen, Saison-Landingpages („Hanse Sail Warnemünde essen", „Silvester am Hafen"), Blog light über /news.
7. **Instagram-Aufbau** + Feed-Einbindung (Persona 10) — erst wenn Kanal bespielt wird.

---

*Ende des Masterdokuments. Bei Umsetzungsfragen gilt die Hierarchie: Teil 2 (CI) und Teil 3 (Anti-KI-Manifest) sind unverhandelbar; Teil 5–6 (Struktur & Copy) verbindlich mit Spielraum im Detail; Teil 11 ist das ausführbare Artefakt daraus.*
