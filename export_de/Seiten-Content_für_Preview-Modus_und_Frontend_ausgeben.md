
In der Methode `papaya_page::createTopic()` wird die Instanz einer Topic-Klasse angelegt und im Attribut `papaya_page::topic` gespeichert. Als Topic-Objekt wird eine Instanz der Klasse `papaya_topic` oder `papaya_publictopic` eingesetzt, die von base_topic_edit abgeleitet ist. Wenn ein Redakteur im Vorschaumodus die Seite aufruft, wird `papaya_topic` instanziiert. Für das Frontend wird die Klasse `papaya_publictopic` eingebunden.

Die Hierarchie der Topic-Klassen ist im folgenden UML-Diagramm dargestellt:

![File: Vererbungshierarchie von papaya_topic und papaya_topic_public](images/KlassenBeimAufrufUMLDiagramm.png)

Das Topic-Objekt als Hüllenklasse für Content-Module

Das Topic-Objekt ist eine Hüllenklasse für bestimmte Content-Klassen, die von `base_plugin` abgeleitet sind. Im Topic-Objekt wird die Content-Klasse instanziiert, die mit der Ansicht der aktuellen Seite verknüpft ist. Anschließend werden die Schnittstellen für die Datenein- und ausgabe angesprochen.

Die Schnittstelle für die Dateneingabe ist das Eingabeformular „Inhalt bearbeiten“, das in der Seitenansicht im papaya-Backend dargestellt wird. Dieses Formular wird in den Content-Klassen im Array `$editFields` definiert, siehe [Eingabemasken für Inhaltsmodule definieren](Eingabemasken_für_Inhaltsmodule_definieren.md). Um das Formular darzustellen, wird in der Klasse `base_topic_edit` die Methode `getEditContent()` aufgerufen. Diese ruft wiederum die Methode `getForm()` des Content-Moduls auf. `getForm()` gibt die Repräsentation des Eingabeformulars als XML aus. Dieses XML wird für das Backend als Eingabeformular transformiert.

Die Schnittstelle für die Datenausgabe besteht in der Methode `parseContent()` des Topic-Objekts. Diese Methode liest die Daten aus dem Content-Modul aus und gibt den Inhaltsteil des Seiten-XMLs zurück.

Ferner ist das Topic-Objekt für die Verwaltung der Metadaten einer Seite verantwortlich. Dazu gehören folgende Informationen:

1.  Sprachunabhängige Seiteneigenschaften wie die Seiten-ID, seitenspezifische Standard-Sprache, sowie alle Eigenschaften, die in der Seitenansicht in der Administration von papaya CMS unter „Eigenschaften“ verwaltet werden können.
2.  Die Liste der Sprachversionen der Seite.
3.  Die ID der Ansicht, die in der aktuellen Sprachversion der Seite benutzt wird.

Ausgabe des Seiteninhalts im Detail

Das folgende Sequenzdiagramm stellt vor, wie das Topic-Objekt instanziiert wird. Ferner beschreibt das Diagramm im Detail, welche Objekte bei der Ausgabe des Seiteninhalts beteiligt sind und wie sie miteinander interagieren:

![File: Sequenzdiagramm](images/UMLDiagramPapayaTopic.png)

Im ersten Schritt wird in der Methode `createTopic()` der Klasse `papaya_page` eine Instanz des Topic-Objekts erzeugt. Die Instanz wird im Attribut `papaya_page::topic` gespeichert. Anschließend wird überprüft, ob die Seite in der aktuellen Sprachversion konfiguriert ist. Dies wird mit der Methode `base_topic::topicExists()` überprüft. Im folgenden Schritt wird versucht, den Seiteninhalt aus dem Cache zu lesen. Wenn dies fehlschlägt, wird die Methode `papaya_page::getPage()` aufgerufen. Mit dieser Methode werden erst alle Metadaten des Topic-Objekts ausgelesen, indem `$topic->loadOutput()` ausgeführt wird.

Mit der Methode `checkPublishPeriod()` aus der Klasse `papaya_publictopic` bzw. `papaya_topic` wird überprüft, ob das aktuelle Datum innerhalb des Veröffentlichungszeitraums der Seite liegt. Im Frontend sollen die Seiteninhalte nur dann ausgegeben werden, wenn sie veröffentlicht worden sind. Daher wertet die Methode `checkPublishPeriod()` der Klasse `papaya_publictopic` alle Datumsangaben für den Veröffentlichungszeitraum aus und gibt nur dann `TRUE` zurück, wenn die Seite öffentlich gestellt ist. In `papaya_topic` gibt die Methode `checkPublishPeriod()` stets `TRUE` zurück, da diese Klasse für den Vorschaumodus von papaya CMS zuständig ist.

Mit `$topic->getViewId()` wird die ID der Ansicht ausgelesen und dem Ausgabefilter übergeben. Anschließend wird in `papaya_page` eine Instanz des Layoutobjekts angelegt, das als Wrapper für den XSLT-Prozessor fungiert. Diesem Objekt soll nun das Seiten-XML übergeben werden. Zu diesem Zweck wird die Methode `parseContent()` des Topic-Objekts `$topic` aufgerufen.

Content-Modul instanziieren und Seiteninhalte auslesen

Im Topic-Objekt wird in der Methode `parseContent()` das Content-Modul instanziiert. Dazu wird die statische Methode `getPluginInstance()` der Klasse `base_pluginloader` benutzt. Die Instanz des Content-Moduls wird im Klassenattribut `$moduleObj` gespeichert. Die Instanz wird benutzt, um den Inhalt der Seite aus der Datenbank zu laden und als XML auszugeben. Zu diesen Zweck werden die Methoden `getParsedData()` und `getParsedTeaser()` des Content-Moduls ausgelesen. `getParsedTeaser()` liest den Teaser der Seite aus, `getParsedData()` den Hauptinhalt. Die XML-Ausgaben dieser Methoden werden in ein XML-Element eingebettet und an die aufrufende Instanz zurückgegeben.

XML der Seitenausgabe an Ausgabefilter übergeben

Das XML wird in der Methode `generatePage()` der Klasse `papaya_page` direkt an das Layoutobjekt übergeben. Zudem werden weitere Inhalte wie Metadaten, Ansichtenlisten für die aktuelle Seite sowie die unterschiedlichen Sprachversionen im Layoutobjekt gesetzt. Ebenso werden entsprechende Parameter für die XSLT-Transformation gesetzt.

papaya_output::getFilter erzeugt eine Instanz des Ausgabefilters. Der Ausgabefilter liest den Pfad zum XSLT-Stylesheet aus, die in der Ansicht gespeichert ist. Anschließend wird der Transformationsprozess gestartet. Dazu wird die Methode `parsePage()` ausgeführt. Diese Methode wird in `base_outputfilter` definiert und von den implementierenden Klassen überladen. Die Seite wird im entsprechenden Zielformat ausgegeben. Unabhängig davon wurden die Header bereits in der Methode `getPageOutput()` sowie in `getPage()` in der Klasse `papaya_page` gesetzt.

[Kategorie:Wie sieht es unter der Haube aus?](export_de/Kategorie:Wie_sieht_es_unter_der_Haube_aus?.md)
