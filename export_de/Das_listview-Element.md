---
title: Das listview-Element
permalink: /Das_listview-Element/
---

Für fast alle Arten tabellarischer Darstellung wird die Listview verwendet.

XML-Struktur
------------

Die Grundstruktur führt alle zur Verfügung stehenden Elemente und Attribute zur schnellen Übersicht auf.

Content-Modell des <listitem>-Elements
--------------------------------------

Die folgende Tabelle listet alle Attribute des `<listview>` -Elements auf:

|Attribut|Bedeutung|
|--------|---------|
|hint|Wenn vorhanden, wird ein Info-Icon oben rechts ausgegeben. Mouseover darüber blendet diesen Hinweis ein.|
|icon|Icon, das vorne in der Titelzeile angezeigt wird|
|maximize|Link, der aufgerufen wird, wenn auf maximize (+) geklickt wird (das Icon wird automatisch erzeugt)|
|minimize|Link, der aufgerufen wird, wenn auf minimize (-) geklickt wird (das Icon wird automatisch erzeugt)|
|title|Der Titel der Listview|
|width|Die Breite der Listview. Wenn nicht angegeben, wird die Breite der Layout-Spalte, in die die Listview eingefügt wurde verwendet.|

Das Element <listview> enthält unmittelbar die folgenden Elemente:

|Attribut|Bedeutung|
|--------|---------|
|buttons|Dieses Element enthält Buttons für Paging oder weitere Funktionen. Die Buttons werden zwischen Titel und Spaltenheader eingefügt. Sollen diese mittig ausgerichtet werden, dürfen die Buttons nicht in `<left>` - oder `<right>` -Elemente gesetzt werden.|
|cols|Enthält die Elemente mit den Spaltenüberschriften.|
|items|Enthält den eigentlichen Inhalt der Listview.|
|menu|Enthält ein Menü, das unterhalb der Listview dargestellt wird.|
|status|Optionale Statuszeile, die unter die Listview eingeblendet werden kann.|

Das Element <buttons>
---------------------

Das Element `<buttons>` enthält Buttons für Paging oder sonstige Funktionen. Sollen die Buttons mittig ausgerichtet werden, dürfen sie nicht in `<left>` - oder `<right>` -Elemente gesetzt werden. Das Element `<buttons>` enthält folgende Elemente:

|Attribut|Bedeutung|
|--------|---------|
|left|Dieses Element enthält Buttons, die links ausgerichtet werden.|
|right|Dieses Element enthält Buttons, die rechts ausgerichtet werden.|
|button|Dieses Element stellt einen Button dar.|

Die Elemente `<left>` und `<right>` besitzen keine Attribute. Beide Elemente enthalten mindestens ein `<button>` -Element. Die Attribute des `<button>` -Elements werden in der folgenden Tabelle aufgeschlüsselt:

|Attribut|Bedeutung|
|--------|---------|
|accesskey|Accesskey für diesen Button|
|down|Der Button wird gedrückt dargestellt.|
|glyph|Das Icon für den Button (vgl. Attribute für das glyph-Element - src TODO: add link)|
|hint|Hinweis, der bei Mouseover angezeigt wird.|
|href|Link, der bei Klick auf den Button aufgerufen wird.|
|onclick|JavaScript onClick-Handler.|
|target|Linkziel für den Button (z.B. _blank)|
|title|Titel des Buttons. Der Text wird neben dem Button angezeigt.|

==Das Element <cols> und seine

<col>
-Elemente== Das Element `<cols>` enthält `<col>` -Elemente mit den Spaltenüberschriften. Die jeweiligen `<col>` -Elemente werden den einzelnen Spalten zugeordnet. Die folgende Tabelle schlüsselt alle Attribute des `<col>` -Elements auf:

|Attribut|Bedeutung|
|--------|---------|
|align|Ausrichtung der Spaltenüberschrift (right, left, center)|
|hint|Hinweis, der bei Mouseover über den Titel angezeigt wird.|
|href|Link, der bei Klick auf die Spaltenüberschrift aufgerufen wird. Der Link sollte die Sortierfunktion für die Spalte aufrufen.|
|sort|Aktuelle Sortierreihenfolge der Spalte. Folgende Werte sind erlaubt:

1.  `desc`: Spalte ist absteigend sortiert.
2.  `asc`: Spalte ist aufsteigend sortiert.
3.  `none`: Keine spezifische Sortierung der Spalte.

