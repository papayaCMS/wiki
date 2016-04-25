
Im folgenden Listing ist das vollständige XSLT-Stylesheet dargestellt:

**Vollständiges XSLT-Stylesheet**

~~~~ {.xml}
...
<?xml version="1.0"?>

<xsl:stylesheet version="1.0"
                xmlns:xsl="http://www.w3.org/1999/XSL/Transform">

<xsl:output method="xml"
            encoding="utf-8"
            standalone="yes"
            indent="yes"
            omit-xml-declaration="no" />

<!-- Params -->
<xsl:param name="PAGE_THEME_PATH" />
<xsl:param name="PAGE_WEB_PATH" />
<xsl:param name="PAGE_TITLE" />
<xsl:param name="PAGE_BASE_URL" />

<xsl:template match="/">
  <xsl:variable name="host" select="$PAGE_BASE_URL"/>
  <xsl:variable name="content" select="page/content/topic"/>
  <xsl:choose>
    <xsl:when test="$content/@module = 'content_categimg'
     or $content/@module = 'content_tagcateg'">
      <xsl:call-template name="basetemplate">
        <xsl:with-param name="content"
                        select="$content"/>
        <xsl:with-param name="host"
                        select="$host"/>
      </xsl:call-template>
    </xsl:when>
  </xsl:choose>
</xsl:template>

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
          <xsl:with-param name="host" select="$host"/>
        </xsl:call-template>
      </items>
    </channel>
    <xsl:call-template name="list.items">
      <xsl:with-param name="host" select="$host"/>
    </xsl:call-template>
  </rdf:RDF>
</xsl:template>

<xsl:template name="toc.items">
  <xsl:param name="host"/>
  <rdf:Seq
   xmlns:rdf="http://www.w3.org/1999/02/22-rdf-syntax-ns#">
    <xsl:for-each
     select="/page/content/topic/subtopics/subtopic">
      <rdf:li resource="{$host}{@href}"/>
    </xsl:for-each>
  </rdf:Seq>
</xsl:template>

<xsl:template name="list.items">
  <xsl:param name="host"/>
  <xsl:for-each
   select="/page/content/topic/subtopics/subtopic">
    <item
     xmlns:rdf="http://www.w3.org/1999/02/22-rdf-syntax-ns#"
     rdf:about="{$host}{@href}">
      <title>
        <xsl:value-of select="title"/>
      </title>
      <link>
        <xsl:value-of select="$host"/>
        <xsl:value-of select="@href"/>
      </link>
      <description>
        <xsl:value-of select="text"/>
      </description>
    </item>
  </xsl:for-each>
</xsl:template>

</xsl:stylesheet>
~~~~

[Kategorie:Template für RSS-Feed erstellen](../export_de/Kategorie:Template_für_RSS-Feed_erstellen.md)