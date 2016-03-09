---
title: Layoutbereich (Element layout)
permalink: /Layoutbereich_(Element_layout)/
---

Im Element `<layout>` wird das Layout des Dokuments definiert. Sie können folgende Layout-Eigenschaften bestimmen:

1.  Die zu verwendende Schriftfamilie bestimmen Sie im Element `<fonts>`.
2.  Templates für verschiedene Seitentypen wie Titelseite und Standardseiten legen Sie mit dem Element `<templates>` fest.
3.  Das Layout für die verschiedenen Seiten bestimmen Sie im Element `<pages>`.
4.  Geben Sie im `<tags>` -Element an, wie HTML-Elemente (bspw. `<h1>` ) gesetzt werden sollen.

Das <layout>-Tag enthält keine Attribute.

<fonts>-Element mit <font>-Elementen
------------------------------------

Das `<fonts>` -Element kann beliebig viele `<font>` -Tags enthalten, die eine Schriftfamilie und die jeweiligen Varianten für Fett und Kursiv einbindet. Die Attribute des <font>-Elements werden in der folgenden Tabelle aufgeschlüsselt:

|Attribut|Bedeutung|
|--------|---------|
|name|Name der Schriftfamilie.|
|default|Standardfont für die Grundschrift.|
|bold|Fontmetrikdatei für fetten Schriftschnitt.|
|italic|Fontmetrikdatei für kursiven Schriftschnitt.|
|bolditalic|Fontmetrikdatei für fettkursiven Schriftschnitt.|

<templates>-Element mit <template>-Elementen
--------------------------------------------

Das `<templates>` -Element kann beliebig viele `<template>` -Elemente enthalten, die eine bestimmte Seite der PDF-Musterdatei ansprechen. Ein <template>-Element entspricht damit einer Seitenvorlage. Jedes `<template>` -Element erhält dabei einen eindeutigen Namen, um die Seitenvorlage ansprechen zu können.

Anhand des Wertes im Attribut `name` können Sie im Inhaltsbereich (siehe [Inhaltsbereich](/Inhaltsbereich ) ) das entsprechende Template auswählen. Wenn Sie Inhalte einbinden, ordnen Sie über das `name` -Attribut die Inhalte der entsprechenden Seite in der PDF-Musterdatei zu. Die Attribute des `<template>` -Elements sind in der folgenden Tabelle aufgeschlüsselt:

|Attribut|Bedeutung|
|--------|---------|
|name|Eindeutiger Bezeichner für diese Seitenvorlage|
|file|Name der PDF-Musterdatei. Die Pfadangabe erfolgt relativ zum Verzeichnis des XSLT-Templates. Im Demotemplate handelt es sich um das Unterverzeichnis mit dem Namen `template/`.|
|page|Seitennummer, beginnend mit 1.|

<tags>-Element
--------------

Das `<layout>` -Element kann optional auch ein `<tags>` -Element enthalten, mit dem Sie ausgewählten Tags eine Schrift in bestimmter Größe, bestimmtem Gewicht und Stil zuweisen. Das `<tags>` -Element enthält dabei einzelne `<tag>` -Elemente, in denen Sie den gewünschten Tag auswählen.

<tag>-Element
-------------

Mit dem `<tag>` -Element wählen Sie ein HTML-Tag aus und können bestimmen, mit welchem Schriftschnitt der Inhalt dieses Tags formatiert werden soll. Die Attribute des `<tag>` -Elements sind in der folgenden Tabelle aufgeschlüsselt:

|Attribut|Bedeutung|
|--------|---------|
|name|Name des HTML-Tags, das formatiert werden soll.|
|font|Schriftschnitt, der dem Tag zugeordnet werden soll. Die Angaben sind:

1.  Schriftfamilie, alternativ „serif“ für Serifenschrift, „non serif“ für serifenlose Schrift.
2.  Schriftgröße in Punkt (pt).
3.  Schriftform:
    1.  *italic*: Kursiv.
    2.  *bold*: Fett.
    3.  *bolditalic*: Fettkursiv.|

<pages>-Element
---------------

Das `<pages>` -Element dient dazu, die Grundschrift sowie die Seitenränder zu bestimmen. In der folgenden Tabelle sind die Attribute des `<pages>` -Elements aufgeschlüsselt:

|Attribut|Bedeutung|
|--------|---------|
|font|Eindeutiger Bezeichner für diese Seitenvorlage|
|margin|Die angegebenen vier Werte bestimmen die Seitenränder. Die vier Werte stehen für den Abstand des Inhaltes von *oben*, *rechts*, *unten* und *links*.|
|align|Ausrichtung des Textes. Der angegebene Wert kann durch align-Attribute in Elementen aus dem Inhaltsbereich überlagert werden. Mögliche Werte sind:

