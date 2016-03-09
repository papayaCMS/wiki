---
title: Basistemplate erstellen
permalink: /Basistemplate_erstellen/
---

Das Basistemplate speichert den Hostnamen in der XSLT-Variablen `$host` und erzeugt das Wurzelelement `<urlset>`. Es ist im folgenden Listing dargestellt:

**Basistemplate**

~~~~ {.xml}
...
<xsl:template name="content_sitemap">
  <xsl:param name="topic" />
  <xsl:variable name="host">
    <xsl:choose>
      <xsl:when test="$topic/host">
        <xsl:text>http://</xsl:text>
        <xsl:value-of
         select="$topic/host"
         disable-output-escaping="true" />
        <xsl:text>/</xsl:text>
      </xsl:when>
      <xsl:otherwise>
        <xsl:text>/</xsl:text>
      </xsl:otherwise>
    </xsl:choose>
  </xsl:variable>

  <urlset ns="http://www.sitemaps.org/schemas/sitemap/0.9">
    <xsl:call-template name="mapitem">
      <xsl:with-param name="items"
                      select="$topic/sitemap/mapitem" />
      <xsl:with-param name="host">
        <xsl:value-of select="$host" />
      </xsl:with-param>
    </xsl:call-template>
  </urlset>
</xsl:template>
...
~~~~

Der Hostname ist im Quelldokument im Element `/page/content/topic/host` enthalten. Um eine gültige URL zu erzeugen, muss der Hostname um die Protokoll-Angabe `http://` ergänzt werden. Zusätzlich wird ein `/` angehängt. Das erzeugte Wurzelelement `<urlset>` erhält zudem einen Namensraum. Anschließend wird das `mapitem` -Template aufgerufen, das als Parameter noch die Variable mit dem Hostnamen erhält

[Kategorie:Sitemap-Template erstellen](/Kategorie:Sitemap-Template_erstellen )