---
title: XSLT-Template für die item-Elemente
permalink: /XSLT-Template_für_die_item-Elemente/
---

Die `<item>` -Elemente stellen die Einträge des RSS-Feeds dar. Sie enthalten den Artikeltitel, den Teaser und den Link zum Artikel auf der Website. Nutzer können anhand des Titels und des Teasers entscheiden, ob sie sich den vollständigen Artikel auf der Website ansehen wollen. Dazu müssen sie lediglich auf den Link klicken. Das folgende Listing stellt Ihnen das XSLT-Template für die `<item>` -Elemente vor:

**XSLT-Template für die <item>-Elemente**

~~~~ {.xml}
...
<xsl:template name="list.items">
  <xsl:param name="host"/>
  <xsl:for-each select="/page/content/topic/subtopics/subtopic">
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
...
~~~~

[export_de/Kategorie.md:Template für RSS-Feed erstellen](export_de/Kategorie.md:Template_für_RSS-Feed_erstellen )