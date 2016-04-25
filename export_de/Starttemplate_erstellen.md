
Das Starttemplate ist das einzige Template mit dem `match` -Attribut. Das Muster im `match` -Attribut liefert dabei als Kontextknoten für das Template das Element `/page`, das die XML-Ausgabe des Seitenmoduls enthält. Nur dann, wenn das Seitenmodul vom Typ `content_sitemap` ist, wird das Basistemplate aufgerufen, in dem das Sitemap-Dokument erzeugt wird. Im folgenden Listing wird das Starttemplate dargestellt:

**Starttemplate**

~~~~ {.xml}
<xsl:template match="/page">
  <xsl:variable name="module" select="content/topic/@module"/>
  <xsl:choose>
    <xsl:when test="$module = 'content_sitemap'">
      <xsl:call-template name="content_sitemap">
        <xsl:with-param name="topic"
                        select="/page/content/topic" />
      </xsl:call-template>
    </xsl:when>
  </xsl:choose>
</xsl:template>
~~~~

[Kategorie:Sitemap-Template erstellen](../export_de/Kategorie:Sitemap-Template_erstellen.md)