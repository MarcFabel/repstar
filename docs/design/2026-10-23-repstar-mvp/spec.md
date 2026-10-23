# Repstar MVP · Design-Spezifikation

Stand: 23. Oktober 2026 · freigegeben in der Design-Session (otto-design, 11 Runden)
Referenz-Mockup: [mockup.html](mockup.html) (eigenständig, im Browser öffnen, am besten Safari)

Diese Spezifikation ist die verbindliche Vorlage für die Umsetzung in Phase 1. Jede Entscheidung steht mit ihrem Grund, damit spätere Änderungen bewusst passieren.

## 1. Leitidee

**Ruhig & präzise. Ein Werkzeug, das im Studio genau eine Frage beantwortet: „Was mache ich jetzt?“**

- Dunkel zuerst, eine Schriftfamilie, große Zahlen, Farbe nur als Information.
- Der häufigste Fall kostet die wenigsten Taps: Satz mit Vorschlagswerten bestätigen = 1 Tap.
- Abweichungen (besetztes Gerät, weniger Wiederholungen, Training früher beenden) sind normal und werden nie als Fehler dargestellt.

## 2. Entscheidungen

| Bereich | Entscheidung | Warum |
|---|---|---|
| Aktive Einheit | **Fokus zuerst, Plan-Liste bei Bedarf** (Sheet über das Listen-Symbol) | Normalfall ohne Navigation, besetztes Gerät in 2 Taps. Die Checkliste dient zur Auswahl. |
| Satz erfassen | Zwei große Wertkarten (kg, Wdh.) mit Plus/Minus. Schrittweite steht klein darunter und ist pro Gerät einstellbar. Zahl antippen öffnet die Zifferntastatur mit Schnelltasten −5 / −2,5 / +2,5 / +5 | Häufige Änderungen (−1 Wdh., +2,5 kg) kosten genau 1 Tap. Die Schrittweite muss sichtbar sein, sonst ist unklar, was „+“ tut. |
| Vergleich | Mehr als letztes Mal = Mint, weniger = neutral grau, gleich = „wie Di“ | Weniger Wiederholungen nach einer Gewichtserhöhung sind gewollt. Rot würde falsch werten. |
| Zeitübungen | Countdown mit sich füllendem Ring. Vor dem Start ± neben dem Ring (Schritt 5 s). Startwert = gleicher Satz vom letzten Mal. Stopp speichert die gehaltene Zeit, bei 0:00 automatisch | Das Handy ist die Uhr. Gleiche Bedienung wie bei den Gewichten. |
| Ergometer & Mobilität | 1 Tap „erledigt“, Dauer vorbelegt, „Dauer ändern“ optional. Mobilität zeigt die Übungsbeschreibung als Spickzettel | Das Gerät zeigt die Zeit selbst; Mobilität zählt nur „gemacht“. |
| Nach dem Satz | Nächster Satz sofort, „Satz gespeichert · Rückgängig“ für 4 s, „Pause 0:42“ zählt dezent über dem Button mit, ab Schwelle (Standard 1:00) als „bereit“ | Setzt 60–120 s Pause um, ohne Extra-Tap und ohne Alarm. |
| Abschluss | Häkchen + Wochenziel, dann **Kennzahlen groß zuerst** (Volumen-Hauptkarte mit Vergleich, Dauer, Sätze), dann eine Bestleistung, dann Frage zur Zusatzübung, optionale Notiz | Wunsch des Nutzers: KPIs sind das Wichtigste. Ein Höhepunkt statt Konfetti. |
| Trainings-Menü (•••) | Übung hinzufügen · Übung ersetzen (nur heute) · Übung überspringen · Pausieren · Training beenden | Bildet besetzte Geräte und den Wechsel Face Pulls ↔ Schulterpresse ab. |
| Vorzeitig beenden | Rückfrage mit den offenen Übungen, „Weiter trainieren“ als Primäraktion. Offene Übungen werden als übersprungen gespeichert | Volle Tage sind normal. App schließen geht auch, das Training wartet. |
| Typografie | **SF Pro** (Systemschrift), Zahlen mit `tabular-nums` | Passt zu „präzise“, keine externen Fonts. Rounded und Mono wurden verworfen. |
| Farbe | **Weiß handelt, Mint informiert.** Buttons weiß (Light Mode: schwarz), Mint `#34D399` nur für erledigt/besser/Bestleistung | Jedes Grün bedeutet „geschafft oder besser“. Ein weißer Button ist im Studio am besten sichtbar. Volt wurde abgelehnt. |
| Navigation | **Startseite als Übersicht, keine Tab-Leiste.** Plan + Woche oben, vier beschriftete Bereiche mit Live-Info, „Training starten“ in der Daumenzone | Die App wird meist nur zum Starten geöffnet. Alles andere ist trotzdem 1 Tap entfernt. |
| Fortschritt | Übersicht: Trainings pro Woche (Balken + Ziellinie), Kennzahlen, jede Übung mit Mini-Kurve und Veränderung in 4 Wochen. Tap = Übungsdetail mit Kurve, Gewicht/Volumen-Umschalter, letzten Trainings | Alle vier Pflichtinhalte auf einen Blick, Tiefe nur auf Wunsch. |
| Verlauf | Nach Monat gruppiert, nur **positive Markierungen** („3 Bestleistungen“, „Neu: Seitheben“) | „8 von 10“ war nicht hilfreich. |
| Korrektur | Im alten Training einen Satz antippen, dann öffnet ein Sheet mit denselben Wertkarten wie im Training: Speichern / Satz löschen. Kein eigener Bearbeiten-Modus | Ein Bedienmuster für alles. |
| Einstellungen | Reihenfolge: **Analyse-Export (CSV)** → Training → Darstellung → **Backup (JSON)** zuletzt | Reihenfolge des Nutzers. Sichtbar bleibt das Backup über die Dashboard-Erinnerung. |
| Export | „Exportieren“ öffnet direkt das iOS-Teilen-Menü (Web Share API) mit der Datei | Direkt an ChatGPT senden, ohne Umweg. ChatGPT erscheint, wenn die App installiert ist. |
| Bewegung | Nur Schlüsselmomente animiert (Satz erledigt, Countdown, Abschluss) | Rest bleibt ruhig. |

