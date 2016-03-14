---
title: Notifying Search Engines
permalink: /Notifying_Search_Engines/
---

Many search engines provide public services URLs to which web site owners can send data in "Google Sitemaps XML" format to inform these search engines about the changed structure of their web sites. Using the "Action Dispatcher" and "Pages Connector" modules, you can do this automatically while publishing a page. This requires three main steps, detailed below:

1.  Create a sitemap page using the Google Sitemaps output filter
2.  Make settings in the Pages Connector
3.  Configure the Action Dispatcher

Sobald die Einstellungen korrekt vorgenommen wurden, werden bei jeder Seiten-Veröffentlichung automatisch Sitemaps für die jeweils veröffentlichten Sprachversionen an die gewünschten Suchmaschinen gesandt. Voraussetzung ist, dass das Feld "Veröffentlicht bis" entweder leer bleibt oder eine Zeitangabe enthält, die sowohl in der Zukunft als auch nach der Zeitangabe für "Veröffentlicht von" liegt. Beachten Sie, dass der Veröffentlichungsvorgang etwas langsamer wird, wenn die automatische Benachrichtigung eingeschaltet ist. Das Ergebnis sehen Sie im Protokoll von papaya CMS, wo Einträge in der Form "Sent <Anzahl> sitemap pings out of a total of <Anzahl>" angezeigt werden.

Create a sitemap page using the Google Sitemaps output filter
-------------------------------------------------------------

![thumb|border|right|300px|Die Sitemap-Ansicht im Bereich "Ansichten"](images/Ansicht_sitemap.png) Als Erstes benötigen Sie eine Sitemap-Seite, die mit Hilfe des passenden Templates eine Ausgabe im Format "Google Sitemaps XML" erzeugt. Überprüfen Sie dazu, ob Sie bereits eine Sitemap-Ansicht konfiguriert haben. Dies funktioniert folgendermaßen:

1.  Klicken Sie in der Symbolleiste des papaya-Backends auf "Ansichten" in der Gruppe "Administration". Die Seite "Administration - Ansichten" wird angezeigt.
2.  Falls in der Toolbar nicht die Schaltfläche "Ansichten" ausgewählt ist, klicken Sie darauf.
3.  Überprüfen Sie, ob sich in der Liste "Ansichten" links die Gruppe "Default/Base" befindet und in dieser Gruppe mindestens eine Seitenansicht mit dem Modul "[page, content_sitemap] Sitemap". Falls nicht, klicken Sie auf "Ansicht hinzufügen" und legen Sie eine solche Ansicht an.
4.  Falls die Sitemap-Ansicht bereits bestand, überprüfen Sie, ob ihr rechts im Kasten "Ausgabefilter" ein Ausgabefilter mit dem Template "page_google_sitemap.xsl" zugewiesen ist. Falls nicht, müssen Sie diese Zuweisung herstellen. Sollte noch kein passender Ausgabefilter vorhanden sein, wechseln Sie mit Hilfe der Toolbar in die Ansicht "Ausgabefilter" und erstellen Sie dort einen Ausgabefilter mit folgenden Eigenschaften:
    -   **Dateiendung**: google [kann frei gewählt werden, sollte aber den Verwendungszweck erkennen lassen]
    -   **Typ**: Seite
    -   **Zeichensatz**: utf-8
    -   **Inhaltstyp**: text/xml
    -   **Filtermodul**: [Default/Base] XSLT-Ausgabefilter
    -   **Template Directory**: google [In dem mit papaya CMS gelieferten Standard-Template-Set befindet sich das gewünschte Template in diesem Unterverzeichnis; bei einem eigenen Projekt kann es auch in einem anderen Verzeichnis liegen]

5.  Falls Sie den Ausgabefilter gerade erst eingerichtet haben, müssen Sie ihn nun für Ihre Sitemap-Ansicht aktivieren und das Template "page_google_sitemap.xsl" auswählen. **Wichtig**: Der Link-Ausgabemodus muss "html" sein, damit die Sitemap URLs für die HTML-Ansicht Ihrer Webseiten enthält.
6.  Wechseln Sie nun im Hauptmenü auf "Ändern" in der Gruppe "Seiten". Die Ansicht "Seiten - Ändern" wird angezeigt.
7.  Erstellen Sie hier eine neue Seite mit der neu erzeugten Sitemap-Ansicht. Details dazu finden Sie im papaya CMS Benutzerhandbuch.

