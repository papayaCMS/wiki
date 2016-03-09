---
title: export_de/Kategorie.md:Print-Templates schreiben
permalink: export_de/Kategorie.md:Print-Templates_schreiben/
---

Grundsätzlich ist es möglich, dass Sie im Header Ihrer HTML-Seiten eine CSS-Datei einbinden, die ein angepasstes CSS für die Druckausgabe enthält. Wenn Sie im `medium` -Attribut des `<link>` -Elements den Wert „print“ einfügen, werden die Angaben der eingebundenen CSS-Datei benutzt. Dies ist in der Druckvorschau der Seite erkennbar.

Allerdings hat diese rein CSS-basierte Lösung einige Probleme. Nutzer erwarten beispielsweise einen Link, mit dem sie eine Druckansicht im Browserfenster öffnen können, ohne dass umständlich eine Druckvorschau des Browsers geöffnet werden muss. Man sollte zudem auch nicht davon ausgehen, dass alle Nutzer die Vorschaufunktion ihres Browsers kennen.

Die Ausgabe der Druckansicht sollte zudem mit der Druckvorschau des Browsers identisch sein. Eine solche Druckansicht ist freilich nur möglich, wenn man eine separate Druckausgabe mit einem Template erzeugt.

Für die Druckausgabe können Sie zudem auf die Ausgabe der Boxen für die Navigation und für die rechte Kontextspalte verzichten. Da diese Boxen für die Druckausgabe nicht gebraucht werden, müssen Sie sie nicht gesondert über CSS-Deklarationen unsichtbar stellen.

Wenn Sie Print-Templates erstellen, gehen Sie im Prinzip exakt so vor wie bei normalen Webseiten:

1.  In der Planungsphase wird das Webseitenkonzept erstellt. Sie legen fest, für welche Seitenmodule Sie ein Print-Template benötigen.
2.  In der Implementierungsphase erstellen Sie die Webseitenvorlage. Für die ausgewählten Seitenmodule erstellen Sie zusätzlich noch die Print-Templates.
3.  Sie installieren die Webseitenvorlage. Erstellen Sie einen Ausgabefilter für die druckfreundliche Seitenausgabe.

Sowohl die Implementierungs- als auch die Installationsphase unterscheidet sich bei den Print-Templates von denen der Webseitenvorlage. Im Unterschied zu normalen Webseitenvorlagen müssen Sie Print-Templates so schreiben, dass eine druckfreundliche Ausgabe erzeugt wird. Wenn Sie Print-Templates installieren, müssen Sie auch einen anderen Ausgabefilter anlegen, damit die alternative, druckfreundliche Ausgabe dargestellt werden kann.

Druckfreundliche Seitenausgabe erzeugen
---------------------------------------

Um eine druckfreundliche Seitenausgabe zu erzeugen, müssen Sie folgende Punkte beachten:

1.  Sie binden in der druckfreundlichen Seitenausgabe ein anderes CSS ein, siehe [Druckfreundliches CSS einbinden](/Druckfreundliches_CSS_einbinden ).
2.  Blenden Sie Boxen in der druckfreundlichen Seitenausgabe aus, siehe [Boxenausgabe unterdrücken](/Boxenausgabe_unterdrücken ).

[export_de/Kategorie.md:Print-Templates erstellen](export_de/Kategorie.md:Print-Templates_erstellen )