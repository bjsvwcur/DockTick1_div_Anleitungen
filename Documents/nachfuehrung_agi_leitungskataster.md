# Nachführung agi_Leitungskataster

Verantwortlicher: Sandra Curiger
## Beschreibung
Dezentraler Import der kommunalen Leitungskataster in die SO!GIS Datenhaltung.

Für verschiedene Solothurner Gemeinden werden die Daten der Leitungskataster in die SO!GIS Geodaten Infrastruktur (GDI) integriert. Die von verschiedenen Ingenieur-Büros erhoben Daten werden den Gemeinden über den Web GIS Cleint zur Verfügung gestellt. 
### Auslöser
Ingenieur-Büros mit der Nachführung des Leitungskatasterdaten.
### Häufigkeit
ca. 4 Imports jährlich
### Auftrag
`"H:\BJSVW\Agi\GemeindeGIS"`
## Vorgehen Nachführung Schema agi_leitungskataster auf Edit-DB
* itf-File ablegen unter: `G:\sogis\daten_tools\projekte\sogis\gemeindegis\<GEMEINDE>\Interlis\Import\<MEDIUM>\`
* auf `geoutil` in das Verzeichnis `/sogis/daten_tools/skripte/db_schema_definition_edit/agi_leitungskataster/v1` wechseln
  * unter diesem Verzeichnis sind von allen bereits bestehenden Gemeindedaten die ili2pg-Skripte vorhanden (ili2pg_dataimport_<BSF-Nr>_<MEDIUM>.sh)
  * itf-File prüfen mit dem ilivalidator
  ```
    Beispiel Gempen Abwasser:

    java -jar /home/XXXX/ilis/ilivalidator-1.9.3/ilivalidator-1.9.3.jar --log /home/XXXX/log.log /sogis/daten_tools/projekte/sogis/gemeindegis/Gempen/Interlis/Import/abwasser/Interlis-Export-Abwasser-Gempen-191025-a.itf
   ```
  * Wenn die Prüfung fehlerfrei war im entspechende ili2pg-Skript den itf-Filenamen und dbhost anpassen.
  * Skript starten mit:
  ```
    ./ili2pg_dataimport_<BSF-Nr>_<MEDIUM>.sh
  ```
* Wenn der Import erfolgreich war in das Verzeichnis `/sogis/geodata/ch.so.agi.leitungskataster/<GEMEINDE>/<MEDIUM>/` wechseln
  * das "alte" itf-File verschieben in den Unterordner /save (Ordner erstellen wenn noch nicht vorhanden)
  * das "neue" itf-File in dieses Verzeichnis kopieren
  ```
    cp G:\sogis\daten_tools\projekte\sogis\gemeindegis\<GEMEINDE>\Interlis\Import\<MEDIUM>\<FILENAME>.itf .
  ```
 
## Vorgehen Nachführung Schema agi_leitungskataster_pub auf Pub-DB
* Wenn die Daten in der Edit-DB importiert sind, kann die Pub-DB mittels Gretljob nachgeführt werden
* Gretljob ausführen: [agi_leitungskataster](https://github.com/sogis/gretljobs/tree/master/agi_leitungskataster_pub)
 
## Vorgehen Neue Gemeindedaten importieren (Gemeinde nocht nicht vorhanden in der Edit- und Pub-DB)
* Auf der Pub-DB im Schema `agi_leitungskataster_pub` die Tabellen `lk_<MEDIUM>_verfuegbarkeit` anpassen (Wird für die Übersicht im Web GIS Client benötigt)
  * Bei der neuen Gemeinde das Attribut `"verfuegbar"` auf true setzen => Muss bei jedem neuen Medium pro Gemeinde gemacht werden.
* auf `geoutil` in das Verzeichnis `/sogis/daten_tools/skripte/db_schema_definition_edit/agi_leitungskataster/v1` wechseln
  * pro neues Medium ein ili2pg-File erstellen (ein bestehenes von einer anderen Gemeinde kopieren und anpassen)
  * Test und Import Analog "Nachführung Schema agi_leitungskataster auf Edit-DB"
