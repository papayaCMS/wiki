
Wenn Ihr Paket eigene Datenbanktabellen benutzt, müssen Sie für jede Tabelle eine XML-Datei mit einer Strukturbeschreibung anlegen und im Unterverzeichnis DATA in Ihrem Paket speichern. Diese XML-Strukturdateien müssen Sie in die `modules.xml` eintragen, damit die Modulverwaltung die Tabellendaten nutzen kann. Wenn das Paket auf eine andere papaya-Installation installiert wird, kann papaya CMS anhand der Tabellenstrukturbeschreibungen die notwendigen Tabellen für das Paket erzeugen.

Um die Tabellendokumente in der `modules.xml` zu registrieren, gehen Sie bitte wie folgt vor:

1.  Öffnen Sie die `modules.xml` mit einem Texteditor.
2.  Fügen Sie, sofern noch nicht vorhanden, unterhalb des `<modules>` -Elements das Element `<tables>` ein: **<tables>-Element einfügen**
    ~~~~ {.xml}
    <?xml version="1.0"?>
    <modulegroup>
      <name>Beispielpaket</name>
      <description>Dieses Paket macht dies und das.</description>
      <modules>
        <!-- Weitere Module können folgen -->
      </modules>
      <tables>
      </tables>
    </modulegroup>
    ~~~~

3.  Fügen Sie für jede Tabelle ein `<table>` -Element ein. Der Name der Tabelle wird dabei im Attribut `name` angegeben, wobei der Tabellenpräfix ausgelassen wird: '''Tabellen in
    <table>
    -Elemente eintragen'''

    ~~~~ {.xml}
    <?xml version="1.0"?>
    <modulegroup>
      <name>Beispielpaket</name>
      <description>Dieses Paket macht dies und das.</description>
      <modules>
        <!-- Weitere Module können folgen -->
      </modules>
      <tables>
        <table name="example_basicstuff" />
        <table name="example_otherstuff" />
      </tables>
    </modulegroup>
    ~~~~

4.  Speichern Sie die Datei ab. Das Paket kann nun im Backend von papaya CMS in der Modulverwaltung registriert werden.

Die XML-Dateien mit den Tabellenstrukturen werden im Verzeichnis `./DATA/` gespeichert, wobei der Dateiname das Präfix `table_` erhält.

[Kategorie:Verzeichnisse und Metadaten für Pakete erstellen](export_de/Kategorie:Verzeichnisse_und_Metadaten_fuer_Pakete_erstellen.md)
