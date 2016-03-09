---
title: Kategorie:Papaya-Formatierungsobjekt
permalink: Kategorie:Papaya-Formatierungsobjekt/
---

Mit dem PDF-Template erzeugen Sie ein papaya-Formatierungsobjekt, dass durch den PDF-Prozessor von papaya CMS in ein PDF-Dokument übersetzt wird. Wenn Sie ein eigenes PDF-Template schreiben, müssen Sie also die XML-Struktur der Modulausgabe kennen:

1.  Sie können die XML-Ausgabe von Seiten aktivieren und anzeigen lassen, siehe [XML-Ausgabe aktivieren und anzeigen lassen](/XML-Ausgabe_aktivieren_und_anzeigen_lassen ).
2.  Die Dokumentation der XML-Seitenausgabe finden Sie in [Struktur der XML-Seitenausgabe](/Struktur_der_XML-Seitenausgabe ).

Um das papaya-Formatierungsobjekt schreiben zu können, müssen Sie wissen, wie es aufgebaut ist. Beim papaya-Formatierungsobjekt handelt es sich um ein XML-Dokument, das aus einem Layoutbereich und mehreren Inhaltsbereichen besteht. Im Layoutbereich wird das Layout der Inhalte bestimmt, die in das PDF gesetzt werden. Im Inhaltsbereich werden die Inhalte aus dem aktuellen Seitenmodul gesetzt. Diese Aufteilung in Layoutbereich und Inhaltsbereich ist bewußt an XSL-FO angelehnt.

Im Inhaltsbereich können Sie zudem HTML-Elemente benutzen, um die Inhalte zu formatieren. Es handelt sich hierbei lediglich um eine Untermenge aller HTML-Elemente. Es werden also nicht alle Elemente aus dem HTML-Standard (Version 4.01) und ihre jeweiligen Attribute unterstützt. Zudem müssen die Elemente so benutzt werden, dass sie mit den XML-Syntaxregeln konform sind. Zu den HTML-Elementen im Inhaltsbereich gehören zwei Gruppen von Elementen:

1.  Block-Level-Elemente, siehe [Block-Level-Elemente](/Block-Level-Elemente ).
2.  Inline-Elemente, siehe [Inline-Elemente](/Inline-Elemente ).

Die Struktur des papaya-Formatierungsobjektes ist in der folgenden Illustration dargestellt:

![File:papaya-fo-schema.png](images/Papaya-fo-schema.png)

Das Wurzelelement `<article>` enthält den Layoutbereich im Element `<layout>`, während der Inhaltsbereich auf die Elemente `<cover>`, `<content>`, `<footer>` und `<final>` aufgeteilt ist. In den folgenden Unterabschnitten werden diese Elemente näher beschrieben.

[Kategorie:Vorlage für die PDF-Ausgabe erstellen](export_de/Kategorie:Vorlage_für_die_PDF-Ausgabe_erstellen )
