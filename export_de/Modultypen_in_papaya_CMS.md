
Die folgende Tabelle enthält eine Übersicht aller Modultypen von papaya CMS:

|Modultyp|Identifier|Dateinamenspräfix|Beschreibung|
|Seiten|page|content_|Seiten sind das Fundament, auf denen alle Ausgaben basieren.|
|Boxen|box|actbox_|Häufig wiederkehrende Inhalte, die auf unterschiedlichen Seiten auftreten, werden als Boxen umgesetzt.|
|Cronjobs|cronjob|cronjob_|Mit Cronjobs lassen sich regelmäßige Aufgaben durchführen, die nicht an Seitenaufrufe gekoppelt sein sollen oder dürfen.|
|Zeiten|time|time_|Zeitmodule dienen der Berechnung von Zeitintervallen oder eines bestimmten Zeitpunktes in Form eines Datums. Sie werden sowohl im Calendar-Paket als auch von Cronjob-Modulen genutzt.|
|Statistic|statistic|statistic_|Statistikmodule (kommerziell) geben Einblick in die Nutzung einer Website und helfen, sie zu optimieren.|
|Importfilter|import|import_|Über Importfilter lassen sich theoretisch beliebige Daten in papaya CMS importieren.|
|Ausgabefilter|output|output_|Die Ausgabe der Module wird meistens über XSLT weiterverarbeitet oder über einen Ausgabefilter, der PDF erstellt.|
|Datenfilter|datafilter|datafilter_|Datenfilter können verwendet werden um Inhalte vor der Ausgabe nochmals zu verändern. Beispielsweise können mit dem Glossar-Paket (kommerziell) Fachwörter in Artikeln mit Einträgen aus dem Glossar verlinkt werden.|
|Administration|admin|edmodule_ admin_|Die meisten Pakete von papaya CMS bringen eine eigene Administrationsoberfläche mit, über die Module konfiguriert werden können und Inhalte überwacht oder erstellt werden.|
|dynamische Bilder|image|image_|Dank GD können Buttons, Fortschrittsbalken und Ranking als Grafiken dynamisch generiert werden.|
|Aliase|alias|alias_|Aliase sind URLs, die auf eine andere URL verweisen. Meistens, damit die URL schöner aussieht oder sich besser merken lässt. Durch Module kann die Funktionsweise von Aliasen umfangreich angepasst werden.|
|Konnektoren|connector|connector_|Wenn unterschiedliche Module miteinander interagieren sollen, muss man eine klare Schnittstelle definieren. Das geschieht über so genannte Konnektoren.|
|Parser|parser|parser_|Ein Parser-Modul ermöglicht es eigene XML-Tags zu verwenden, die von dem Modul entsprechend ausgewertet werden. Über den Richtext-Editor können diese Inhalte bequem eingefügt werden.|

Im normalen Arbeitsalltag kommen vorwiegend Content-, Box- und Adminmodule zum Einsatz. Bei großen Projekten mit verschiedenen Paketen spielen auch Konnektoren und Cronjobs eine größere Rolle.

[Kategorie:Der modulare Aufbau von papaya CMS](export_de/Kategorie:Der_modulare_Aufbau_von_papaya_CMS.md)