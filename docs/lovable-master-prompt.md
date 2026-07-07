# Lovable Master-Prompt (gesendet 2026-07-07)

Projekt: `c410c7fe-e1e1-4b85-bc91-5cef9f3254ea` · Workspace „Manuel's Lovable"
Quelle: Teil 11 des Relaunch-Konzepts, ergänzt um Redirect-Map (Teil 4.3), Meta-Titles (Teil 6.3), /terrasse-Landingpage und Platzhalter-Politik (Teil 2.4).

## Initial-Message (Auszug der Ergänzungen gegenüber Teil 11)

- **IMAGE PLACEHOLDER POLICY:** keine Stock-/KI-Bilder; `<PhotoPlaceholder label="FOTO: …"/>`-Plates in Sand/Navy mit Motiv-Beschriftung, feste width/height (CLS-final).
- **Logo-Interim:** Text-Brandmark „SKIPPER'S" (Archivo 900) + Steuerrad-SVG in einer austauschbaren `<Logo />`-Komponente; Original-Logo wird nachgeliefert.
- **Redirects:** `/public/_redirects` mit 301s für alle alten indexierten `.php`-URLs.
- **Meta-Titles/Descriptions:** exakt nach Copy-Deck 6.3 pro Route.
- **/terrasse** als dritte SEO-Landingpage ergänzt.

Der vollständige Prompt-Text ist 1:1 Teil 11 des Konzepts (`relaunch-konzept.md`, Zeilen 443–554) mit obigen Ergänzungen — nachlesbar in der Lovable-Projekt-Historie (erste User-Message).

## Phasen-Vereinbarung mit dem Lovable-Agenten

Der Agent schlug einen 4-Phasen-Build vor (bestätigt):

1. **Phase 1:** Design-System, Layout (Header/Footer/Bottom-Bar/404), Homepage (8 Sektionen), Lovable Cloud + Supabase-Schema + RLS + Seed, Reservierungs-Modal, `/speisekarte`, robots/sitemap/_redirects/Restaurant-JSON-LD
2. **Phase 2:** alle Unterseiten + Formulare + per-Page-JSON-LD
3. **Phase 3:** `/admin` (Auth + CRUD) + Edge-Function-Mail (Stub, Key später)
4. **Phase 4:** `/en`-Routen + i18n-Dictionaries + hreflang

## Fixierte Entscheidungen

| Punkt | Entscheidung |
|---|---|
| Admin-Whitelist | info@skippers-bistro.de + info@gradore.de |
| Mail-Versand | Edge Function als sauberer Stub (Log + TODO); Formular-Erfolg hängt nur am Supabase-Insert |
| Google-Profil-Link | Konstante `GOOGLE_PROFILE_URL` = Maps-Search-URL, später gegen exakte Business-Profil-URL tauschen |
| Logo | Interim-Brandmark, Originaldatei folgt |
