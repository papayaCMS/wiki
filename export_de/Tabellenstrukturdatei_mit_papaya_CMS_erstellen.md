
Wenn Sie eine Tabellenstrukturdatei erstellen möchten, müssen Sie diese nicht von Hand schreiben. Um eine neue Tabelle zu einem Modulpaket hinzuzufügen, gehen Sie wie folgt vor:

1.  Legen Sie die Tabelle mit allen benötigten Feldern in der Datenbank an. Denken Sie dabei auch an die Indices.
2.  Fügen Sie den Namen der Tabelle in die `modules.xml` ein, siehe [Tabellen in modules.xml referenzieren](Tabellen_in_modules.xml_referenzieren.md).
3.  Führen Sie in der Modulverwaltung von papaya CMS den Modulscan aus. Die referenzierten Tabellen werden im Abschnitt "Paketinhalt" unter "Tabellen" aufgeführt.

In der Modulverwaltung von papaya CMS ist die Tabelle im Tabellenbereich des Pakets sichtbar. Zusätzlich wird ein Infodialog angezeigt, der auf die fehlende Strukturdatei hinweist. Sie können die Tabellenstruktur über die papaya-Funktion Tabelle exportieren in der Modulverwaltung als XML-Datei speichern. Gehen Sie dazu wie folgt vor:

1.  Klicken Sie in der Modulverwaltung auf die Tabelle, deren Strukturinformationen Sie als XML-Datei speichern möchten.
2.  Klicken Sie im Bearbeitungsmenü auf Tabelle exportieren . Ein Downloaddialog wird angezeigt. Die Datei wird mit folgendem Dateinamensformat auf Ihrem Rechner gespeichert: **Dateinamensformat der heruntergeladenen Tabellenstrukturdatei**
        <nowiki>table_<tabellenname>_<Datum>.xml</nowiki>

3.  Kopieren Sie die Datei nun auf den Server in das Unterverzeichnis `./DATA/` Ihres Paketverzeichnisses. Ändern Sie dabei den Dateinamen in das folgende Namensformat um, indem Sie das Datum aus dem Namen entfernen. **Namensformat der Tabellenstrukturdateien**
        <nowiki>table_<tabellenname>.xml</nowiki>

Wenn Sie nun in der Modulverwaltung die Tabelle anklicken, wird der Infodialog mit der Warnung über die fehlende Tabellenstrukturdatei nicht mehr angezeigt.

Näheres zur Modulverwaltung erfahren Sie in „papaya CMS 5: Handbuch für Administratoren“, Kapitel 17.

[Kategorie:Verzeichnisse und Metadaten für Pakete erstellen](export_de/Kategorie:Verzeichnisse_und_Metadaten_fuer_Pakete_erstellen.md)