Make settings in the Pages Connector
------------------------------------

![thumb|border|right|300px|Einstellungen für den Pages Connector in dessen Moduloptionen](images/Pagesconnector_options.png) Das Modul PagesConnector stellt die eigentliche Funktionalität für den Sitemap-Versand bereit. Die Einstellungen dafür werden in den Moduloptionen dieses Moduls vorgenommen. Um die Moduloptionen einzustellen, gehen Sie wie folgt vor:

1.  Klicken Sie im Hauptmenü auf "Module" im Bereich "Administration". Die Ansicht "Administration - Module" wird angezeigt.
2.  Klicken Sie im Bereich "Packages" auf der linken Seite auf die Gruppe "Default/Base". Rechts neben "Packages" wird der Bereich "Package content" angezeigt.
3.  Klicken Sie im Bereich "Package content" auf das Modul "Pages Connector". Ganz rechts werden die Details dieses Moduls angezeigt.
4.  Klicken Sie in der Toolbar der Moduldetails auf die Schaltfläche "Optionen". Die Moduloptionen des Moduls Pages Connector werden angezigt.
5.  Füllen Sie die Moduloptionen wie folgt aus:
    -   **Notfication URLs**: URLs, unter denen Sie Ihre Sitemaps an Suchmaschinen senden können. Verwenden Sie den Platzhalter {%SITEMAP%} für die Stelle, an der Ihre eigene Sitemap-URL in die URL eingefügt werden soll. Voreingestellt sind die URLs von Google, Bing und Yahoo! (Stand Sommer 2014). Sie können weitere URLs hinzufügen, je eine pro Zeile.
    -   **Sitemap**: Die Seiten-ID Ihrer Sitemap-Seite. Falls Sie sie auswendig wissen, können Sie sie einfach eintippen. Andernfalls hilft Ihnen die Schaltfläche rechts neben dem Eingabefeld - sie öffnet eine Lightbox, in der Sie die gewünschte Seite aus dem Seitenbaum auswählen können.
    -   **Ausgabefilter**: Wählen Sie den Ausgabefilter aus, der das Google-Sitemaps-Template bereitstellt.

6.  Klicken Sie abschließend auf "Speichern", um Ihre Änderungen zu übernehmen.

Configure the Action Dispatcher
-------------------------------

![thumb|border|right|300px|Einstellungen im Action Dispatcher zum Informieren von Suchmaschinen](images/Actiondispatcher_onpublish_pagesconnector.png) Schließlich wird der Action Dispatcher konfiguriert, damit die Sitemap-Versand-Funktion beim Veröffentlichen von Seiten automatisch in Gang gesetzt wird. Gehen Sie dazu folgendermaßen vor:

1.  Klicken Sie im Hauptmenü von papaya CMS auf "Anwendungen". Die Ansicht "Anwendungen" wird angezeigt.
2.  Klicken Sie in der Liste der Anwendungen auf "Action Dispatcher". Die Administration des Moduls Action Dispatcher wird angezeigt.
3.  Falls sich links im Bereich "Action Groups" noch nicht der Eintrag "default" befindet, klicken Sie in der Toolbar auf "Add Group". Geben Sie den Namen "default" ein und klicken Sie auf "Speichern".
4.  Klicken Sie die bereits bestehende oder neu erstellte Gruppe "default" an. Falls bereits Aktionen für diese Gruppe eingetragen wurden, stehen diese unter dem Bereich Gruppen.
5.  Sollten keine Aktionen vorhanden sein oder zumindest die Aktion "onPublishPage" noch nicht existieren, klicken Sie in der Toolbar auf "Aktion hinzufügen". Geben Sie den Namen "onPublishPage" ein und klicken Sie auf "Speichern".
6.  Klicken Sie die bereits bestehende oder neu erstellte Aktion "default" an. Falls bereits Observer für diese Aktion registriert sind, stehen diese im Hauptbereich der Seite unter "Observers".
7.  Sollten keine Observer vorhanden sein oder zumindest der Observer "PapayaBasePagesConnector" noch nicht vorhanden sein, klicken Sie in der Toolbar auf "Observer hinzufügen". Wählen Sie "PapayaBasePagesConnector" aus dem Auswahlfeld aus und klicken Sie auf "Hinzufügen".
