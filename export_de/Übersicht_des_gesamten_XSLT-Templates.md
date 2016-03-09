---
title: Übersicht des gesamten XSLT-Templates
permalink: /Übersicht_des_gesamten_XSLT-Templates/
---

Das komplette XSLT-Template ist im folgenden Listing komplett abgebildet:

**XSLT-Template für die Sitemap-Ausgabe**

~~~~ {.xml}
<?xml version="1.0"?>
<xsl:stylesheet version="1.0"
 xmlns:xsl="http://www.w3.org/1999/XSL/Transform">

<xsl:output method="xml"
            encoding="utf-8"
            standalone="no"
            indent="yes"
            omit-xml-declaration="no" />

<xsl:template match="/page">
  <xsl:variable name="module"
               select="content/topic/@module"/>
  <xsl:choose>
    <xsl:when test="$module = 'content_sitemap'">
      <xsl:call-template name="content_sitemap">
        <xsl:with-param name="topic"
                 select="/page/content/topic" />
      </xsl:call-template>
    </xsl:when>
  </xsl:choose>
</xsl:template>

<xsl:template name="content_sitemap">
  <xsl:param name="topic" />
  <xsl:variable nam"host">
    <xsl:choose>
      <xsl:when test="$topic/host">
        <xsl:text>http://</xsl:text>
        <xsl:value-of select="$topic/host"
          disable-output-escaping="true" />
        <xsl:text>/</xsl:text>
      </xsl:when>
      <xsl:otherwise>
        <xsl:text>/</xsl:text>
      </xsl:otherwise>
    </xsl:choose>
  </xsl:variable>

  <urlset
ns="http://www.google.com/schemas/sitemap/0.84">
    <xsl:call-template name="mapitem">
      <xsl:with-param name="items"
        select="$topic/sitemap/mapitem" />
      <xsl:with-param name="host">
        <xsl:value-of select="$host" />
      </xsl:with-param>
    </xsl:call-template>
  </urlset>
</xsl:template>

<xsl:template name="mapitem">
  <xsl:param name="items" />
  <xsl:param name="host" />
  <xsl:for-each select="$items">
    <url>
      <loc>
        <xsl:value-of select="$host" />
        <xsl:value-of select="@href"
         disable-output-escaping="true" />
      </loc>
      <lastmod>
        <xsl:value-of select="substring(@lastmod,0,11)"
         disable-output-escaping="true" />
      </lastmod>
      <changefreq>
        <xsl:value-of select="@changefreq"
         disable-output-escaping="true" />
      </changefreq>
      <priority>
        <xsl:value-of select="@priority"
         disable-output-escaping="true" />
      </priority>
    </url>
    <xsl:if test="mapitem">
      <xsl:call-template name="mapitem">
       <xsl:with-param name="items"
                       select="mapitem" />
       <xsl:with-param name="host"
                       select="$host" />
      </xsl:call-template>
    </xsl:if>
  </xsl:for-each>
</xsl:template>

</xsl:stylesheet>
~~~~

[Kategorie:Sitemap-Template erstellen](Kategorie:Sitemap-Template_erstellen )