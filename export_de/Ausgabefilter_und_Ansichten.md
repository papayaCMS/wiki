---
title: Ausgabefilter und Ansichten
permalink: /Ausgabefilter_und_Ansichten/
---

Der Ausgabefilter in papaya CMS wandelt die Inhalte durch den Ausgabefilter in das gewünschte Zielformat um. Anhand der Regeln im XSLT-Stylesheet weiß der Ausgabefilter, wie die jeweiligen XML-Elemente der Box- und Seitenmodule transformiert werden müssen.

Modularität
-----------

Da papaya CMS über beliebig viele Module verfügen kann, die ihre Inhalte in unterschiedlichem XML ausliefern, ermöglicht die Kombination von Ausgabefilter und XSLT-Template eine große Flexibilität. Sie müssen also nicht für jedes Modul einen neuen Ausgabefilter programmieren, sondern müssen lediglich ein XSLT-Stylesheet schreiben. XSLT-Stylesheets sind deutlich einfacher zu programmieren als PHP-Programme, was den Erstellungsprozess für Templates vereinfacht.

Eigenschaften des Ausgabefilters
--------------------------------

Während XSLT-Templates Transformationsregeln definieren, bestimmt der Ausgabefilter folgende Eigenschaften der Ausgabe:

|Eigenschaft|Bedeutung|
|-----------|---------|
|Content-Type|Eigenschaft im HTTP-Header, in der der Inhaltstyp festgelegt wird. Für HTML-Webseiten sollte er auf „text/html“ eingestellt sein, bei RSS-Feeds auf „text/xml“. Für die PDF-Ausgabe lautet der Content-Type „application/x-pdf“.|
|Encoding|Zeichensatz der Ausgabe. Sie können festlegen, welcher Zeichensatz bei der Ausgabe einer Webseite benutzt werden soll.|
|Dateiendung|Wenn Sie einen Ausgabefilter anlegen, können Sie auch bestimmen, mit welcher Endung der Ausgabefilter assoziiert werden soll. Für HTML sollten Sie beispielsweise die Endung `.html` wählen, während für RSS-Feeds die Endung `.rss` ausgewählt werden sollte. Wenn Sie einen Ausgabefilter für PDF anlegen, geben Sie als Endung entsprechend `.pdf` an.|

Durch die Dateiendung kann papaya CMS beim Aufruf einer Seite entscheiden, welcher Ausgabefilter benutzt werden soll, um die Inhalte der Seite auszuliefern. Wenn Sie also eine relative URL wie /index.12.de.html in die Adresszeile eingeben, liefert Ihr papaya CMS die HTML-Version der Seite mit der ID 12 aus. Dies funktioniert jedoch nur, wenn die Ansicht der Seite auch mit dem entsprechenden Ausgabefilter verknüpft worden ist.

Näheres dazu, wie Sie Ausgabefilter anlegen, erfahren Sie im Handbuch für Benutzer und Administratoren, Kapitel 15.3.

Ansichten
---------

Seiten- und Boxmodule können nicht direkt benutzt werden, um den Inhalt einer Seite oder einer Box zu speichern. Stattdessen legen Sie für jedes benötigte Modul eine Ansicht an. Ansichten verknüpften ein Seiten- oder Boxmodul mit einem Ausgabefilter. Die Ansicht speichert zudem auch, welche XSLT-Stylesheetdatei für den Ausgabefilter benutzt werden soll.

Ansichten lassen sich darüber hinaus mit beliebig vielen Ausgabefiltern kombinieren, wobei für jeden verknüpften Ausgabefilter genau ein XSLT-Template ausgewählt werden kann. Eine Ansicht kann also mit Ausgabefiltern für HTML, PDF und RSS verknüpft werden. Wenn Sie eine Seite mit solch einer Ansicht verknüpften, können Sie sich die Seite sowohl als HTML-Dokument öffnen als auch als RSS-Feed oder PDF-Datei herunterladen. Sie müssen lediglich die entsprechende Dateiendung in die Adresszeile Ihres Browsers eingeben. Näheres zu Ansichten erfahren Sie im Handbuch für Benutzer und Administratoren, Kapitel 15.

[Kategorie:Formatvorlagen in papaya CMS](export_de/Kategorie:Formatvorlagen_in_papaya_CMS )