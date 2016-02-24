---
title: Inhalte der Titelseite
permalink: /Inhalte_der_Titelseite/
---

Die Inhalte für die Titelseite werden durch das XSLT-Template mit dem Namen `cover` eingebunden. Das folgende Codebeispiel stellt dieses Template vor:

**XSLT-Template für die Titelseite**

~~~~ {.xml}
...
<xsl:template name="cover">
  <xsl:param name="content"/>
  <element id="title">
    <xsl:call-template name="title">
      <xsl:with-param name="content"
                      select="$content"/>
    </xsl:call-template>
  </element>

  <element id="subtitle">
    <xsl:call-template name="subtitle">
      <xsl:with-param name="content"
                      select="$content"/>
    </xsl:call-template>
  </element>

<!--
  <element id="teaser">
    <xsl:call-template name="teaser">
      <xsl:with-param name="content"
                      select="$content"/>
    </xsl:call-template>
  </element>
-->
<!--
  <element id="image">
    <xsl:call-template name="image">
      <xsl:with-param name="content"
                      select="$content"/>
    </xsl:call-template>
  </element>
-->

</xsl:template>
...
~~~~

Im obigen Beispiel werden der Titel sowie der Untertitel in `<element>` -Tags eingefügt. Über das Attribut `id` können Sie dabei auswählen, welche `<element>` -Definition aus dem Layoutbereich angewendet werden soll. Optional können Sie auch den Teaser sowie ein Bild in die Titelseite setzen, allerdings sind die entsprechenden `<element>` -Tags auskommentiert. Wenn Sie diese Elemente in Ihrem Template benutzen möchten, müssen Sie jedoch entsprechende `<element>` -Definitionen im Layoutbereich vornehmen.

Die Inhalte werden jedoch nicht direkt eingebunden. Stattdessen werden Hilfstemplates aufgerufen, um den Titel, Untertitel und ggf. auch den Teaser und das Bild einzubinden. Näheres zu diesen Hilfstemplates erfahren Sie in [Inhalte für den Fließtext](/Inhalte_für_den_Fließtext "wikilink").

[Kategorie:PDF-Template schreiben](/Kategorie:PDF-Template_schreiben "wikilink")