1.  left
2.  center
3.  right
4.  justify|

<page>-Element
--------------

Das <pages>-Element enthält beliebig viele `<page>` -Elemente. Mit einem `<page>` -Element bestimmen Sie die Position von Inhalten auf der Seite. Sie können dabei bestimmte Seitentypen wie Titelseite, Standardseite oder Schluss-Seite unterscheiden. In der folgenden Tabelle sind die Attribute des `<page>` -Elements aufgeschlüsselt:

|Attribut|Bedeutung|
|--------|---------|
|name|Name dieser Seitenvorlage.|
|mode|„elements“, um in den Element-Modus umzuschalten, „default“, wenn die Seite Fließtext in `<column>` -Elementen enthalten soll.|
|template|Template-Definition auswählen. Der Wert muss dem *name* -Attribut eines `<template>` -Elements entsprechen.|
|font|Schrift der Seite festlegen. Die Angaben sind:

1.  Schriftfamilie
2.  Schriftgröße
3.  Schriftgewicht oder -stil ( `bold`, `italic`, `bolditalic` )|

<element>-Tag
-------------

Für die Titelseite können Sie den Titel sowie den Untertitel des Artikels in fest definierte Bereiche der Seite einfügen. Diese Bereiche bestimmen Sie Dabei mit `<element>` -Tags. Um `<element>` -Tags benutzen zu können, müssen Sie das `<page>` -Element in den Element-Modus ( `mode =
      "elements"` ) umschalten. Die folgende Tabelle schlüsselt die Bedeutung der einzelnen Attribute auf:

|Attribut|Bedeutung|
|--------|---------|
|for|Name des aktuellen Elements.|
|margin|Position des Elements. Die Positionierung erfolgt über die Angabe des Seitenabstandes. Die vier Werte stehen für:

1.  oben
2.  rechts
3.  unten
4.  links|
|align|Ausrichtung der Texte, die in diese Seite eingebunden werden. Mögliche Werte:

1.  right
2.  center
3.  left
4.  justify|

<column>-Element
----------------

Standardmäßig wird Fließtext in Spalten (auch Kolumnen genannt) gesetzt. Eine Seite kann dabei nur aus einer einzigen Spalte bestehen oder beliebig viele zusätzliche Spalten haben. Die einzelnen Spalten einer Seite werden dabei von links nach rechts aufgefüllt. Sobald eine Spalte keinen Platz mehr hat, werden die Inhalte in die folgende Spalte gesetzt. Wenn die letzte Spalte einer Seite voll ist, wird ein Seitenumbruch eingefügt, sodass wieder mit einer neuen Seite begonnen wird.

Die Abstände sowie die Textausrichtung einer Spalte können Sie mit dem `<column>` -Element bestimmen. Die Attribute dieses Elements werden in der folgenden Tabelle erläutert:

|Attribut|Bedeutung|
|--------|---------|
|margin|Abstand zu den Seitenrändern. Die vier Werte stehen für:

1.  oben
2.  rechts
3.  unten
4.  links|
|align|Textausrichtung. Mögliche Werte sind:

1.  left
2.  center
3.  right
4.  justify|

Damit das `<column>` -Element benutzt werden kann, müssen Sie das `<page>` -Element in den Standardmodus ( `mode = "default"` ) umschalten.

==

<footer>
-Element== Das `<page>` -Element kann auch ein `<footer>` -Element enthalten. Mit dem `<footer>` -Element bestimmen Sie die Position sowie die Schriftgröße der Fußzeile. Sie können zusätzlich die Textausrichtung festlegen. Näheres zu den Attributen des `<footer>` -Elements erfahren Sie in der folgenden Tabelle:

|Attribut|Bedeutung|
|--------|---------|
|margin|Abstand zu den Seitenrändern. Die vier Werte stehen der Reihe nach für:

1.  oben
2.  rechts
3.  unten
4.  links|
|align|Textausrichtung. Mögliche Werte sind:

1.  left
2.  center
3.  right
4.  justify|
|font|Schrift der Seite festlegen. Die Angaben sind:

1.  Schriftfamilie.
2.  Schriftgröße in Punkt (pt).
3.  Schriftgewicht oder -stil ( `bold`, `italic`, `bolditalic` ).|

[Kategorie:papaya-Formatierungsobjekt](export_de/Kategorie:papaya-Formatierungsobjekt )