## 3. Design-Tokens

### Farben · Dark (Standard)

| Token | Wert | Verwendung |
|---|---|---|
| `bg` | `#000000` | Hintergrund |
| `surface-1` | `#141416` | Karten, Wertkarten |
| `surface-2` | `#1C1C1E` | Sheets, runde Kopfzeilen-Buttons |
| `surface-3` | `#2C2C2E` | Plus/Minus, Eingaben, Gruppen im Sheet |
| `surface-4` | `#3A3A3C` | Gedrückt-Zustand, Tastatur-Schnelltasten |
| `separator` | `rgba(255,255,255,.06)` | Trennlinien in Listen (`.08` für Bereichsgrenzen) |
| `text-1` | `#F5F5F7` | Primärtext, Zahlen |
| `text-2` | `rgba(235,235,245,.60)` | Sekundärtext (≈ 5,6 : 1 auf `surface-1`) |
| `text-3` | `rgba(235,235,245,.55)` | Hinweise wie „Schritt 2,5 kg ›“ (≈ 4,7 : 1 auf `surface-1`) |
| `action` / `on-action` | `#F5F5F7` / `#000000` | Primärbuttons |
| `action-soft` | `rgba(255,255,255,.14)` | Sekundärbuttons („Sichern“, „Übernehmen“) |
| `info` / `on-info` | `#34D399` / `#03140C` | Erledigt, Fortschritt, Bestleistung (≈ 9,6 : 1 auf `surface-1`) |
| `info-soft` / `info-line` | `rgba(52,211,153,.10)` / `rgba(52,211,153,.18)` | Bestleistungs-Karte, Markierungen (`.14` für Pills) |
| `warning` | `#FBBF24` | Ausschließlich „Backup überfällig“ |
| `destructive` | `#FF6961` | Ausschließlich Löschen |

### Farben · Light (folgt dem System)

| Token | Wert |
|---|---|
| `bg` / `surface-1` / `surface-3` | `#F2F2F7` / `#FFFFFF` / `#E5E5EA` |
| `text-1` / `text-2` / `text-3` | `#000000` / `rgba(60,60,67,.78)` / `rgba(60,60,67,.72)` |
| `action` / `on-action` | `#000000` / `#FFFFFF` |
| `info` (Flächen, Balken) | `#059669` (≥ 3 : 1 für UI-Elemente) |
| `info-text` | `#047857` (≈ 5,5 : 1 auf Weiß) |
| `warning-text` / `destructive` | `#B45309` / `#D70015` |

