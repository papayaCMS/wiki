---
title: XML-Ausgabe der Module
permalink: /XML-Ausgabe_der_Module/
---

Seiten- und Boxmodule sind Schnittstellen für die Ein- und Ausgabe von Inhalten. Für die Ausgabe werden die Inhalte als XML-Dokument an den Ausgabefilter geliefert. Dieser transformiert das XML der Boxen und Seiten in das gewünschte Ausgabeformat wie HTML oder RSS.

Das Besondere daran ist, dass die Ausgaben der Boxen getrennt von der Ausgabe der Seiten in das Zielformat transformiert wird. Wenn eine Box mit einer Seite verknüpft ist, wird die Boxausgabe in das XML der Seite in Form eines CDATA-Abschnittes eingebunden. Wenn der Ausgabefilter das Seiten-XML transformiert, muss die eingebettete Boxenausgabe lediglich in das Zieldokument durchgereicht werden.

Sowohl die Kombination verschiedener Modulausgaben als auch die Transformation des XML-Outputs der Module in ein gewünschtes Ausgabeformat wird durch XSLT-Templates realisiert. Templates bestehen dabei aus einem Satz von XSLT-Dateien, wobei für jedes Modul eine separate XSLT-Datei vorliegt.

[export_de/Kategorie.md:Formatvorlagen in papaya CMS](export_de/Kategorie.md:Formatvorlagen_in_papaya_CMS )