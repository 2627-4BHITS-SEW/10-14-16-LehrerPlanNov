# Testprotokoll LehrerPlan · Gruppe 10-14-16

**Testdatum:** 23.11.2026 · **Getesteter Commit:** `a102c01` · **Unity-Version:** 6000.3.24f1 · **Szene:** `Assets/Scenes/SampleScene.unity`

> Der getestete Commit ergänzt `Debug.Log`-Ausgaben in `Assets/Scripts/LehrerSuche.cs`:
> `GET Start: <URL>` direkt vor jedem Request, `GET Ende: <URL> -> HTTP <Code> (<Ergebnis>)` danach
> und `Raum <Nr>: <n> Lehrer gefunden` nach dem Durchsuchen der Übersicht.
> Die Konsolentests (3, 4, 6, 7) lesen diese Meldungen in der Unity Console ab.

| Nr. | Durchführung | Soll | Ist | Status |
|---|---|---|---|---|
| 1 | Play Mode gestartet, gewartet bis `GET Ende`-Meldungen fertig sind, mit dem Spieler zur Tür **B210** gegangen. Sprechstunde von **Eitzinger Martin, Prof. B.Rel.Ed.Univ. BA BA** am Schild mit der Detailseite `https://www.htl-salzburg.ac.at/lehrerinnen-details/eitzinger-martin-prof-b-rel-ed-univ-ba-ba-b-210.html` verglichen. | Tag und Uhrzeit stimmen überein. | Schild: „Sprechstunde: Mittwoch 09:45-10:35 Uhr“ · Website: „Mittwoch 09:45-10:35 Uhr“ | Bestanden |
| 2 | Räume **B212** und **B211** (unterschiedliche Lehrkräfte) besucht, Raumnummer und Namen am Schild mit `https://www.htl-salzburg.ac.at/lehrerinnen.html` verglichen. | Raumnummer und Personen stimmen für beide Räume. | B212: Schild „Neuhuber Wolfgang, AV Prof. Mag.“ / Website „…/neuhuber-wolfgang-av-prof-mag-b-212.html → „Neuhuber Wolfgang, AV Prof. Mag.““ · B211: Schild „Holzer Roland, Prof. Dipl.-Ing.(FH)“ / Website „…/holzer-roland-prof-dipl-ing-fh-b-211.html → „Holzer Roland, Prof. Dipl.-Ing.(FH)““ | Bestanden |
| 3 | Unity frisch gestartet, Console geleert, Play Mode gestartet. Lehrkraft **Eitzinger Martin, Prof. B.Rel.Ed.Univ. BA BA** in Raum **B210** gewählt und in der Console den Eintrag `GET Ende: https://www.htl-salzburg.ac.at/lehrerinnen-details/eitzinger-martin-prof-b-rel-ed-univ-ba-ba-b-210.html` gesucht. | Für diese URL erscheint `HTTP 200 (Success)`. | `GET Ende: https://www.htl-salzburg.ac.at/lehrerinnen-details/eitzinger-martin-prof-b-rel-ed-univ-ba-ba-b-210.html -> HTTP 200 (Success)` | Bestanden |
| 4 | Unity frisch gestartet, Console geleert, Play Mode gestartet. In der Console den Abruf von `https://www.htl-salzburg.ac.at/lehrerinnen.html` beim Start geprüft (Übersicht, keine Detailseite). | Für diese URL erscheint `HTTP 200 (Success)`. | `GET Ende: https://www.htl-salzburg.ac.at/lehrerinnen.html -> HTTP 200 (Success)` | Bestanden |
| 5 | Tür **B209** besucht, Name und Sprechstunde von **Dantendorfer Simon, Prof. BSc.** am Schild mit der Detailseite verglichen; Position geprüft (Raum oben, Name fett, darunter Sprechstunde und Telefon). | Die Angaben stimmen überein und stehen am vorgesehenen Platz. | Schild: „Raum B209“, darunter fett „Dantendorfer Simon, Prof. BSc.“, darunter „Sprechstunde: Donnerstag 09:45-10:35 Uhr“ und „Telefon: 321“ · Website: „Dantendorfer Simon, Prof. BSc.“, „Tel-DW: 321“, „Sprechstunde: Donnerstag 09:45-10:35 Uhr“. Reihenfolge weicht von der Website ab, entspricht aber dem vorgesehenen Aufbau des Schilds. | Bestanden |
| 6 | Play Mode gestartet, gewartet bis alle Räume geladen sind (letzte `GET Ende`-Meldung). Anzahl `GET Start`-Meldungen notiert (15). Zur Tür **B212** gegangen, weggegangen, erneut hingegangen, danach erneut gezählt. | Keine neue Anfrage. Das Schild zeigt die geladenen Angaben. | Vorher 15 / nachher 15 `GET Start`-Meldungen · Schild: „Raum B212 – Neuhuber Wolfgang, AV Prof. Mag., Sprechstunde: Donnerstag 17:10 - 18:00 Uhr, Telefon: 800“ | Bestanden |
| 7 | Bei Tür **B210** im Inspector (`DoorInfo` → `Lehrer Namen`) den Ersatztext „ERSATZTEXT B210 - offline“ eingetragen. Internet getrennt, Unity frisch gestartet, Play Mode gestartet, zur Tür gegangen. | Nach dem Abruffehler erscheint der Ersatztext. | Console: `GET Ende: https://www.htl-salzburg.ac.at/lehrerinnen.html -> HTTP 0 (ConnectionError) Fehler: Cannot resolve destination` · Schild: „ERSATZTEXT B210 - offline“ | Bestanden |

**Status:** **Bestanden**, **Fehlgeschlagen** oder **Nicht ausführbar** (mit Grund).

## Hinweise aus der Code-Analyse

- **Test 6:** `LehrerDatenLader` lädt alle Türen einmal beim Start. `DoorInfo` zeigt beim Betreten der Tür nur den gespeicherten Text an und startet keinen Request. Nach dem Laden sollten also keine neuen `GET Start`-Meldungen auftauchen.
- **Test 7:** Ist die Übersicht nicht erreichbar, liefert `LehrerSuche` `null`, und `LehrerDatenLader` setzt den im Inspector eingetragenen Text wieder ein. In der Szene ist bei keiner der 13 Türen (B201–B213) ein Ersatztext eingetragen. Ohne diesen erscheint „Keine Lehrkräfte gefunden“. Weil die Übersicht nach einem Fehler nicht gespeichert wird, versucht das Skript `lehrerinnen.html` für jede Tür erneut zu laden, also erscheinen 13 Fehlermeldungen.
- Den Ersatztext für Test 7 nach dem Test wieder entfernen (oder die Szene nicht speichern), damit der getestete Stand gleich bleibt.
