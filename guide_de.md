# ProPrompt – Der Leitfaden für effektives Prompting

> **Zielgruppe:** Alle, die GitHub Copilot, Copilot Studio Agents oder KI-gestützte Toolchains produktiv einsetzen wollen – auch ohne KI-Vorwissen.

---

## Inhaltsverzeichnis

1. [Markdown-Schnelleinstieg](#1-markdown-schnelleinstieg)
2. [Grundlagen des Promptings](#2-grundlagen-des-promptings)
3. [Dos & Don'ts – Übersicht](#3-dos--donts--übersicht)
4. [Prompting im normalen Arbeitsalltag (Chat & Edit)](#4-prompting-im-normalen-arbeitsalltag-chat--edit)
5. [Prompting im Agent-Modus](#5-prompting-im-agent-modus)
6. [Copilot Studio Agents bauen](#6-copilot-studio-agents-bauen)
7. [Agent-Toolchains entwerfen](#7-agent-toolchains-entwerfen)
8. [Office-Dateien → LLM-freundliche Strukturen](#8-office-dateien--llm-freundliche-strukturen)
9. [Dateien & Formate richtig aufbereiten](#9-dateien--formate-richtig-aufbereiten)
10. [Instruction-Files & Custom Instructions](#10-instruction-files--custom-instructions)
11. [Cheat-Sheet](#11-cheat-sheet)

---

## 1 Markdown-Schnelleinstieg

Markdown ist das Standardformat für Dokumentation und KI-Kontextdateien. Hier die wichtigsten Elemente:

### Textformatierung

```markdown
# Überschrift 1
## Überschrift 2
### Überschrift 3

**Fett**  
*Kursiv*  
~~Durchgestrichen~~  
`Inline-Code`
```

### Listen

```markdown
- Aufzählung Punkt 1
- Aufzählung Punkt 2
  - Unterpunkt

1. Nummerierte Liste
2. Zweiter Punkt
```

### Code-Blöcke

````markdown
```python
def hello():
    print("Hallo Welt")
```
````

### Links & Bilder

```markdown
[Linktext](https://example.com)
![Bildtext](bild.png)
```

### Tabellen

```markdown
| Spalte A | Spalte B |
|----------|----------|
| Wert 1   | Wert 2   |
```

### Warum Markdown für KI?

- LLMs verstehen Markdown-Struktur nativ
- Überschriften schaffen klare Hierarchie → besserer Kontext
- Code-Blöcke werden als Code erkannt (Syntax-Highlighting)
- Tabellen transportieren strukturierte Daten kompakt

---

## 2 Grundlagen des Promptings

### Was ist ein Prompt?

Ein Prompt ist die **Anweisung**, die du an ein KI-Modell sendest. Je klarer und strukturierter dein Prompt, desto besser das Ergebnis.

### Die 4 Säulen eines guten Prompts

| Säule | Beschreibung | Beispiel |
|-------|-------------|---------|
| **Rolle** | Wer soll die KI sein? | *„Du bist ein erfahrener C#-Entwickler."* |
| **Kontext** | Welche Hintergrundinformationen braucht die KI? | *„Wir arbeiten an einer .NET 8 Web-API."* |
| **Aufgabe** | Was genau soll getan werden? | *„Erstelle einen Controller für User-CRUD."* |
| **Format** | Wie soll die Ausgabe aussehen? | *„Gib den Code mit XML-Kommentaren aus."* |

### Das RICE-Prinzip

> **R**olle → **I**nstruktion → **C**ontext → **E**xpected Output

```
Rolle:      Du bist ein Senior DevOps Engineer.
Instruktion: Erstelle ein Dockerfile für eine Node.js 20-App.
Context:     Die App nutzt pnpm, hat ein /src-Verzeichnis und braucht Port 3000.
Expected:    Multi-Stage Dockerfile mit Kommentaren.
```

---

## 3 Dos & Don'ts – Übersicht

### ✅ DOs

| # | Do | Warum |
|---|-----|-------|
| 1 | **Sei spezifisch** | „Erstelle eine TypeScript-Funktion, die ein Array sortiert" > „schreib mir Code" |
| 2 | **Gib Kontext** | Sprache, Framework, Version, Architektur mitgeben |
| 3 | **Definiere das Ausgabeformat** | „Gib JSON aus", „Nutze Bullet Points", „Erstelle eine Tabelle" |
| 4 | **Arbeite iterativ** | Erst Grundgerüst, dann Verfeinerung in Folge-Prompts |
| 5 | **Nutze Beispiele (Few-Shot)** | Zeige 1–2 Beispiele des gewünschten Outputs |
| 6 | **Begrenze den Scope** | Ein Prompt = eine klare Aufgabe |
| 7 | **Nutze Markdown in Prompts** | Überschriften, Listen und Code-Blöcke für Struktur |
| 8 | **Referenziere Dateien** | `#file:src/service.ts` in Copilot Chat nutzen |
| 9 | **Prüfe die Ausgabe** | KI-Output immer reviewen, nie blind übernehmen |
| 10 | **Nutze Custom Instructions** | `.github/copilot-instructions.md` für projektweite Regeln |

### ❌ DON'Ts

| # | Don't | Warum |
|---|-------|-------|
| 1 | **Vage Prompts** | „Mach das besser" → Kein klares Ziel |
| 2 | **Zu viel auf einmal** | „Erstelle mir eine komplette App" → Überforderung |
| 3 | **Kontext vergessen** | Ohne Sprache/Framework rät die KI |
| 4 | **Blind Copy-Paste** | Immer Code lesen und verstehen |
| 5 | **Sensible Daten eingeben** | Keine echten Passwörter, API-Keys oder Kundendaten |
| 6 | **Erwarten, dass es beim ersten Mal perfekt ist** | Iteratives Prompting ist normal |
| 7 | **Negationen nutzen** | „Nutze NICHT var" → Besser: „Verwende const und let" |
| 8 | **Kontext-Fenster überladen** | Nicht ganze Codebases in einen Prompt packen |
| 9 | **Prompt-Sprache wechseln** | Bleib bei einer Sprache pro Konversation |
| 10 | **Agent-Mode für Triviales** | Einfache Edits brauchen keinen Agent |

---

## 4 Prompting im normalen Arbeitsalltag (Chat & Edit)

### Copilot Chat – Best Practices

**Slash-Commands nutzen:**

| Command | Funktion |
|---------|----------|
| `/explain` | Code erklären lassen |
| `/fix` | Fehler beheben |
| `/tests` | Tests generieren |
| `/doc` | Dokumentation erstellen |
| `/new` | Neues Projekt/Datei scaffolden |

**Kontext-Variablen:**

| Variable | Beschreibung |
|----------|-------------|
| `#file` | Spezifische Datei referenzieren |
| `#selection` | Markierten Code referenzieren |
| `#editor` | Aktuellen Editor-Inhalt |
| `#codebase` | Gesamtes Projekt durchsuchen |
| `#terminalLastCommand` | Letzten Terminal-Befehl referenzieren |

### Beispiel-Prompts für den Alltag

**Code-Review:**
```
Überprüfe #selection auf:
1. Potenzielle Bugs
2. Performance-Probleme
3. Best-Practice-Verstöße
Gib Verbesserungsvorschläge als Diff aus.
```

**Refactoring:**
```
Refactore die Funktion in #file:src/utils/parser.ts:
- Extrahiere die Validierungslogik in eine eigene Funktion
- Nutze Early Returns statt verschachtelter If-Blöcke
- Behalte die bestehende Signatur bei
```

**Fehlerbehebung:**
```
Der folgende Fehler tritt auf: #terminalLastCommand
Analysiere den Fehler im Kontext von #file:src/app.ts und schlage eine Lösung vor.
```

---

## 5 Prompting im Agent-Modus

### Was ist der Agent-Modus?

Der Agent-Modus in VS Code erlaubt Copilot, **selbstständig** mehrere Schritte auszuführen:
- Dateien lesen, erstellen und editieren
- Terminal-Befehle ausführen
- Über mehrere Dateien hinweg arbeiten
- Fehler erkennen und selbst korrigieren

### Wann Agent-Modus nutzen?

| Szenario | Agent ✅ | Chat ❌ |
|----------|---------|--------|
| Neues Feature über mehrere Dateien | ✅ | |
| Refactoring eines ganzen Moduls | ✅ | |
| Debugging mit Terminalzugriff | ✅ | |
| Einzelne Funktion schreiben | | ❌ Chat reicht |
| Schnelle Erklärung | | ❌ Chat reicht |

### Effektives Agent-Prompting

**Struktur für Agent-Prompts:**

```markdown
## Ziel
[Was soll am Ende erreicht sein?]

## Kontext
[Relevante Architektur, Technologien, Constraints]

## Schritte
1. [Erster Schritt]
2. [Zweiter Schritt]
3. [Dritter Schritt]

## Anforderungen
- [Nicht-funktionale Anforderung 1]
- [NFR 2]

## Nicht tun
- [Explizite Ausschlüsse]
```

**Praxis-Beispiel:**

```markdown
## Ziel
Erstelle eine REST-API-Route für Benutzerregistrierung.

## Kontext
- Express.js mit TypeScript
- Prisma ORM mit PostgreSQL
- Bestehende Struktur in /src/routes/ und /src/services/

## Schritte
1. Erstelle das Prisma-Schema für User (email, passwordHash, createdAt)
2. Erstelle den Service in /src/services/userService.ts
3. Erstelle die Route in /src/routes/auth.ts
4. Füge Input-Validierung mit zod hinzu
5. Schreibe Tests in /src/__tests__/auth.test.ts

## Anforderungen
- Passwort mit bcrypt hashen
- E-Mail-Validierung
- Bestehende Code-Konventionen einhalten

## Nicht tun
- Keine Änderungen an bestehenden Routes
- Kein neues ORM einführen
```

### Agent-Modus Tipps

1. **Instruction Files nutzen** – `.github/copilot-instructions.md` wird automatisch geladen
2. **Aufgaben klar abgrenzen** – Lieber 3 fokussierte Agent-Sessions als eine riesige
3. **Checkpoints setzen** – Nach jedem Schritt die Änderungen reviewen
4. **Terminal-Output beobachten** – Agent führt Befehle aus, die Nebeneffekte haben können
5. **Undo nutzen** – VS Code kann Agent-Änderungen rückgängig machen

---

## 6 Copilot Studio Agents bauen

### Was ist Copilot Studio?

Microsoft Copilot Studio ermöglicht das Erstellen eigener KI-Agents ohne Code – für Teams, SharePoint, Web und mehr.

### Prompt-Design für Studio Agents

**System-Prompt strukturieren:**

```markdown
# Rolle
Du bist [Name], ein Assistent für [Zweck].

# Fähigkeiten
- Du kannst [Fähigkeit 1]
- Du kannst [Fähigkeit 2]
- Du hast Zugriff auf [Datenquelle]

# Verhalten
- Antworte immer auf [Sprache]
- Nutze einen [formellen/informellen] Ton
- Maximal [X] Sätze pro Antwort

# Grenzen
- Du beantwortest KEINE Fragen zu [Thema]
- Bei Unsicherheit sagst du: "[Fallback-Text]"

# Ausgabeformat
- Nutze Bullet Points für Listen
- Verlinke auf [Quellen] wenn möglich
```

**Praxis-Beispiel – IT-Helpdesk-Agent:**

```markdown
# Rolle
Du bist IT-Helper, der interne IT-Support-Assistent der Firma Contoso.

# Fähigkeiten
- Lösung häufiger IT-Probleme (Passwort-Reset, VPN, Drucker)
- Durchsuchen der internen Wissensdatenbank
- Erstellen von Support-Tickets

# Verhalten
- Antworte auf Deutsch
- Nutze eine freundliche, geduldige Sprache
- Frage nach, wenn das Problem unklar ist
- Gib Schritt-für-Schritt-Anleitungen

# Grenzen
- Keine Änderungen an Produktionssystemen
- Kein Zugriff auf personenbezogene Daten
- Bei Hardware-Problemen: Weiterleitung an physischen Support

# Eskalation
Wenn du das Problem nicht lösen kannst, erstelle ein Ticket mit:
- Problembeschreibung
- Bisherige Schritte
- Dringlichkeit (Niedrig/Mittel/Hoch)
```

### Topics & Trigger-Phrases

| Thema | Trigger-Phrases |
|-------|----------------|
| Passwort-Reset | „Passwort vergessen", „Kennwort zurücksetzen", „kann mich nicht anmelden" |
| VPN-Probleme | „VPN geht nicht", „Verbindung zum Netzwerk", „Remote-Zugriff" |
| Drucker | „Drucker druckt nicht", „Drucker hinzufügen", „Papier-Stau" |

---

## 7 Agent-Toolchains entwerfen

### Was ist eine Agent-Toolchain?

Eine Toolchain verbindet mehrere Agents und Tools zu einem automatisierten Workflow.

### Architektur-Muster

```
[Benutzer-Anfrage]
       ↓
[Orchestrator-Agent]
       ↓
  ┌────┼────┐
  ↓    ↓    ↓
[Agent A] [Agent B] [Agent C]
(Recherche) (Code)  (Review)
  ↓    ↓    ↓
  └────┼────┘
       ↓
[Zusammenfassung]
       ↓
[Benutzer-Antwort]
```

### Toolchain-Prompts gestalten

**Orchestrator-Prompt:**
```markdown
Du bist der Orchestrator. Deine Aufgabe:
1. Analysiere die Benutzer-Anfrage
2. Entscheide, welche Agents benötigt werden
3. Rufe die Agents in der richtigen Reihenfolge auf
4. Kombiniere die Ergebnisse zu einer kohärenten Antwort

Verfügbare Agents:
- @recherche – Sucht in Datenquellen
- @code – Schreibt und überprüft Code
- @review – Prüft Qualität und Sicherheit
```

**Spezialisten-Agent-Prompt:**
```markdown
Du bist der Code-Agent. Du erhältst Aufgaben vom Orchestrator.

Input-Format:
- Aufgabe: [Beschreibung]
- Kontext: [Relevante Dateien/Infos]
- Constraints: [Einschränkungen]

Output-Format:
- Status: [Erfolg/Fehler]
- Ergebnis: [Code/Erklärung]
- Confidence: [Hoch/Mittel/Niedrig]
```

### Praxis: Power Automate + Copilot Studio

```
[Teams-Nachricht]
  → [Copilot Studio Agent] (Klassifizierung)
    → [Power Automate Flow]
      → [SharePoint-Liste aktualisieren]
      → [E-Mail senden]
      → [Teams-Nachricht an Kanal]
```

---

## 8 Office-Dateien → LLM-freundliche Strukturen

### Das Problem

LLMs können keine `.docx`, `.xlsx` oder `.pptx`-Dateien direkt lesen. Diese müssen in textbasierte Formate konvertiert werden.

### Konvertierungs-Guide

#### Word (.docx) → Markdown

**Manuell:**
1. Öffne das Dokument in Word
2. Datei → Speichern unter → **„Nur Text (.txt)"** oder nutze Pandoc
3. Strukturiere mit Markdown-Überschriften nach

**Mit Pandoc (empfohlen):**
```bash
pandoc input.docx -t markdown -o output.md
```

**Mit Python:**
```python
from docx import Document

def docx_to_markdown(filepath):
    doc = Document(filepath)
    md_lines = []
    for para in doc.paragraphs:
        if para.style.name.startswith('Heading 1'):
            md_lines.append(f"# {para.text}")
        elif para.style.name.startswith('Heading 2'):
            md_lines.append(f"## {para.text}")
        elif para.style.name.startswith('Heading 3'):
            md_lines.append(f"### {para.text}")
        elif para.style.name == 'List Bullet':
            md_lines.append(f"- {para.text}")
        else:
            md_lines.append(para.text)
    return "\n\n".join(md_lines)
```

#### Excel (.xlsx) → Markdown/CSV

**Als CSV exportieren:**
1. Datei → Speichern unter → **CSV (Komma-getrennt)**

**Als Markdown-Tabelle (Python):**
```python
import pandas as pd

def xlsx_to_markdown(filepath, sheet_name=0):
    df = pd.read_excel(filepath, sheet_name=sheet_name)
    return df.to_markdown(index=False)
```

**Tipp:** Für große Tabellen nur relevante Spalten/Zeilen extrahieren:
```python
df = pd.read_excel("data.xlsx")
# Nur relevante Spalten
subset = df[["Name", "Status", "Datum"]].head(50)
context = subset.to_markdown(index=False)
```

#### PowerPoint (.pptx) → Markdown

```python
from pptx import Presentation

def pptx_to_markdown(filepath):
    prs = Presentation(filepath)
    md_lines = []
    for i, slide in enumerate(prs.slides, 1):
        md_lines.append(f"## Folie {i}")
        for shape in slide.shapes:
            if shape.has_text_frame:
                for para in shape.text_frame.paragraphs:
                    if para.text.strip():
                        md_lines.append(para.text)
        md_lines.append("")  # Leerzeile
    return "\n".join(md_lines)
```

#### PDF → Text

```bash
# Mit pdftotext (poppler-utils)
pdftotext input.pdf output.txt

# Oder mit Python
pip install PyPDF2
```

```python
from PyPDF2 import PdfReader

def pdf_to_text(filepath):
    reader = PdfReader(filepath)
    text = ""
    for page in reader.pages:
        text += page.extract_text() + "\n"
    return text
```

### Qualitäts-Checkliste nach Konvertierung

- [ ] Überschriften korrekt übernommen?
- [ ] Tabellen lesbar formatiert?
- [ ] Listen beibehalten?
- [ ] Bilder als Beschreibung ergänzt?
- [ ] Fußnoten/Referenzen übernommen?
- [ ] Sonderzeichen korrekt?

---

## 9 Dateien & Formate richtig aufbereiten

### Projektstruktur für KI-Kontext

```
mein-projekt/
├── .github/
│   └── copilot-instructions.md    ← Projektweite KI-Regeln
├── .copilot/
│   ├── context.md                 ← Architektur & Technologie-Stack
│   ├── conventions.md             ← Code-Konventionen
│   └── glossary.md                ← Fachbegriffe
├── docs/
│   ├── architecture.md            ← Systemarchitektur
│   ├── api.md                     ← API-Dokumentation
│   └── decisions/                 ← Architecture Decision Records
│       └── 001-database-choice.md
├── src/
│   └── ...
└── README.md
```

### copilot-instructions.md – Beispiel

```markdown
# Projekt: Contoso Web-App

## Tech-Stack
- Frontend: React 18 + TypeScript 5
- Backend: .NET 8 Web API
- Datenbank: PostgreSQL 16
- ORM: Entity Framework Core

## Code-Konventionen
- Verwende PascalCase für C#-Klassen und Methoden
- Verwende camelCase für TypeScript-Variablen und Funktionen
- Alle API-Endpoints geben `ApiResponse<T>` zurück
- Fehlerbehandlung über globale Exception-Middleware

## Architektur
- Clean Architecture (Domain → Application → Infrastructure → API)
- CQRS mit MediatR für Commands und Queries
- Repository-Pattern für Datenzugriff

## Regeln
- Schreibe Unit-Tests für alle neuen Services
- Verwende keine `var` in C# – explizite Typen bevorzugen
- Alle DTOs sind `record`-Typen
- API-Versioning über URL-Pfad (/api/v1/)
```

### Kontext-Dateien für Agents

**Für Copilot Studio – Wissensdatenbank strukturieren:**

```markdown
# FAQ – Produkt XY

## Häufige Fragen

### Wie setze ich mein Passwort zurück?
1. Gehe zu https://portal.contoso.com
2. Klicke auf "Passwort vergessen"
3. Gib deine E-Mail-Adresse ein
4. Folge dem Link in der Bestätigungs-E-Mail

### Wie beantrage ich einen neuen Laptop?
1. Öffne ein Ticket im IT-Portal
2. Kategorie: "Hardware-Anfrage"
3. Beschreibe deine Anforderungen
4. Genehmigung durch Vorgesetzten erforderlich
```

---

## 10 Instruction-Files & Custom Instructions

### Ebenen der Konfiguration

```
┌────────────────────────────────────┐
│  1. VS Code Settings (global)       │  → Gilt für alle Projekte
├────────────────────────────────────┤
│  2. .github/copilot-instructions.md │  → Gilt für das Projekt
├────────────────────────────────────┤
│  3. .copilot/*.md                   │  → Kontextdateien pro Thema
├────────────────────────────────────┤
│  4. Inline-Prompt-Kontext           │  → Gilt für die einzelne Anfrage
└────────────────────────────────────┘
```

### VS Code Custom Instructions

In `settings.json`:
```json
{
  "github.copilot.chat.codeGeneration.instructions": [
    { "text": "Verwende immer TypeScript strict mode." },
    { "text": "Bevorzuge funktionale Programmierung." },
    { "file": ".copilot/conventions.md" }
  ]
}
```

### Instruction-File Best Practices

1. **Kurz und präzise** – Jede Regel in einer Zeile
2. **Positiv formulieren** – „Verwende X" statt „Verwende nicht Y"
3. **Priorisieren** – Wichtigste Regeln zuerst
4. **Aktuell halten** – Regelmäßig reviewen und updaten
5. **Team-Konsens** – Alle Teammitglieder einbeziehen

---

## 11 Cheat-Sheet

### Prompt-Vorlagen zum Kopieren

**Code erklären:**
```
Erkläre #selection Schritt für Schritt. Fokus auf:
- Was macht der Code?
- Welche Edge Cases gibt es?
- Wie könnte man es verbessern?
```

**Bug finden:**
```
Analysiere #file auf potenzielle Bugs:
1. Null-Reference-Fehler
2. Race Conditions
3. Fehlende Fehlerbehandlung
4. Speicher-Leaks
```

**Tests schreiben:**
```
Schreibe Unit-Tests für #file:
- Nutze [Jest/xUnit/pytest]
- Teste Happy Path und Error Cases
- Nutze Arrange-Act-Assert Pattern
- Mocke externe Abhängigkeiten
```

**API-Dokumentation:**
```
Erstelle eine OpenAPI 3.0-Dokumentation für #file.
Inkludiere:
- Alle Endpoints mit Methoden
- Request/Response-Schemas
- Beispiel-Payloads
- Fehlercodes
```

**Agent – Neues Feature:**
```
## Ziel
[Feature-Beschreibung]

## Kontext
- Projekt: [Name]
- Tech-Stack: [Technologien]
- Relevante Dateien: #file:... #file:...

## Aufgabe
1. [Schritt 1]
2. [Schritt 2]
3. [Tests schreiben]
4. [Dokumentation aktualisieren]

## Regeln
- Bestehende Architektur einhalten
- Keine Breaking Changes
- Alle Tests müssen grün sein
```

---

## Weiterführende Links

- [GitHub Copilot Docs](https://docs.github.com/en/copilot)
- [Copilot Studio Docs](https://learn.microsoft.com/en-us/microsoft-copilot-studio/)
- [Prompt Engineering Guide](https://www.promptingguide.ai/)
- [Markdown Guide](https://www.markdownguide.org/)
- [Pandoc](https://pandoc.org/)

---

> **Lizenz:** MIT – Frei verwendbar und anpassbar.  
> **Beitragen:** Pull Requests und Issues sind willkommen!
