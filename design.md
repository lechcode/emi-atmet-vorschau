# design.md — emi.atmet

Design-System-Referenz für Claude Code. Vor jeder Änderung an Layout, Farben, Typografie oder Komponenten diese Datei lesen und einhalten. Bei Abweichungen: hier zuerst aktualisieren, dann Code anpassen.

## Konzept
Archetyp **„Weiter Atem"**: Editorial-Ruhe × Warm-Organisch. Ruhig, atmend, kein Wellness-Kitsch. Grenzt sich bewusst ab von der Konkurrenz in derselben Nische (Fraunces+Coral+Blau bei nadya-seeberger, kühl-petrol bei joshua-rudisch) — deshalb hier warme Erdtöne + Newsreader statt Fraunces.

## Farben
| Token | Hex | Verwendung |
|---|---|---|
| `--paper` | #F5EFE7 | Haupt-Hintergrund |
| `--paper-2` | #EFE7DB | Sekundär-Flächen |
| `--sand` | #E8DCCB | Karten/Panels |
| `--white` | #FBF8F3 | helle Flächen |
| `--espresso` / `--espresso-2` | #2A2521 / #211d19 | dunkle Sektionen (Hero Ausbildung) |
| `--ink` | #2C2824 | Fließtext |
| `--ink-soft` | #5C534A | Sekundärtext |
| `--ink-faint` | #726757 | Meta/Captions |
| `--clay` / `--clay-deep` | #B4694E / #9A5238 | **CTA-Farbe**, Akzent |
| `--clay-soft` | #EBD7CC | CTA-Hintergrund hell |
| `--sage` / `--sage-deep` | #8A9A82 / #54654C | Zweitakzent (Natur/Ruhe) |
| `--sage-soft` | #E1E7DC | Zweitakzent-Fläche |
| `--mocha` | #A47864 | Foto-Rahmen/Deko |
| `--bronze` | #875426 | Akzent Ausbildungsseite (Gold #C99A5B nur auf Dunkel — Kontrast auf hell reicht nicht) |

**Regel:** Terrakotta (`--clay`) ausschließlich für CTAs/Handlungsaufforderungen. Salbei für ruhige Zweit-Akzente. Nie beide gleichzeitig gleich gewichtet in einer Komponente.

## Typografie
- **Serif (Headlines):** `Newsreader` — bewusst *nicht* Fraunces (Differenzierung von Wettbewerb).
- **Sans (UI/Fließtext):** `Instrument Sans`.
- Fluid Type-Scale via `clamp()`: `--step--1` bis `--step-5` (13px–62px). Immer diese Stufen verwenden, keine Fixgrößen.
- Line-height: `--lh-tight` (1.06, Headlines) / `--lh-body` (1.68, Fließtext).
- Letter-spacing: Labels/Kicker `--ls-label: .15em` (Uppercase-Kicker-Stil).

## Spacing & Radius
- Abstände als fluid `clamp()`-Stufen: `--sp-2` (14–18px) bis `--sp-6` (62–120px). Keine festen px-Werte für Section-Paddings.
- Radius: `--radius` 16px (Standard), `--radius-lg` 24px (große Karten/Bilder), `--radius-sm` 11px (Buttons/Chips), `--pill` 999px (Pills/Badges).
- Schatten: `--shadow-soft` für Karten, `--shadow` für schwebende/erhöhte Elemente.
- Easing: `--ease: cubic-bezier(.22,.61,.36,1)` für alle Transitions.

## Bildsprache
- **Nur echte Kundenfotos** (12 Profifotos von Fotograf HY3A, Emi + Rouven). Nie Stock.
- Porträts oft im **Bogenrahmen** (organische Kontur statt hartem Rechteck) — Signatur-Element der Marke.
- Atem-Signatur: dekorative SVG-Welle als wiederkehrendes Grafikmotiv.
- Hero-Regel: **Vollbild-Cinematic nur mit ruhiger Negativfläche + gemessenem Kontrast.** Bei belebten Gruppenfotos → Split-Hero (Text auf eigenem Panel, nie über Gesichtern). Fehler aus Runde 2 (Volltext über Gruppenfoto) korrigiert — nicht wiederholen.
- **Atem-Welle im Hero (`.ahero-wave`):** dünne Sinuslinie (`--clay`, opacity ~.15) als leise Marken-Signatur im dunklen Ausbildungs-Hero — hinter dem Text (`z-index:0`, Grid `z-index:1`). Gleiches Motiv wie `.breath` und die Header-Welle (`.nav-wave`). Atem = Getragensein, nicht Deko.

