# Struktur und Code-Konventionen

## Ordner-Layout (Zielbild ab Phase 1)

```
repstar/
├── .github/workflows/      # deploy.yml: Lint, Tests, Build, GitHub Pages
├── .kiro/steering/         # product.md, tech.md, structure.md
├── docs/
│   └── design/             # Design-Spezifikationen und Mockups (otto-design-Output)
├── public/                 # Icons, apple-touch-icon, favicon (statisch)
├── src/
│   ├── app/                # App-Shell, Router, Tab-Bar, Provider, Update-Banner
│   ├── features/           # Ein Ordner pro Bereich, enthält Screens + bereichsspezifische Komponenten
│   │   ├── dashboard/
│   │   ├── workout/        # Aktive Trainingseinheit, Satz-Erfassung
│   │   ├── exercises/      # Übungskatalog
│   │   ├── plans/          # Trainingsplan-Editor
│   │   ├── history/
│   │   ├── progress/
│   │   └── settings/       # inkl. Export/Backup
│   ├── db/
│   │   ├── schema.ts       # Dexie-Instanz, Tabellen, Versionen
│   │   ├── migrations/     # Eine Datei pro Schema-Version
│   │   ├── seed/           # Standardübungen und Standardplan
│   │   └── repositories/   # Einzige Schreib-/Lesestelle für Features
│   ├── domain/             # Reine Logik und Typen: Vorschläge, Volumen, PRs, Export-Mapping
│   ├── ui/                 # Wiederverwendbare, rein präsentationale Komponenten
│   ├── lib/                # Kleine Helfer: Datum, Formatierung, Teilen/Download
│   ├── styles/             # tokens.css (Design-Tokens), global.css
│   └── main.tsx
├── tests/
│   ├── e2e/                # Playwright-Szenarien
│   └── fixtures/           # Ausschließlich synthetische Testdaten
├── index.html
├── vite.config.ts
└── package.json
```

`.otto-design/` enthält nur temporäre Design-Session-Dateien und ist gitignored.

## Code-Konventionen

**Allgemein**
- TypeScript `strict`, kein `any` (stattdessen `unknown` + Narrowing).
- Code, Bezeichner und Kommentare auf Englisch, UI-Texte auf Deutsch.
- Named Exports, keine Default Exports (Ausnahme: Konfigurationsdateien).
- Dateinamen: Komponenten `PascalCase.tsx`, alles andere `kebab-case.ts`.
- Kleine, fokussierte Module; keine vorgezogenen Abstraktionen.

**React**
- Nur Funktionskomponenten und Hooks.
- Daten über Hooks aus `db/repositories` (`useLiveQuery`), nie direkter Dexie-Zugriff in Komponenten.
- Kein globaler State-Manager; UI-State lokal, persistenter State in IndexedDB.
- Barrierefreiheit: semantische Elemente, `aria-label` für Icon-Buttons, Tap-Ziele ≥ 44 × 44 pt, ausreichender Kontrast.

**Daten**
- Alle Schreibvorgänge über Repository-Funktionen, die `updatedAt` setzen.
- Keine harten Deletes für Nutzerdaten; Archivieren bzw. Soft-Delete.
- Jede Schemaänderung = neue Dexie-Version + Migrationstest. Bestehende Versionen werden nie nachträglich geändert.
- Exportformat ist versioniert (`formatVersion`); Änderungen sind abwärtskompatibel oder erhöhen die Version.

**Styling**
- Farben, Abstände, Radien und Typografie nur über Tokens aus `styles/tokens.css` bzw. die Tailwind-Konfiguration.
- 4/8-pt-Raster, Systemschrift (`-apple-system`/SF Pro), keine externen Fonts.

**Tests**
- Unit-Tests liegen neben der Datei: `foo.ts` → `foo.test.ts`.
- DB-Tests laufen gegen `fake-indexeddb`, jede Test-Suite mit frischer Datenbank.
- E2E-Tests in `tests/e2e/`, WebKit mit iPhone-Profil.
- Keine echten Trainingsdaten in Tests oder Fixtures.

**Git**
- Branch `main` wird automatisch deployed; Arbeit auf Feature-Branches.
- Conventional Commits (`feat:`, `fix:`, `test:`, `docs:`, `chore:`).
