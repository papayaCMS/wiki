---
title: Mapitem-Template erstellen
permalink: /Mapitem-Template_erstellen/
---

Das `<mapitem>` -Template wandelt alle vorhandenen `<mapitem>` -Elemente aus dem Quelldokument in `<url>` -Elemente für das Sitemap-Dokument um. Da `<mapitem>` -Elemente ineinander verschachtelt sein können, ruft sich das mapitem-Template so lange selbst auf, bis alle `<mapitem>` -Tags in `<url>` -Tags transformiert worden sind. Bei diesem Vorgang wird nur ein Teil der Informationen aus den `<mapitem>` -Element genutzt.

**mapitem-Template**

~~~~ {.xml}
<xsl:template name="mapitem">
  <xsl:param name="items" />
  <xsl:param name="host" />
  <xsl:for-each select="$items">
    <url>
      <loc><xsl:value-of select="$host" /><xsl:value-of select="@href" disable-output-escaping="true" /></loc>
      <lastmod><xsl:value-of select="substring(@lastmod,0,11)" disable-output-escaping="true" /></lastmod>
      <changefreq><xsl:value-of select="@changefreq" disable-output-escaping="true" /></changefreq>
      <priority><xsl:value-of select="@priority" disable-output-escaping="true" /></priority>
    </url>
    <xsl:if test="mapitem">
      <xsl:call-template name="mapitem">
       <xsl:with-param name="items" select="mapitem" />
       <xsl:with-param name="host" select="$host" />
      </xsl:call-template>
    </xsl:if>
  </xsl:for-each>
</xsl:template>
~~~~

[Kategorie:Sitemap-Template erstellen](Kategorie:Sitemap-Template_erstellen )