## Ansprache & Copy-Haltung im Hero
- **Gefühl zuerst, dann Funktion.** Vor dem Texten fragen: *Was soll die Person in den ersten Sekunden fühlen?* Ausbildungs-Hero-Zielgefühl: „Das ruft mich — und ich werde dabei gehalten" (Berufung · Tiefe/Ernst · Sicherheit/Getragensein · Ruhe).
- **Erkennung statt Anweisung.** Nicht „Lerne, X zu tun", sondern die Sehnsucht der Person benennen. Muster: kursive Serif-Erkennungszeile trägt das Gefühl, der Sans-Fließtext trägt die Fakten. Beleg-Claims (Art of Breath Standard, traumasensibel, Termine) bleiben unangetastet — nichts erfinden.
- **Lead-Zeile als Signatur-Element** — beide Heros: `.ahero-lead` (Ausbildung, dunkel, Gold-Akzentbalken `rgba(234,192,142,.5)`) und `.hero-lead` (Startseite, hell, **Mocha**-Akzentbalken `--mocha` — `--clay` bleibt CTA-only!). Serif kursiv, `border-left:2px`, `padding-left:18px`. Startseite ist damit auf Augenhöhe mit dem Ausbildungs-Hero.

## Motion-System (zurückhaltend, `prefers-reduced-motion`-sicher)
- **Atem-Linie zeichnet sich** (`.breath`, Startseite): Pfad hat `pathLength="1"`; `.js .breath path{stroke-dasharray:1;stroke-dashoffset:1}`, beim Reveal (`.breath-wrap.in`) → `stroke-dashoffset:0` (1,9 s). Fallback ohne JS = volle Linie. Reduced-motion = volle Linie statisch.
- **Sanfter Parallax auf Cine-Bändern** (beide Seiten): `.cine .bg` mit `top/bottom:-10%` Overscan (von `.cine{overflow:hidden}` geclippt), JS verschiebt `.cine .bg img` per `translate3d` (rAF-throttled, ±22px). Reduced-motion → `transform:none`.
- **Reveals** über IntersectionObserver (`.reveal` → `.in`), Stagger `.d1–.d3`. Grundregel: nie Layout-verschiebende Animationen (nur `transform`/`opacity`/`stroke-dashoffset`).

## Komponenten-Konventionen
- CTA-Buttons: `--clay` Hintergrund, `--radius-sm`, Hover dunkler (`--clay-deep`).
- **Karten-Hover einheitlich (ruhig):** helle Karten (`.offer`, `.voice`, `.member`, `.forwho .w`) → `translateY(-3…-4px)` + Elevation `--shadow` + Border `--line-2`. Dunkle Karten auf Espresso (`.step`, `.area`) → `translateY(-4px)` + `background rgba(245,239,231,.08)` + `border rgba(245,239,231,.26)`. Basis-Tint der Dunkel-Karten `.05`, Border `.16` (nicht `.03/--line-d` — war zu unsichtbar). Alle mit `transition … .35s var(--ease)`.
- **Section-Headline-Größen nie inline** — Klassen `h2.sec-h3` / `h2.sec-h4` (Token-Disziplin; kein `style="font-size:var(--step-*)"`).
- Karten mit Bild + `aspect-ratio`: **immer `height:auto`** ergänzen (sonst sickert alte `height`-Regel rein).
- Trustpilot-Stimmen (4,6/5, 22 Bewertungen) — HWG-sicher: nur Atmosphäre/Sicherheit/Präsenz. **Kein `AggregateRating` im JSON-LD** (self-serving — bewusst weggelassen, nicht nachrüsten).
- Cookiefrei by Design: keine Tracker, keine externen Requests außer Web3Forms (falls Formular).

## Technik-Constraints
- Fonts lokal gehostet, subsettet (`pyftsubset`, Latin+Deutsch, `--layout-features=kern,liga`) — keine Google Fonts CDN-Requests.
- Ziel-Werte: Lighthouse Performance ≥ 90, A11y 100, 0 externe Tracker.
- Mehrseitig: `index.html` (Start), `breathwork-ausbildung.html` (Ausbildung), `impressum.html`, `datenschutz.html`.

## Wann diese Datei aktualisieren
Nach jeder bestätigten Design-Entscheidung (neue Farbe, neue Komponente, Foto-Regel) hier ergänzen — nicht nur im Code. Ziel: jeder neue Claude-Code-Prompt zu dieser Seite kann sich allein auf `design.md` stützen, ohne den kompletten CSS-Code neu zu lesen.