Kontrastwerte sind berechnet, nicht mit Hilfstechnik getestet. Eine vollständige WCAG-Prüfung braucht manuelle Tests, z. B. mit VoiceOver.

### Typografie

Eine Familie: `-apple-system, BlinkMacSystemFont, "SF Pro Text", system-ui, sans-serif`. Gewichte 400 / 600 / 700. Alle Zahlen `font-variant-numeric: tabular-nums`. Große Zahlen mit `letter-spacing: -0.03em`, große Titel `-0.02em`.

| Token | Größe / Gewicht | Verwendung |
|---|---|---|
| `num-xl` | 64 / 600 | Gewicht, Wdh., Countdown |
| `num-l` | 48 / 600 | Volumen-Hauptkarte, große Kennzahl |
| `title-l` | 34 / 700 | Übungsname, Screen-Titel |
| `num-m` | 34 / 600 | Sekundäre Kennzahlen |
| `title-m` | 22 / 700 | Sheet-Titel, Planname im Sheet |
| `body` | 17 / 400–600 | Listen, Buttons |
| `callout` | 15 / 400–600 | Zusatzinfos, „Letztes Mal“ |
| `footnote` | 13 / 600 | Abschnitts-Labels (VERSALIEN), Vergleich „↑ 2,5 ggü. Di“ |
| `caption` | 12 / 400 | Schrittweite, Diagrammachsen (11) |

In den Mockups vorkommende 28 und 20 pt werden auf `title-l` bzw. `title-m` vereinheitlicht.

### Abstände, Radien, Größen

- Raster 4/8 pt. Seitenrand: 16 pt für Karten, 20–24 pt für freien Text.
- Radien: Karten 20–24, Sheets 28 (oben), Listenzeilen im Sheet 12, Pills = halbe Höhe.
- Primärbutton: 60 pt hoch, volle Breite, 44 pt über dem Home-Indikator (Safe Area beachten).
- Plus/Minus: 52 pt rund (Sheets 48). Kleinste Tap-Fläche: 44 × 44 pt.
- Listenzeilen: 48 (Sheet) / 52–64 pt (Seiten).

### Bewegung

- Satz erledigt: Häkchen-Puls + Wechsel zum nächsten Satz ≤ 250 ms, Toast 4 s.
- Countdown: Ring füllt sich kontinuierlich; bei 0:00 kurzer Ton (Web Audio), Ring voll mit weichem Glow.
- Abschluss: Häkchen mit Glow blendet ein (≤ 400 ms).
- `prefers-reduced-motion`: nur Überblendungen, keine Skalierung.

## 4. Screens und Zustände

Die Nummern entsprechen den Abläufen im Referenz-Mockup.

**1 · Vor dem Training**
- *Dashboard:* Datum (VERSALIEN), Planname mit ▾ (später Planwechsel), Planinfo, Wochenstreifen Mo–So (erledigt = Mint mit Häkchen, heute = weißer Ring), „x von y Trainings diese Woche“. Vier Bereiche: Verlauf, Fortschritt (grüne Live-Info bei Verbesserung), Plan & Übungen, Einstellungen. Backup-Zeile ab 7 Tagen ohne Backup. „Training starten“ unten.
- *Training fortsetzen:* Statt Start eine Mint-Karte „Training läuft seit …“, nächste Übung, Fortschrittsbalken. Primär „Training fortsetzen“, sekundär „Training beenden“.
- *Erster Start:* Plan ist vorbereitet, Hinweis zu Startgewichten, Datenhinweis „nur auf diesem iPhone“.
- *Plan-Editor:* Entfernen (−), Ziel-Pill antippen = Sätze/Wiederholungen bzw. Dauer ändern, ≡ ziehen = sortieren, „Übung aus Katalog hinzufügen“.

