# Testprotokoll LehrerPlan · Gruppe 10-14-16

**Testdatum:** __.__.2026 · **Getesteter Commit:** `_______` (Kurz-Hash von `git rev-parse --short HEAD` nach dem Commit der Logs) · **Unity-Version:** 6000.3.24f1 · **Szene:** `Assets/Scenes/SampleScene.unity`

> Der getestete Commit ergänzt `Debug.Log`-Ausgaben in `Assets/Scripts/LehrerSuche.cs`:
> `GET Start: <URL>` direkt vor jedem Request, `GET Ende: <URL> -> HTTP <Code> (<Ergebnis>)` danach
> und `Raum <Nr>: <n> Lehrer gefunden` nach dem Durchsuchen der Übersicht.
> Die Konsolentests (3, 4, 6, 7) lesen diese Meldungen in der Unity Console ab.

| Nr. | Durchführung | Soll | Ist | Status |
|---|---|---|---|---|
| 1 | Play Mode gestartet, gewartet bis `GET Ende`-Meldungen fertig sind, mit dem Spieler zur Tür **B___** gegangen. Sprechstunde von **___** am Schild mit der Detailseite `https://www.htl-salzburg.ac.at/lehrerinnen-details/___.html` verglichen. | Tag und Uhrzeit stimmen überein. | Schild: „Sprechstunde: ___“ · Website: „___“ | ___ |
| 2 | Räume **B___** und **B___** (unterschiedliche Lehrkräfte) besucht, Raumnummer und Namen am Schild mit `https://www.htl-salzburg.ac.at/lehrerinnen.html` verglichen. | Raumnummer und Personen stimmen für beide Räume. | B___: Schild „___“ / Website „___“ · B___: Schild „___“ / Website „___“ | ___ |
| 3 | Unity frisch gestartet, Console geleert, Play Mode gestartet. Lehrkraft **___** in Raum **B___** gewählt und in der Console den Eintrag `GET Ende: https://www.htl-salzburg.ac.at/lehrerinnen-details/___.html` gesucht. | Für diese URL erscheint `HTTP 200 (Success)`. | `GET Ende: …/lehrerinnen-details/___.html -> HTTP ___ (___)` | ___ |
| 4 | Unity frisch gestartet, Console geleert, Play Mode gestartet. In der Console den Abruf von `https://www.htl-salzburg.ac.at/lehrerinnen.html` beim Start geprüft (Übersicht, keine Detailseite). | Für diese URL erscheint `HTTP 200 (Success)`. | `GET Ende: https://www.htl-salzburg.ac.at/lehrerinnen.html -> HTTP ___ (___)` | ___ |
| 5 | Tür **B___** besucht, Name und Sprechstunde von **___** am Schild mit der Detailseite verglichen; Position geprüft (Raum oben, Name fett, darunter Sprechstunde und Telefon). | Die Angaben stimmen überein und stehen am vorgesehenen Platz. | Schild: „___“ · Website: „___“ | ___ |
| 6 | Play Mode gestartet, gewartet bis alle Räume geladen sind (letzte `GET Ende`-Meldung). Anzahl `GET Start`-Meldungen notiert (___). Zur Tür **B___** gegangen, weggegangen, erneut hingegangen, danach erneut gezählt. | Keine neue Anfrage. Das Schild zeigt die geladenen Angaben. | Vorher ___ / nachher ___ `GET Start`-Meldungen · Schild: „___“ | ___ |
| 7 | Bei Tür **B___** im Inspector (`DoorInfo` → `Lehrer Namen`) den Ersatztext „___“ eingetragen. Internet getrennt, Unity frisch gestartet, Play Mode gestartet, zur Tür gegangen. | Nach dem Abruffehler erscheint der Ersatztext. | Console: `GET Ende: …/lehrerinnen.html -> HTTP ___ (___) Fehler: ___` · Schild: „___“ | ___ |

**Status:** **Bestanden**, **Fehlgeschlagen** oder **Nicht ausführbar** (mit Grund).

## Hinweise aus der Code-Analyse

- **Test 6:** `LehrerDatenLader` lädt alle Türen einmal beim Start. `DoorInfo` zeigt beim Betreten der Tür nur den gespeicherten Text an und startet keinen Request. Nach dem Laden sollten also keine neuen `GET Start`-Meldungen auftauchen.
- **Test 7:** Ist die Übersicht nicht erreichbar, liefert `LehrerSuche` `null`, und `LehrerDatenLader` setzt den im Inspector eingetragenen Text wieder ein. In der Szene ist bei keiner der 13 Türen (B201–B213) ein Ersatztext eingetragen. Ohne diesen erscheint „Keine Lehrkräfte gefunden“. Weil die Übersicht nach einem Fehler nicht gespeichert wird, versucht das Skript `lehrerinnen.html` für jede Tür erneut zu laden, also erscheinen 13 Fehlermeldungen.
- Den Ersatztext für Test 7 nach dem Test wieder entfernen (oder die Szene nicht speichern), damit der getestete Stand gleich bleibt.
