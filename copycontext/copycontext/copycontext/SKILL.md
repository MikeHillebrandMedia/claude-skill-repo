---
name: copycontext
description: >
  Überträgt den aktuellen Chat-Kontext in einen neuen Chat. Fasst den bisherigen
  Gesprächsverlauf zusammen, kopiert ihn in die Zwischenablage und öffnet automatisch
  einen neuen Chat in Cowork, wo der Kontext eingefügt wird. IMMER nutzen wenn der
  User /copycontext eingibt, oder sagt: "copy context", "kontext übertragen",
  "neuer chat mit kontext", "context kopieren", "chat übertragen", "context transfer",
  "kontext in neuen chat", "chat neu starten mit kontext", "context rüberretten".
---

# CopyContext Skill

Dieser Skill überträgt den laufenden Chat-Kontext nahtlos in einen neuen Chat —
damit der Context-Window nicht voll läuft, aber alle wichtigen Infos erhalten bleiben.

## Ablauf (in dieser Reihenfolge)

### 1. Zusammenfassung erstellen

Fasse den bisherigen Chat präzise zusammen. Max. 500 Wörter. Ziel: Ein frischer
Claude-Chat soll sofort wissen, worum es geht und wo wir stehen — ohne den
alten Chat zu kennen.

Verwende dieses Format:

```
## Kontext-Übergabe – [heutiges Datum]

**Projekt / System:** [Name, URL, Tech-Stack]

### Was wurde gemacht
[Konkrete abgeschlossene Aufgaben — nicht allgemein, sondern spezifisch]

### Wichtige Erkenntnisse
[Dinge die wir rausgefunden haben, die ein neuer Chat kennen muss]

### Offene Punkte / Nächste Schritte
[Was noch aussteht oder als nächstes kommt]

### Technische Details (nur wenn relevant)
[Pfade, Zugangsdaten, Konfigurationen, Fehlermuster, etc.]
```

Kein Blabla, keine Einleitung à la "In diesem Chat haben wir...".
Direkt starten mit den Fakten.

### 2. Clipboard befüllen

Schreibe die fertige Zusammenfassung mit dem write_clipboard Computer-Use-Tool
in die Zwischenablage.

Tool: mcp__computer-use__write_clipboard

### 3. Neuen Chat öffnen (Computer-Use)

Lade die Computer-Use-Tools via ToolSearch (query: "computer-use", max_results: 30).

Dann:
1. request_access für die Cowork / Claude App
2. Screenshot machen — Cowork-Fenster identifizieren
3. "Neuer Chat"-Button suchen und klicken (meist oben links, Stift-Icon oder "+")
4. Warten bis das leere Chat-Eingabefeld erscheint
5. In das Eingabefeld klicken
6. key mit cmd+v — Zusammenfassung wird eingefügt
7. NICHT absenden — der User soll den Text sehen und selbst entscheiden

### 4. Abschluss

Sage dem User:
"Kontext ist im neuen Chat eingefügt. Du kannst ihn anpassen und dann abschicken."

## Was in die Zusammenfassung gehört

Immer rein:
- Aktuelles Projekt, System, URL
- Erledigte Aufgaben (konkret)
- Erkenntnisse die ein neuer Chat braucht
- Offene Punkte

Nur wenn vorhanden:
- FTP/SSH/Admin-Zugangsdaten (nur wenn aktiv genutzt)
- Datei-Pfade die wichtig sind
- Bekannte Bugs / Workarounds
- Spezifische Konfigurationen

Nie rein:
- Smalltalk über den Chat
- Bereits gelöste, irrelevante Dinge
- Wiederholungen

## Fehlerfall: Computer-Use klappt nicht

1. Zusammenfassung trotzdem in Clipboard schreiben
2. User: "Zusammenfassung ist in der Zwischenablage. Neuen Chat öffnen und Cmd+V drücken."
3. Zusammenfassung als Text-Fallback im aktuellen Chat ausgeben
