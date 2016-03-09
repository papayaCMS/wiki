---
title: Starttemplate erzeugen
permalink: /Starttemplate_erzeugen/
---

Das Starttemplate ist das einzige Template mit dem `match` -Attribut. Das Muster enthält als Pfadangabe das Wurzelelement „/“. In diesem Template wird überprüft, ob das verwendete Seitenmodul vom Typ „export_de/Kategorie with image“ oder „Tag export_de/Kategorie“ ist, da nur diese Module ein Seiten-XML mit `<subtopic>` -Elementen ausliefern. Das folgende Listing stellt das XSLT-Template vor:

**Starttemplate mit match auf „/“**

~~~~ {.xml}
...
<xsl:template match="/">
  <xsl:variable name="host" select="$PAGE_BASE_URL"/>
  <xsl:variable name="content" select="page/content/topic"/>
  <xsl:choose>
    <xsl:when test="$content/@module = 'content_categimg' or
     $content/@module = 'content_tagcateg'">
      <xsl:call-template name="basetemplate">
        <xsl:with-param name="content" select="$content"/>
        <xsl:with-param name="host"    select="$host"/>
      </xsl:call-template>
    </xsl:when>
  </xsl:choose>
</xsl:template>
...
~~~~

Das Basistemplate `basetemplate` wird nur dann aufgerufen, wenn das Seiten-XML durch das richtige Modul geliefert worden ist. Andernfalls wird eine leere Seite ausgeliefert. Über den Parameter `$PAGE_BASE_URL` wird die Basis-URL in die Variable `$host` eingelesen. Näheres zu diesen Parametern erfahren Sie in [Parameter in XSLT-Stylesheets](/Parameter_in_XSLT-Stylesheets ).

[export_de/Kategorie:Template für RSS-Feed erstellen](export_de/Kategorie:Template_für_RSS-Feed_erstellen )