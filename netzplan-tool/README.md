# Netzplan-Tool

Eine einzelne HTML-Datei, mit der sich aus einer Excel-Tabelle ein einfacher
Vorgangsknoten-Netzplan (AON) inklusive Vorwärts-/Rückwärtsrechnung und
kritischem Pfad erstellen lässt – über mehrere Projekte hinweg, mit
projektübergreifenden Abhängigkeiten. Läuft vollständig lokal im Browser, es
werden keine Daten irgendwohin hochgeladen.

## Verwendung

1. `index.html` im Browser öffnen (Doppelklick genügt).
2. Über **"Excel-Vorlage herunterladen"** eine `.xlsx`-Vorlage mit den
   richtigen Spalten und einem Beispielprojekt herunterladen.
3. Vorlage in Excel mit den eigenen Projekten und Lieferergebnissen füllen:

   | Nr. | Projekt        | Lieferergebnis          | Dauer (Tage) | Vorgänger |
   |-----|----------------|--------------------------|--------------|-----------|
   | IA  | Infrastruktur  | Netzwerk aufbauen        | 3            |           |
   | IB  | Infrastruktur  | Server bereitstellen     | 4            | IA        |
   | SA  | Software       | Architektur definieren   | 3            |           |
   | SB  | Software       | Entwicklung              | 8            | SA,IB     |

   - **Nr.**: eindeutige Kennung des Lieferergebnisses (Zahl oder Kürzel) –
     über **alle** Projekte hinweg eindeutig, da Vorgänger projektübergreifend
     referenziert werden können
   - **Projekt**: Name des Projekts, zu dem das Lieferergebnis gehört
   - **Lieferergebnis**: Bezeichnung/Name des Lieferergebnisses (entspricht
     dem klassischen "Vorgang" der Netzplantechnik)
   - **Dauer (Tage)**: Dauer als Zahl
   - **Vorgänger**: Nr(n). der Vorgänger, mit Komma/Semikolon getrennt; leer
     lassen für Start-Lieferergebnisse. Ein Vorgänger darf aus einem
     **anderen** Projekt stammen – genau so werden Abhängigkeiten zwischen
     Projekten abgebildet (z. B. Zeile SB oben hängt von IB aus dem Projekt
     "Infrastruktur" ab).

4. Ausgefüllte Datei in das Tool ziehen (oder anklicken zum Auswählen).
5. Lieferergebnisse lassen sich direkt in der Tabelle im Tool noch
   korrigieren – der Netzplan wird automatisch neu berechnet.
6. Diagramm mit Mausrad zoomen, mit gedrückter Maustaste verschieben,
   und über die Buttons als PNG exportieren oder drucken/als PDF speichern.

## Mehrere Projekte & projektübergreifende Abhängigkeiten

Sobald mehr als ein Projekt vorkommt, zeichnet das Tool das Diagramm als
**Schwimmbahnen** (eine horizontale Bahn je Projekt, farblich hinterlegt).
Jedes Lieferergebnis bekommt zusätzlich einen farbigen Streifen am linken
Rand seines Kastens entsprechend der Projektfarbe. Abhängigkeiten, die eine
Projektgrenze überschreiten, werden als **gestrichelte Pfeile** gezeichnet.

Darunter listet die Karte **"Projekt-Abhängigkeiten"** automatisch auf,
welches Projekt über welches Lieferergebnis von welchem anderen Projekt
abhängt (z. B. "Infrastruktur → Software, über IB: Server bereitstellen →
SB: Entwicklung"). Hängen zwei Projekte wechselseitig voneinander ab, weist
das Tool zusätzlich per Hinweis darauf hin.

Bei nur einem Projekt (oder wenn die Projekt-Spalte leer bleibt) verhält
sich das Tool wie zuvor: ein einzelnes, flaches Diagramm ohne Schwimmbahnen.

## Notation

Jedes Lieferergebnis wird als klassischer 5-Felder-Kasten dargestellt:

```
| FAZ | Dauer | FEZ |
|   ID: Vorgangsname |
| SAZ |  GP   | SEZ |
```

- **FAZ** – Frühester Anfangszeitpunkt
- **FEZ** – Frühester Endzeitpunkt
- **SAZ** – Spätester Anfangszeitpunkt
- **SEZ** – Spätester Endzeitpunkt
- **GP** – Gesamtpuffer (0 = kritischer Vorgang)

Kritische Lieferergebnisse und der kritische Pfad werden rot hervorgehoben.

## Hinweise

- Das Tool erkennt die Spalten anhand der Kopfzeile automatisch (auch bei
  leicht abweichenden Bezeichnungen wie "ID"/"Predecessor" oder
  "Lieferobjekt"). Wird keine passende Kopfzeile erkannt, wird die
  Standardreihenfolge Nr./Projekt/Lieferergebnis/Dauer/Vorgänger angenommen –
  das Tool weist in diesem Fall per Hinweis darauf hin.
- Unbekannte Vorgänger-Referenzen, doppelte IDs und zyklische Abhängigkeiten
  werden erkannt und als Fehler/Warnung angezeigt (auch projektübergreifend).
- Für den Excel-Import/-Export wird die Bibliothek
  [SheetJS](https://sheetjs.com/) von cdnjs.cloudflare.com nachgeladen –
  dafür ist beim Öffnen der Datei eine Internetverbindung nötig. Ohne
  Internetverbindung funktionieren Tabelle, Berechnung und Diagramm trotzdem
  (z. B. über "Beispielprojekt laden" oder manuelles Eintragen), nur der
  Excel-Datei-Import/-Export benötigt die Bibliothek.