**2 · Im Training**
- *Fokus:* Kopfzeile (Liste · Planname + Laufzeit · •••), 10-teiliger Fortschrittsbalken, „Übung n von m · Muskelgruppe“, Name, Ziel, „Satz x von y“ mit Punkten, Wertkarten, „Letztes Mal · <Tag>“, erledigte Sätze, Pause-Zeile, Primärbutton „Satz x erledigt“.
- *Plan-Liste (Sheet):* Erledigt (gedimmt, Mint), aktuell (weißer Ring, „Satz x von y“), offen (Ring) mit Ziel; Tap springt zur Übung. Unten „Übung hinzufügen“.
- *Zeitübung:* vor dem Start ± am Ring + „Start“; läuft: Ring füllt sich, Markierung bei Ziel-Minimum, Hinweis „im Ziel · x s gehalten“, Button „Stopp & speichern“ (`surface-3`); fertig: Ring voll, „Satz gespeichert“, „Weiter zu Satz 2“ / „Rückgängig“.
- *Übung hinzufügen:* Sheet mit Suche, Muskelgruppen-Chips, Liste (Typ · Ausstattung), Plan-Übungen als „Im Plan“, „Eigene Übung anlegen“. Hinweis: gilt für heute, die Plan-Frage kommt am Ende.
- *Menü und Beenden:* siehe Entscheidungen.
- *Abschluss:* siehe Entscheidungen. Bei „Ersetzen“ lautet die Frage „Schulterpresse statt Face Pulls in den Plan?“ mit „Nur heute“ / „Im Plan ersetzen“.

**3 · Danach**
- *Verlauf:* Monatsgruppen, Zeile = Datum + Markierung, darunter Plan · Dauer · Volumen.
- *Korrektur:* Detail mit allen Übungen und Sätzen; Satz antippen öffnet das Sheet (Wertkarten, Speichern, Satz löschen in `destructive`). „Training löschen“ am Ende des Details.
- *Fortschritt / Übung:* siehe Entscheidungen. Diagramme: Mint-Linie mit weichem Verlauf darunter, letzter Punkt hervorgehoben; Wochenbalken unter dem Ziel in Grau.
- *Einstellungen:* Analyse-Export-Karte · TRAINING (Wochenziel, Pause „bereit“ ab, Standard-Schrittweite) · DARSTELLUNG (System / Dunkel / Hell) · BACKUP (JSON, Datum, `warning` ab 7 Tagen) · Speicherhinweis „Dauerhaft gespeichert“.
- *Teilen:* iOS-Teilen-Menü mit Dateiname und Umfang.

**Leere und Fehlerzustände (in der Umsetzung ergänzen):** Fortschritt mit weniger als 2 Trainings („Nach dem zweiten Training siehst du hier deine Entwicklung“), leere Suche („Keine Übung gefunden · Eigene anlegen“), Teilen nicht verfügbar (Fallback Download), Speicher nicht dauerhaft (Hinweis in Einstellungen mit Backup-Empfehlung).

## 5. Interaktionsregeln

- **Vorschlag Gewicht:** voriger Satz heute → gleicher Satz im letzten Training dieser Übung → Standardwert der Übung → leer (erstes Training).
- **Vorschlag Wiederholungen / Dauer:** gleicher Satz im letzten Training → obere Zielgrenze (Wdh.) bzw. untere Zielgrenze (Dauer).
- **Vergleich:** pro Wert gegen den gleichen Satz im letzten Training. Höher = Mint „↑ x ggü. <Wochentag>“, niedriger = `text-2` „−x ggü. …“, gleich = „wie <Wochentag>“.
- **Schrittweite:** `exercise.weightStepKg ?? settings.defaultWeightStepKg` (Standard 2,5). Wdh. immer 1, Dauer 5 s.
- **Weiterschalten:** nach dem letzten Satz einer Übung zur nächsten offenen Übung in Planreihenfolge.
- **Pause:** Zeit seit `completedAt` des letzten Satzes; „bereit“ ab `settings.restReadySec` (Standard 60).
- **Countdown:** Bildschirm bleibt an (Screen Wake Lock), Ton am Ende. Vibration ist auf iOS für Web-Apps nicht verfügbar.
- **Bestleistung:** Kraft = höchstes Gewicht eines erledigten Satzes (bei Gleichstand mehr Wdh.), Zeit = längste Dauer.
- **Volumen:** Σ Gewicht × Wdh. aller erledigten Kraftsätze.
- **Wochenziel:** ISO-Woche Mo–So, Standard 2 (`settings.weeklyGoal`).
- **Markierung „Neu“:** Übung wurde in diesem Training zum ersten Mal überhaupt gemacht.
- **Backup-Erinnerung:** Dashboard und Einstellungen, wenn `lastBackupAt` älter als 7 Tage oder nie.

## 6. Später (nicht MVP)

- Wochenrückblicke mit Aussagen wie „Rückenstrecker seit 4 Trainings gleich, 32,5 kg versuchen?“ (Variante C der Fortschritts-Runde).
- Planwechsel über den Planname auf dem Dashboard (Datenmodell unterstützt mehrere Pläne bereits).
