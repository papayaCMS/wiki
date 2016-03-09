---
title: Element content
permalink: /Element_content/
---

Das `<content>` -Element enthält den Fließtext eines Artikels inklusive Bilder und Tabellen. Es besteht aus mindestens einem `<section>` -Element, das ein `<data>` -Element enthält. Der Inhalt der Seite wird direkt in das `<data>` -Element eingefügt.

Optional kann das `<section>` -Element auch ein `<title>` -Element enthalten, das eine Zwischenüberschrift darstellt. Dies ist dann sinnvoll, wenn ein Seitenmodul mehrere betitelte Abschnitte enthält.

==

<section>
-Element== Das `<section>` -Element stellt einen Abschnitt mit mehreren Absätzen dar. Jedes `<section>` -Element ist einer bestimmten `<page>` -Definition aus dem `<layout>` -Element zugeordnet. In der folgenden Tabelle sind die Attribute des `<section>` -Elements aufgeschlüsselt:

|Attribut|Bedeutung|
|--------|---------|
|page|Zuordnung zum `<page>` -Tag aus dem `<layout>` -Element, das die Layoutdefinition enthält.|
|page-break-before|„yes“, wenn der Seitenumbruch vor diesem `<section>` -Element erfolgen soll, andernfalls „no“.|

==

<title>
-Element== Das optionale `<title>` -Element kann immer dann verwendet werden, wenn mehr als ein `<section>` -Element benutzt wird. Dies ist der Fall, wenn Sie ein Seitenmodul benutzen, das mehr als einen betitelten Abschnitt enthält.

<data>-Element
--------------

Das `<data>` -Element ist das unmittelbare Kindelement eines `<section>` -Elements. Es kann eine Reihe von Block-Level-Elementen enthalten, die einzelne Absätze bilden können. Diese Block-Level-Elemente enthalten wiederum Inline-Elemente, mit deren Hilfe einzelne Wörter physisch formatiert werden können. Das `<data>` -Element enthält keine Attribute.

[export_de/Kategorie:papaya-Formatierungsobjekt](export_de/Kategorie:papaya-Formatierungsobjekt )