
Um das RSS-Template zu installieren, gehen Sie wie folgt vor:

1.  Kopieren Sie die Stylesheet-Datei in das Verzeichnis, in dem sich die Webseitenvorlagen befinden. Im Falle des Demotemplates ist dies `/papaya-data/templates/standard-html/xslt/`.
2.  Legen Sie einen Ausgabefilter mit dem Filtermodul „XSLT Output Filter“ und der Dateiendung `.rss` an. Geben Sie als Content-Type „text/xml“ an.
3.  Verknüpfen Sie die Ansicht mit dem Stylesheet, dass Sie in [Kategorie:Template für RSS-Feed erstellen](Export_de/Kategorie:Template_für_RSS-Feed_erstellen.md) geschrieben haben.

Sie können das RSS-Template freilich nur mit den Ansichten sinnvoll einsetzen, die für die Seitenmodule „Kategorie with image“ und „Tag export_de/Kategorie“ erstellt worden sind. Diese Seitenmodule stellen Übersichtsseiten dar, die andere Seiten in Form einer Liste anteasern. Sie lassen sich so konfigurieren, dass nur die neuesten Seiten angezeigt werden. Zudem können Sie die Anzahl der Seiten, die angeteasert werden sollen, auf eine bestimmte Zahl beschränken. Diese Eigenschaft der genannten Übersichtsseiten entsprechen in Funktion und Nutzen den RSS-Feeds, die ebenso nur die neuesten Seiten als Links enthalten.

Näheres zum Modul „Kategorie with image“ erfahren Sie im Benutzerhandbuch für papaya CMS in Kapitel 5.2.1. Näheres zum Modul „Tag export_de/Kategorie“ können Sie im genannten Handbuch in Kapitel 7.2.2 erfahren.

[Kategorie:RSS-Feeds erstellen](../export_de/Kategorie:RSS-Feeds_erstellen.md)
