# Technik: Stack, Tools und Einschränkungen

> Status: **Freigegeben** (23.10.2026), ebenso das UX-Design (siehe `docs/design/`). Repository: `MarcFabel/repstar`, GitHub-Pages-Pfad `/repstar/`.
> Abhängigkeiten werden beim Projektsetup mit **exakten Versionen** gepinnt (kein `^`/`~`).

## Stack

| Bereich | Wahl | Begründung |
|---|---|---|
| Sprache | **TypeScript** (strict) | Typsicheres Datenmodell, sichere Migrationen und Exporte |
| UI | **React** | Etabliert, größtes Ökosystem, beste Test-Tooling-Unterstützung |
| Build | **Vite** | Schnell, minimale Konfiguration, `base` für GitHub Pages |
| Styling | **Tailwind CSS** + CSS-Variablen für Design-Tokens | Konsistentes 4/8-pt-Raster, Dark Mode via Tokens, kein CSS-Wildwuchs |
| Lokale DB | **IndexedDB über Dexie.js** | Versionierte Schemas mit `upgrade()`-Migrationen, Indizes, `useLiveQuery` für reaktive UI, gut testbar mit `fake-indexeddb` |
| Routing | **React Router** im Hash-Modus | Keine 404 bei Deep Links auf GitHub Pages, kein `404.html`-Trick nötig |
| PWA | **vite-plugin-pwa** (Workbox) | Manifest, Precache, Offline, kontrolliertes Update (`registerType: 'prompt'`) |
| Icons | **lucide-react** | Konsistent, tree-shakeable |
| Charts | Eigene schlanke **SVG-Komponenten** | Fortschritt bleibt bewusst einfach; Chart-Library erst bei echtem Bedarf |
| Unit-/Integrationstests | **Vitest** + **React Testing Library** + **fake-indexeddb** | Gleiche Konfiguration wie Vite, schnelle Tests von DB-Logik und Migrationen |
| E2E | **Playwright** (WebKit, iPhone-Geräteprofil) | Kommt iOS Safari am nächsten, testet Offline und Persistenz |
| Qualität | ESLint + Prettier | Standard |
| CI/CD | **GitHub Actions** → GitHub Pages | Build, Tests, Deploy bei Push auf `main` |
| Runtime | Node.js LTS | Lokal und in CI; Version via `.nvmrc` |

### Bewusst verworfen

- **localStorage:** zu klein, synchron, keine Migrationen, keine Indizes.
- **SQLite-WASM/OPFS:** mächtig, aber größeres Bundle und mehr Komplexität ohne Mehrwert für einen Einzelnutzer.
- **Next.js / SSR-Frameworks:** kein Server nötig, statisches Hosting reicht.
- **Globaler State-Manager (Redux, Zustand):** IndexedDB ist die Single Source of Truth, `useLiveQuery` + lokaler React-State genügen.
- **Native App / Capacitor:** App-Store-Aufwand ohne Nutzen für das MVP.

## Architekturprinzipien

- **Local-first:** Die Datenbank ist die Wahrheit. Jede Nutzeraktion schreibt sofort in IndexedDB, es gibt keinen „Speichern"-Button.
- **Schichten:** `ui/features` → `db/repositories` → Dexie. Komponenten greifen nie direkt auf Dexie zu.
- **Reine Domänenlogik** (Vorschläge, Volumen, Bestleistungen, Export-Mapping) ohne React/DB-Abhängigkeit, damit sie einfach testbar ist.
- **Sync-fähig vorbereitet:** Jeder Datensatz hat eine UUID (`crypto.randomUUID()`), `createdAt`, `updatedAt` und Soft-Delete (`deletedAt`/`archivedAt`) statt hartem Löschen.
- **Seed statt Hardcode:** Standardübungen und Standardplan werden beim ersten Start als Daten angelegt. Seed-Übungen haben stabile IDs (z. B. `ex-leg-press`), damit Exporte über Zeit vergleichbar bleiben.

## Datenmodell (Entwurf)

Zeitstempel als ISO-8601-Strings in UTC, Gewichte in kg (Dezimalzahl), Dauern in Sekunden.

