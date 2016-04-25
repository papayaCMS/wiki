
Die Sitemap-Datei ist sehr einfach aufgebaut. Das Wurzelelement ist `<urlset>`. Dieses Element kann beliebig viele `<url>` -Elemente enthalten, die alle die URL einer Webseite beschreiben. Ein `<url>` -Element wiederum besteht aus folgenden Elementen:

|Element|Bedeutung|
|-------|---------|
|`<loc>`|Enthält die absolute URL der Seite, beispielsweise <http://www.domain.tld/startseite.html> . Diese Angabe ist obligatorisch.|
|`<lastmod>`|Datum der letzten Änderung im ISO-8601-Format (YYYY-mm-dd, also Jahr, Monat, Tag). Diese Angabe ist optional.|
|`<changefreq>`|Rhythmus, in dem die Seite aktualisiert wird. Diese Angabe ist Optional. Folgende Werte sind erlaubt:
| |1.  `always`
| |2.  `hourly`
| |3.  `daily`
| |4.  `weekly`
| |5.  `monthly`
| |6.  `yearly`
| |7.  `never`|
|`<priority>`|Gibt an, wie wichtig diese Seite im Verhältnis zu den anderen Seiten der Webpräsenz ist. Diese Angabe ist optional. Die Angabe erfolgt als Dezimalwert, wobei `0.1` der Minimalwert ist und `1.0` der Maximalwert.|

Im folgenden Listing ist eine Beispiel-Sitemapdatei dargestellt:

**Ausgabe der XML-Sitemap-Datei in papaya CMS**

~~~~ {.xml}
<?xml version="1.0" encoding="utf-8" standalone="no"?>
<urlset ns="http://www.google.com/schemas/sitemap/0.84">
  <url>
    <loc>http://papaya5/startseite.11.de.html</loc>
    <lastmod>2007-03-01</lastmod>
    <changefreq>always</changefreq>
    <priority>0.8</priority>
  </url>
  <url>
    <loc>http://papaya5/test.29.de.html</loc>
    <lastmod>2007-03-01</lastmod>
    <changefreq>always</changefreq>
    <priority>0.8</priority>
  </url>
  <url>
    <loc>http://papaya5/sitemap.15.de.html</loc>
    <lastmod>2007-02-20</lastmod>
    <changefreq>always</changefreq>
    <priority>0.5</priority>
  </url>
  <url>
    <loc>http://papaya5/feedback-form.16.de.html</loc>
    <lastmod>2007-02-23</lastmod>
    <changefreq>yearly</changefreq>
    <priority>0.7</priority>
  </url>
  <url>
    <loc>http://papaya5/comm-loginform.18.de.html</loc>
    <lastmod>2007-02-23</lastmod>
    <changefreq>monthly</changefreq>
    <priority>0.5</priority>
  </url>
</urlset>
~~~~

Die oben dargestellte Sitemap-Datei enthält ein `<urlset>` -Element, in dem fünf Seiten der Website in Form von `<url>` -Elementen spezifiziert sind. Laut Dokumentation darf das `<urlset>` -Element bis zu 50.000 `<url>` -Elemente enthalten. Die maximale Größe einer Sitemap-Datei ist auf 10 MB beschränkt.

[Kategorie:Sitemap-Datei für Suchmaschinen](../export_de/Kategorie:Sitemap-Datei_für_Suchmaschinen.md)
