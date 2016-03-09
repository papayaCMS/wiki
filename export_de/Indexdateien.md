---
title: Indexdateien
permalink: /Indexdateien/
---

Bei sehr umgangreichen Webprojekten werden Sie mehr als 50.000 Seiten vorliegen haben. Damit auch solche Projekte mit dem Sitemap-Protokoll erfasst werden können, haben die Autoren des Sitemaps-Protokolls Indexdateien vorgesehen. Sie legen dabei eine Menge an Sitemap-Dateien an, in der alle Ihre Seiten erfasst werden, und listen die Sitemap-Dateien in der Indexdatei auf. Anstelle der Sitemap-Datei müssen Sie in diesem Fall die Indexdatei beim Suchmaschinenbetreiber registrieren.

Die Sitemapindex-Datei ist ähnlich einfach aufgebaut wie die Sitemap-Datei. Das Wurzelelement ist `<sitemapindex>`. Dieses Element kann beliebig viele `<sitemap>` -Elemente enthalten, die alle eine Sitemap-Datei referenzieren. In der folgenden Tabelle sind die Elemente des Sitemapindex aufgeschlüsselt:

|Element|Bedeutung|
|-------|---------|
|`<sitemapindex>`|Wurzelelement, das beliebig viele `<sitemap>` -Elemente enthalten kann.|
|`<sitemap>`|Fasst Informationen zu einer Sitemap-Datei zusammen. Enthält das obligatorische Element `<loc>` und das fakultative Element `<lastmod>`.|
|`<loc>`|Enthält die URI zu einer Sitemap-Datei.|
|`<lastmod>`|Enthält das Datum der letzten Änderung der Sitemap-Datei.|

[Kategorie:Sitemap-Datei für Suchmaschinen](export_de/Kategorie:Sitemap-Datei_für_Suchmaschinen.md)