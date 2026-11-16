# AI Development Framework – Quick Start

Dieses Framework unterstützt drei Ziele:

1. Meine Fähigkeiten als Entwickler verbessern.
2. Die Zusammenarbeit mit AI-Agenten kontinuierlich verbessern.
3. Die Qualität des Projekts sicherstellen.

Es ist unabhängig vom verwendeten AI-Agenten. Alle Agenten sollen dieselben Projektregeln und Arbeitsweisen berücksichtigen.

## 1. Neue Aufgabe starten

Beschreibe dem AI-Agenten, was du erreichen möchtest.

Wähle einen passenden Arbeitsmodus:

| Modus | Wann? |
|---|---|
| Independent Implementation | Ich möchte selbst programmieren und lernen. |
| Guided Implementation | Ich möchte verstehen, wo und wie ich etwas ändern soll. |
| AI-Assisted Implementation | Der AI-Agent soll die Implementierung übernehmen. |

Der Agent soll die vorhandenen Framework-Dokumente und gegebenenfalls seine verfügbaren Spezialfunktionen berücksichtigen.

## 2. Selbst implementieren und vergleichen

Mein bevorzugter Lernmodus:

1. Aufgabe verstehen und offene Fragen klären.
2. Lösung selbst überlegen und implementieren.
3. Optional eine AI-Referenzlösung in `.scratch/drafts/` erstellen lassen.
4. Meine Implementierung mit der Referenz vergleichen lassen.
5. Unterschiede verstehen und bewerten.
6. Wichtige Erkenntnisse in `notes/` dokumentieren.

**Wichtig:** Erst selbst implementieren, dann mit dem AI-Draft vergleichen. Der Draft ist nur eine Referenz.

## 3. Änderungen planen lassen

Wenn ich eine bestehende Funktion verstehen oder selbst verändern möchte:

- Einen geeigneten Planungs- oder Analysemodus verwenden.
- Den Code analysieren und einen Implementierungsleitfaden erstellen lassen.
- Die Änderung selbst umsetzen.
- Bei Bedarf anschließend beide Implementierungen vergleichen.

Verfügbare Spezialfunktionen der jeweiligen Agenten dürfen genutzt werden. Sie ersetzen aber nicht die gemeinsamen Framework-Regeln.

## 4. AI-Agenten implementieren lassen

Wenn ein Agent die Arbeit übernehmen soll:

1. Gewünschte Änderung beschreiben.
2. Umfang und Erwartungen klären.
3. Implementierung erstellen lassen.
4. Änderungen und Tests prüfen lassen.
5. Ergebnis selbst kontrollieren und akzeptieren.

## 5. Aus Fehlern der AI-Agenten lernen

Wenn ein Agent einen Fehler macht:

1. Fehler identifizieren.
2. Ursache analysieren lassen.
3. Verbesserungsvorschlag anfordern.
4. Entscheiden, ob eine neue KI-Regel sinnvoll ist.
5. Nach meiner Freigabe in `AI_RULES.md` aufnehmen.
6. Wenn möglich, eine automatische Prüfung ergänzen.

**Wichtig:** Kein Agent darf die gemeinsamen AI-Regeln eigenständig ändern.

## 6. Wissen festhalten

Während der Entwicklung:

- Neue Erkenntnisse → `notes/`
- Design Patterns → `patterns/`
- Architekturentscheidungen → `docs/decisions/`
- AI-Verhaltensregeln → `AI_RULES.md`
- Projektregeln → `RULES.md`

Nur relevante Erkenntnisse dokumentieren.

## 7. Wichtige Dateien

| Datei / Ordner | Zweck |
|---|---|
| `AI.md` | Gemeinsamer Einstiegspunkt für AI-Agenten |
| `GUIDE.md` | Zusammenarbeit und Lernprozess |
| `AI_RULES.md` | Regeln für das Verhalten der AI-Agenten |
| `RULES.md` | Technische Regeln des Projekts |
| `TREE.md` | Repository-Übersicht |
| `PROGRESS.md` | Aktueller Projektstand |
| `.scratch/` | AI-Drafts und Reviews |
| `notes/` | Mein technisches Wissen |
| `patterns/` | Pattern-Training |
| `docs/decisions/` | Architekturentscheidungen |

## Grundregel

**Ich denke, entscheide und lerne. Die AI-Agenten unterstützen, analysieren und verbessern ihre Zusammenarbeit mit mir.**

Nicht jeder Schritt ist bei jeder Aufgabe notwendig. Nutze nur die Teile des Frameworks, die tatsächlich helfen.
