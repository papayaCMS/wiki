---
title: XSLT-Template für die Basisstruktur
permalink: /XSLT-Template_für_die_Basisstruktur/
---

Das Basistemplate `basetemplate` erzeugt das Wurzelelement des RSS-Dokuments und ruft weitere Templates auf, um die `<item>` -Elemente hinzuzufügen. Im Basistemplate werden auch allgemeine Informationen über die Website in das `<channel>` -Element eingefügt, die im Feedreader als Metainformation angezeigt werden. Das `<channel>` -Element enthält auch eine Liste der Links zu den Artikeln, die in den jeweiligen `<item>` -Elementen angeteasert werden.

Das folgende Listing stellt das XSLT-Template mit der Basisstruktur vor:

**XSLT-Template mit der Basisstruktur des RSS-Feeds**

~~~~ {.xml}
...
<xsl:template name="basetemplate">
  <xsl:param name="content"/>
  <xsl:param name="host"/>
  <rdf:RDF xmlns:rdf="http://www.w3.org/1999/02/22-rdf-syntax-ns#"
           ns="http://purl.org/rss/1.0/">
    <channel rdf:about="{$host}{$content/@href}">
      <title>
        <xsl:value-of select="$content/title" />
      </title>
      <link>
        <xsl:value-of select="$host" />
      </link>
      <description>
        <xsl:value-of select="$content/text"/>
      </description>
      <xsl:if test="$content/image/img">
        <image rdf:resource="{$host}{$content/image/img/@src}">
          <title>papaya5</title>
          <link><xsl:value-of select="$host"/></link>
          <url>
            <xsl:value-of select="$host"/>
            <xsl:value-of select="$content/image/img/@src"/>
          </url>
        </image>
      </xsl:if>
      <items>
        <xsl:call-template name="toc.items">
          <xsl:with-param name="host"
                          select="$host"/>
        </xsl:call-template>
      </items>
    </channel>
    <xsl:call-template name="list.items">
      <xsl:with-param name="host"
                      select="$host"/>
    </xsl:call-template>
  </rdf:RDF>
</xsl:template>
...
~~~~

Das Template für die Basisstruktur ruft noch das Hilfstemplate `toc.items` auf, dass das Inhaltsverzeichnis erstellt. Das Hilfstemplate listet die URLs aller `<item>` -Elemente in `<rdf:li>` -Tags auf:

**Hilfstemplate für RSS-Inhaltsverzeichnis**

~~~~ {.xml}
...
<xsl:template name="toc.items">
  <xsl:param name="host"/>
  <rdf:Seq xmlns:rdf="http://www.w3.org/1999/02/22-rdf-syntax-ns#">
    <xsl:for-each select="/page/content/topic/subtopics/subtopic">
      <rdf:li resource="{$host}{@href}" />
    </xsl:for-each>
  </rdf:Seq>
</xsl:template>
...
~~~~

[Kategorie:Template für RSS-Feed erstellen](/Kategorie:Template_für_RSS-Feed_erstellen )