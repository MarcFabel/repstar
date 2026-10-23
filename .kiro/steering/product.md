# Produkt: Repstar – Personal Strength Tracker

## Zweck

Repstar ist eine minimalistische Progressive Web App fürs iPhone, die das Krafttraining im Fitnessstudio begleitet. Sie ersetzt eine bisherige HTML-basierte Trainingsdokumentation.

**Produktprinzip: Weniger Verwaltungsaufwand, mehr Training.**

Die App muss im Studio mit einer Hand, zwischen zwei Sätzen und mit möglichst wenigen Eingaben bedienbar sein.

## Zielgruppe

Ein einzelner Nutzer (persönliche App, keine Accounts):

- Einstieg ins regelmäßige Krafttraining, sitzende Tätigkeit
- Rhythmus: ca. 2× pro Woche, 60–70 Minuten Ganzkörpertraining
- Ziele: Rücken, Schultern und Rumpf kräftigen, bessere Haltung, allgemeine Fitness, moderater Muskelaufbau
- Nutzt die Trainingsdaten regelmäßig zur Analyse in ChatGPT (deshalb ist der Export zentral)

## UX-Grundsätze

- **Ein Tap zum Training:** „Training starten" ist die dominante Aktion auf dem Dashboard.
- **Vorbefüllen statt eintippen:** Gewicht und Wiederholungen werden aus dem letzten Training vorgeschlagen. Ein unveränderter Satz wird mit einem Tap bestätigt.
- **Daumenzone:** Primäraktionen im unteren Bildschirmdrittel, Tap-Ziele ≥ 44 × 44 pt.
- **Dark Mode zuerst**, Light Mode folgt dem System.
- **Nichts geht verloren:** Jede Änderung wird sofort gespeichert. Eine unterbrochene Einheit wird beim nächsten Öffnen fortgesetzt.
- **Backup sichtbar machen:** Die App erinnert an Exporte, weil Browserdaten verloren gehen können.
- UI-Sprache: Deutsch.

## Kern-Features (MVP)

1. **Standardtrainingsplan** als editierbare Startvorgabe (siehe unten)
2. **Übungskatalog:** durchsuchen, nach Muskelgruppe filtern, eigene Übungen anlegen, bearbeiten, archivieren
3. **Trainingspläne:** Übungen hinzufügen/entfernen, Reihenfolge ändern, Ziel-Sätze/-Wiederholungen bearbeiten; Datenmodell unterstützt mehrere Pläne
4. **Training durchführen:** Einheit mit Datum, Startzeit, Status; Gewicht (kg) und Wiederholungen pro Satz; Satz abhaken; Vorschlag aus letztem Training; spontane Zusatzübungen (nur heute oder dauerhaft in den Plan übernehmen); Übung für heute ersetzen oder überspringen; pausieren, fortsetzen, unvollständig abschließen
   - Schrittweite für Plus/Minus pro Gerät einstellbar
   - Countdown für Zeitübungen (Planks), 1 Tap für Ergometer und Mobilität
   - Dezente Pausenanzeige nach jedem Satz, Schwelle einstellbar
   - Abschluss mit Kennzahlen, Bestleistung und Wochenziel (Standard 2 Trainings pro Woche)
5. **Trainingshistorie:** alle Einheiten mit Datum, Dauer, Übungen, Sätzen, Status, Notizen; nachträglich korrigierbar; positive Markierungen (Bestleistungen, neue Übung)
6. **Fortschritt:** Einheiten pro Woche, Gewichtsentwicklung, Volumen pro Übung, persönliche Bestleistungen (bewusst einfach)
7. **Export:** CSV (eine Zeile pro Satz) und versioniertes JSON (vollständiges Backup), über das iOS-Teilen-Menü oder als Datei
8. **Offline & lokal:** vollständig offline, alle Daten nur auf dem Gerät, keine Analytics, kein Tracking
9. **Deployment:** GitHub Pages, installierbar über Safari „Zum Home-Bildschirm"

## Übungstypen

| Typ | Erfasst | Beispiele |
|---|---|---|
| Kraft (`strength`) | Gewicht (kg) × Wiederholungen pro Satz | Beinpresse, Latzug |
| Zeitbasiert (`timed`) | Dauer pro Satz | Planks, Ruderergometer |
| Mobilität (`mobility`) | Erledigt / Dauer gesamt | Mobilität 5 Min |

## Standardtrainingsplan (Startvorgabe, frei editierbar)

| # | Übung | Typ | Ziel |
|---|---|---|---|
| 1 | Ruderergometer | zeitbasiert | 8 Min Aufwärmen |
| 2 | Beinpresse | Kraft | 2 × 10–12 |
| 3 | Rudermaschine | Kraft | 3 × 10–12 |
| 4 | Brustpresse | Kraft | 2 × 10–12 |
| 5 | Latzug | Kraft | 2 × 10–12 |
| 6 | Reverse Butterfly | Kraft | 3 × 12–15 |
| 7 | Rückenstrecker | Kraft | 2 × 10–12 |
| 8 | Face Pulls | Kraft | 2 × 12–15 |
| 9 | Planks | zeitbasiert | 2 × 30–45 Sek |
| 10 | Mobilität | Mobilität | 5 Min |

Der Plan ist **Seed-Daten**, kein fest verdrahteter Ablauf im Code.

## Nicht Bestandteil des MVP

Lauf-/Ausdauertracking, GPS, Apple Health, Benutzerkonten, Cloud-Sync, Social Features, KI-Plangenerierung, Datenimport.
Cloud-Sync und Import sollen später möglich sein, ohne das Datenmodell umzubauen.

## Ideen für später

- Wochenrückblicke mit konkreten Hinweisen („seit 4 Trainings gleich, mehr Gewicht versuchen?“)
- Planwechsel direkt über den Plannamen auf dem Dashboard

## Design

Freigegebene Gestaltung, Tokens und Interaktionsregeln: `docs/design/2026-10-23-repstar-mvp/spec.md`, Referenz-Mockup daneben (`mockup.html`).

## Erfolgskriterien

- Vom App-Start zum ersten erfassten Satz: ≤ 2 Taps
- Satz mit unveränderten Vorschlagswerten erfassen: 1 Tap
- Backup-Export: ≤ 3 Taps aus den Einstellungen
- Kein Datenverlust bei App-Neustart, Update oder Abbruch mitten im Training
