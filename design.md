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

## Komponenten-Konventionen
- CTA-Buttons: `--clay` Hintergrund, `--radius-sm`, Hover dunkler (`--clay-deep`).
- Karten (`.member`, Session-Karten etc.): Bild + `aspect-ratio` gesetzt — **immer mit `height:auto` ergänzen**, sonst kann eine ältere `height`-Regel (z. B. `.member .ph{height:130px}`) reinsickern und `aspect-ratio` aushebeln (flache Bildstreifen — bereits einmal passiert, s. Memory).
- Trustpilot-Stimmen (4,6/5, 22 Bewertungen) — Zitate bleiben HWG-sicher: nur Atmosphäre/Sicherheit/Präsenz, keine Heilversprechen.
- Cookiefrei by Design: keine Tracker, keine externen Requests außer Web3Forms (falls Formular).

## Technik-Constraints
- Fonts lokal gehostet, subsettet (`pyftsubset`, Latin+Deutsch, `--layout-features=kern,liga`) — keine Google Fonts CDN-Requests.
- Ziel-Werte: Lighthouse Performance ≥ 90, A11y 100, 0 externe Tracker.
- Mehrseitig: `index.html` (Start), `breathwork-ausbildung.html` (Ausbildung), `impressum.html`, `datenschutz.html`.

## Wann diese Datei aktualisieren
Nach jeder bestätigten Design-Entscheidung (neue Farbe, neue Komponente, Foto-Regel) hier ergänzen — nicht nur im Code. Ziel: jeder neue Claude-Code-Prompt zu dieser Seite kann sich allein auf `design.md` stützen, ohne den kompletten CSS-Code neu zu lesen.
