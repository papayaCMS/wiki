
Um eine Übersetzungsdatei anzulegen, gehen Sie wie folgt vor:

1.  Legen Sie eine neue Datei an, in die Sie die Übersetzungen einfügen möchten. Der Dateiname sollte dem Schema folgen, das in [Namensschema für Übersetzungsdateien](Namensschema_für_Übersetzungsdateien.md) beschrieben ist. Ferner sollte die Datei im selben Verzeichnis gespeichert werden, in dem sich auch die Stylesheetdatei befindet.
2.  Fügen Sie in die leere Datei eine XML-Deklaration ein: **XML-Deklaration einfügen**
    ~~~~ {.xml}
    <?xml version="1.0"?>
    ~~~~

3.  Fügen Sie das Dokumentenelement ein: **Dokumentenelement einfügen**
    ~~~~ {.xml}
    <?xml version="1.0"?>
    <texts>
    </texts>
    ~~~~

4.  Fügen Sie für jede Phrase, die Sie übersetzen möchten, ein `<text>` -Element mit der Übersetzung ein. Die Phrase selbst muss im Attribut ident stehen: **Übersetzungen für Phrasen einfügen**
    ~~~~ {.xml}
    <?xml version="1.0"?>
    <texts>
      <text ident="MORE">mehr</text>
      <text ident="SAVE">Speichern</text>
      <text ident="CANCEL">Abbrechen</text>
      <text ident="BACK">Zurück</text>
      ...
    </texts>
    ~~~~

5.  Speichern Sie die Datei ab. Der Dateiname sollte dem Schema folgen, das in [Namensschema für Übersetzungsdateien](Namensschema_für_Übersetzungsdateien.md) beschrieben ist.

[Kategorie:Mit Übersetzungsdateien arbeiten](../export_de/Kategorie:Mit_Übersetzungsdateien_arbeiten.md)