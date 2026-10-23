# Repstar

Persönlicher Krafttraining-Tracker als Progressive Web App fürs iPhone. Minimalistisch, offline-fähig, alle Daten bleiben lokal auf dem Gerät.

> **Weniger Verwaltungsaufwand, mehr Training.**

## Status

**Phase 0 abgeschlossen.** Noch keine lauffähige App. Design und Architektur sind freigegeben, als Nächstes folgt das MVP.

| Phase | Inhalt | Status |
|---|---|---|
| 0 | UX-Design und Mockups | freigegeben (23.10.2026) |
| 0 | Architektur und Datenmodell | freigegeben (23.10.2026) |
| 1 | Funktionales MVP inkl. Tests und GitHub-Pages-Deployment | offen |

## Dokumentation

- [Produkt](.kiro/steering/product.md): Zweck, Zielgruppe, Kern-Features
- [Technik](.kiro/steering/tech.md): Stack, Datenmodell, Exportformat, Einschränkungen
- [Struktur](.kiro/steering/structure.md): Ordner-Layout und Code-Konventionen
- [Design-Spezifikation](docs/design/2026-10-23-repstar-mvp/spec.md): Entscheidungen, Tokens, Interaktionsregeln
- [Referenz-Mockup](docs/design/2026-10-23-repstar-mvp/mockup.html): alle Screens, im Browser öffnen (am besten Safari)

## Datenschutz

- Keine Accounts, keine Cloud, keine Analytics, kein Tracking.
- Trainingsdaten liegen ausschließlich in IndexedDB auf dem Gerät.
- Exporte (`repstar-export-*`, `repstar-backup-*`) sind per `.gitignore` vom Repo ausgeschlossen.
- Browserdaten können durch Nutzer oder Betriebssystem gelöscht werden. Regelmäßige JSON-Backups über die Export-Funktion sind daher Pflicht.

## Entwicklung

Setup, Tests, iPhone-Test und Deployment werden mit Phase 1 dokumentiert. Geplante Befehle stehen in [tech.md](.kiro/steering/tech.md#befehle-ab-phase-1).
