# Netzplan-Tool

Eine einzelne HTML-Datei, mit der sich aus einer Excel-Tabelle ein einfacher
Vorgangsknoten-Netzplan (AON) inklusive Vorwärts-/Rückwärtsrechnung und
kritischem Pfad erstellen lässt – läuft vollständig lokal im Browser, es
werden keine Daten irgendwohin hochgeladen.

## Verwendung

1. `index.html` im Browser öffnen (Doppelklick genügt).
2. Über **"Excel-Vorlage herunterladen"** eine `.xlsx`-Vorlage mit den
   richtigen Spalten und einem Beispielprojekt herunterladen.
3. Vorlage in Excel mit den eigenen Vorgängen füllen:

   | Nr. | Vorgang            | Dauer (Tage) | Vorgänger |
   |-----|---------------------|--------------|-----------|
   | A   | Projekt vorbereiten | 2            |           |
   | B   | Planung             | 4            | A         |
   | C   | Bauarbeiten         | 6            | A,B       |

   - **Nr.**: eindeutige Kennung des Vorgangs (Zahl oder Kürzel)
   - **Vorgang**: Bezeichnung/Name des Vorgangs
   - **Dauer (Tage)**: Dauer als Zahl
   - **Vorgänger**: Nr(n). der Vorgänger, mit Komma/Semikolon getrennt; leer
     lassen für Start-Vorgänge

4. Ausgefüllte Datei in das Tool ziehen (oder anklicken zum Auswählen).
5. Vorgänge lassen sich direkt in der Tabelle im Tool noch korrigieren –
   der Netzplan wird automatisch neu berechnet.
6. Diagramm mit Mausrad zoomen, mit gedrückter Maustaste verschieben,
   und über die Buttons als PNG exportieren oder drucken/als PDF speichern.

## Notation

Jeder Vorgang wird als klassischer 5-Felder-Kasten dargestellt:

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

Kritische Vorgänge und der kritische Pfad werden rot hervorgehoben.

## Hinweise

- Das Tool erkennt die Spalten anhand der Kopfzeile automatisch (auch bei
  leicht abweichenden Bezeichnungen wie "ID" oder "Predecessor"). Wird keine
  passende Kopfzeile erkannt, wird die Standardreihenfolge Nr./Vorgang/Dauer/
  Vorgänger angenommen – das Tool weist in diesem Fall per Hinweis darauf hin.
- Unbekannte Vorgänger-Referenzen, doppelte IDs und zyklische Abhängigkeiten
  werden erkannt und als Fehler/Warnung angezeigt.
- Für den Excel-Import/-Export wird die Bibliothek
  [SheetJS](https://sheetjs.com/) von cdnjs.cloudflare.com nachgeladen –
  dafür ist beim Öffnen der Datei eine Internetverbindung nötig. Ohne
  Internetverbindung funktionieren Tabelle, Berechnung und Diagramm trotzdem
  (z. B. über "Beispielprojekt laden" oder manuelles Eintragen), nur der
  Excel-Datei-Import/-Export benötigt die Bibliothek.