`Es wird ein entsprechendes Icon eingeblendet. `|
|span|Falls eine Spaltenüberschriften über mehrere Inhaltsspalten geht, wird die Spaltenanzahl in diesem Attribut angegeben. Dieses Attribut entspricht dem `colspan` -Attribut in HTML-Tabellen (td).|

Das Element <items> und die Unterelemente <listitem>
----------------------------------------------------

Das Element `<items>` enthält Zeilen der Liste der Listview in Form von `<listitem>` -Elementen. Das `<listitem>` -Element enthält den eigentlichen Inhalt in `<subitem>` -Elemente. Ein `<subitem>` entspricht einer Zelle in der Zeile. Die Attribute des <listitem>-Elements sind in der folgenden Tabelle aufgeschlüsselt:

|Attribut|Bedeutung|
|--------|---------|
|emphased|Die Zeile ist hervorgehoben, die Formatierung ist dann fettgedruckt.|
|hint|Hinweis, der bei mouseover über den Zeilentitel eingeblendet wird.|
|href|Link, der bei Klick auf den Zeilentitel aufgerufen wird.|
|id|ID der Zeile.|
|image|Icon, das vor dem Titel angezeigt wird.|
|indent|Einrückung der Zeile, wichtig bei Baumansichten.|
|nhref|Link, der aufgerufen wird, wenn auf den Knoten geklickt wird. Das Attribut `node` muss vorhanden sein.|
|node|Status des Knotens (Baumansicht):

1.  `close`: Knoten geschlossen.
2.  `open`: Knoten geöffnet.
3.  `empty`: Knoten leer (enthält keine eingebetteten Knoten).

`Ein Knoten-Icon zum Öffnen und Schließen des Knotens wird noch vor dem Bild angezeigt; in Kombination mit dem Attribut ``indent`` lassen sich Baumansichten einfach realisieren. `|
|selected|Die Zeile wird als ausgewählt markiert. Dazu erhält das Attribut `selected` den Wert „selected“. Die Zeile wird farbig hinterlegt.|
|subtitle|Untertitel der Zeile, wird unterhalb des Titels angezeigt und anders formatiert (kursiv).|
|title|Der Titel der Zeile.|

Das Element <subitem>
---------------------

Das Element `<subitem>` enthält den eigentlichen Inhalt der Liste. Ein `<subitem>` -Element steht für eine Zelle in der Listenzeile. Für jede Zelle nach dem Titelfeld muss ein subitem vorhanden sein. Die folgende Tabelle listet alle Attribute des `<subitem>` -Elements auf:

|Attribut|Bedeutung|
|--------|---------|
|align|Ausrichtung der Zelle:

1.  `left`: Zelleninhalt wird links ausgerichtet.
2.  `center`: Zelleninhalt wird mittig ausgerichtet.
3.  `right`: Zelleninhalt wird rechts ausgerichtet.|
|overflow|Damit kann überfließender Text abgeschnitten werden (overflow="hidden")|
|span|Damit können mehrere Zellen einer Zeile zusammengefasst werden (entspricht dem Attribut `colspan` für HTML-Tabellen (td).|
|wrap|Falls `wrap="nowrap"` angegeben wird, soll der Browser die Zeile nicht umbrechen.|

„Tiled“-Ausgabe
---------------

In der Anwendungen-Übersicht und der Iconliste wird die so genannte „Tiled“-Ausgabe verwendet. Alle Elemente werden nacheinander angeordnet. Wenn man das Browserfenster verkleinert, rutschen die Elemente nach, die gesamte Breite wird also genutzt und horizontales Scrolling ausgeschlossen. Auch braucht man sich über eine Verteilung der Elemente in Zeilen und Spalten keine Gedanken zu machen.

Der Aufbau ist dementsprechend etwas anders:

Der einzige Unterschied besteht darin, dass Listview das Attribut mode="tile" hat. Je nachdem, was man anzeigen möchte, kann man gut links mit Icons kombinieren, oder anderes HTML einfügen. Einige Angaben wie Menü, Status und Columns machen bei dieser Listenausgabe natürlich wenig Sinn.

[Kategorie:Die einzelnen Komponenten im Detail](export_de/Kategorie:Die_einzelnen_Komponenten_im_Detail.md)