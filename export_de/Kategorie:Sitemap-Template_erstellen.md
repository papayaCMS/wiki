---
title: Kategorie:Sitemap-Template erstellen
permalink: /Kategorie:Sitemap-Template_erstellen/
---

Es ist sehr einfach, ein Template für die Sitemap-Ausgabe zu schreiben. Die meisten benötigten Angaben werden durch das Sitemap-Modul in die XML-Ausgabe der Seite eingefügt.

Das XSLT-Template für die Ausgabe der Sitemap besteht aus folgenden Templates:

1.  Ein Starttemplate mit dem `match` -Muster auf `/page`.
2.  Das Basistemplate, das den Hostnamen in einer Variablen speichert und das Wurzelelement `<urlset>` definiert.
3.  Ein Template für die `<mapitem>` -Elemente aus dem Quelldokument. Das Template ruft sich solange selbst rekursiv auf, bis alle `<mapitem>` -Elemente in `<url>` -Tags überführt worden sind.

[Kategorie:Sitemap erstellen](/Kategorie:Sitemap_erstellen )