```ts
type ExerciseType = 'strength' | 'timed' | 'mobility';

interface Exercise {
  id: string;
  name: string;
  muscleGroup: MuscleGroup;          // z. B. 'back' | 'shoulders' | 'core' | 'legs' | 'chest' | 'full_body'
  type: ExerciseType;
  description?: string;              // z. B. Mobilitäts-Spickzettel im Training
  equipment?: string;                // z. B. 'Gerät' | 'Kabelzug' | 'Kurzhantel'
  defaults?: Target;                 // Standard-Sätze/-Wiederholungen bzw. -Dauer
  weightStepKg?: number;             // Schrittweite pro Gerät, sonst settings.defaultWeightStepKg
  isSeed: boolean;
  archivedAt?: string;
  createdAt: string;
  updatedAt: string;
}

interface Target {
  sets: number;
  repsMin?: number; repsMax?: number;            // strength
  durationSecMin?: number; durationSecMax?: number; // timed / mobility
}

interface Plan {
  id: string;
  name: string;
  isDefault: boolean;
  items: PlanItem[];                 // eingebettet: Reihenfolge wird atomar gespeichert
  archivedAt?: string;
  createdAt: string;
  updatedAt: string;
}

interface PlanItem {
  id: string;
  exerciseId: string;
  target: Target;
}

type SessionStatus = 'active' | 'paused' | 'completed' | 'incomplete';

interface Session {
  id: string;
  planId?: string;
  planName: string;                  // Snapshot, falls Plan später geändert wird
  status: SessionStatus;
  startedAt: string;
  endedAt?: string;
  pausedAt?: string;
  pausedDurationSec: number;
  exercises: SessionExercise[];      // Reihenfolge und Ziele dieser Einheit
  notes?: string;
  createdAt: string;
  updatedAt: string;
}

interface SessionExercise {
  id: string;
  exerciseId: string;
  exerciseName: string;              // Snapshot
  type: ExerciseType;
  target: Target;
  source: 'plan' | 'adhoc';          // adhoc = spontan hinzugefügt oder ersetzt
  replacesSessionExerciseId?: string; // gesetzt bei „Übung ersetzen (nur heute)“
  skipped?: boolean;
}

interface SetEntry {                 // eigene Tabelle: kleine Writes pro Satz, 1:1 zum CSV
  id: string;
  sessionId: string;
  sessionExerciseId: string;
  exerciseId: string;
  setNumber: number;
  weightKg?: number;
  reps?: number;
  durationSec?: number;
  completed: boolean;
  completedAt?: string;
  updatedAt: string;
}

interface Settings {                 // Key-Value
  weeklyGoal: number;                // Standard 2 (ISO-Woche Mo–So)
  restReadySec: number;              // Pause gilt ab hier als „bereit“, Standard 60
  defaultWeightStepKg: number;       // Standard 2.5
  durationStepSec: number;           // Standard 5
  theme: 'system' | 'dark' | 'light';
  lastExportAt?: string;             // letzter CSV-Export
  lastBackupAt?: string;             // letztes JSON-Backup, Erinnerung nach 7 Tagen
}
```

Dexie-Tabellen und Indizes (Version 1):

```
exercises: id, name, muscleGroup, type, archivedAt
plans:     id, isDefault
sessions:  id, status, startedAt
sets:      id, sessionId, [exerciseId+completedAt], sessionExerciseId
settings:  key
```

## Export

- **CSV** (eine Zeile pro Satz, UTF-8 mit BOM für Excel):
  `session_id, session_date, session_status, exercise_id, exercise_name, exercise_type, set_number, weight_kg, reps, duration_sec, completed, completed_at, session_notes`
- **JSON** (Backup, späterer Import):
  ```json
  {
    "format": "repstar-backup",
    "formatVersion": 1,
    "dbSchemaVersion": 1,
    "appVersion": "0.1.0",
    "exportedAt": "2026-10-23T18:00:00Z",
    "data": { "exercises": [], "plans": [], "sessions": [], "sets": [], "settings": [] }
  }
  ```
- Teilen über **Web Share API** (`navigator.share({ files })`), Fallback: Blob-Download.
- Dateinamen: `repstar-export-YYYY-MM-DD.csv` / `repstar-backup-YYYY-MM-DD.json`.

## Technische Einschränkungen

- **Statisches Hosting:** GitHub Pages, Projekt-Repo → Vite `base: '/repstar/'`, Manifest `start_url`/`scope` relativ.
- **iOS Safari:** Storage-Isolation zwischen Safari und installierter PWA (Daten liegen getrennt). `navigator.storage.persist()` beim ersten Start anfragen. Safe Areas via `viewport-fit=cover` und `env(safe-area-inset-*)`. `apple-touch-icon` (180 × 180) zusätzlich zu Manifest-Icons.
- **Countdown auf iOS:** Bildschirm per Screen Wake Lock API wach halten, Endsignal per Web Audio (nur im Vordergrund). Die Vibration API gibt es in iOS-Web-Apps nicht. Verhalten auf dem Gerät testen.
- **Design-Vorgaben:** Tokens, Typo-Skala und Interaktionsregeln stehen verbindlich in `docs/design/2026-10-23-repstar-mvp/spec.md`.
- **Updates ohne Datenverlust:** Service-Worker-Updates ersetzen nur App-Dateien, nie IndexedDB. Updates werden nicht während einer aktiven Einheit eingespielt; der Nutzer bestätigt den Reload.
- **Migrationen:** Jede Schemaänderung ist eine neue Dexie-Version mit `upgrade()`-Funktion und einem Migrationstest.
- **Datenschutz:** Keine Analytics, kein Tracking, keine externen Requests (auch keine Web-Fonts/CDNs), keine Trainingsdaten im Repo oder in ausgelieferten Dateien.
- **Performance-Budget:** Initiales JS-Bundle < 200 kB gzip, Interaktion < 100 ms auf einem aktuellen iPhone.

## Befehle (ab Phase 1)

```bash
npm install          # Abhängigkeiten
npm run dev          # Dev-Server (http://localhost:5173/repstar/)
npm run dev -- --host  # im WLAN fürs iPhone erreichbar
npm test             # Vitest, einmaliger Lauf
npm run test:e2e     # Playwright (WebKit)
npm run build        # Produktionsbuild nach dist/
npm run preview      # Build lokal prüfen (inkl. Service Worker)
```
