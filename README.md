# Vokabeltrainer – Go Ahead 5

Vokabeltrainer für das Englischbuch **Go Ahead, Realschule Bayern 2017, 5. Jahrgangsstufe** (Cornelsen).

Läuft als GitHub Page: https://rollendetonne.github.io/Vokabeltrainer/

- **Karteikarten**, **Schreiben** (mit automatischer Prüfung) und **Liste**
- Richtung Englisch → Deutsch oder Deutsch → Englisch
- Auswahl nach Unit und Abschnitt
- Lernstand wird im Browser gespeichert (ein Wort gilt nach 2× richtig in Folge als gelernt)

## Vokabeln

Alle Vokabeln stehen in `vokabeln.json`:

```json
{ "id": 1, "unit": "Unit 1", "section": "Text 1", "page": 14,
  "english": "subject", "german": "(Schul-)Fach", "example": "" }
```

Bisher enthalten: *Let's go!* und *Unit 1* bis Text 2 (Buch S. 191–198).
Neue Vokabeln einfach hinten anhängen – die Auswahl auf der Seite baut sich automatisch aus `unit` und `section` auf.
