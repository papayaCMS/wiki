
Neben den Templates, die die Struktur des papaya-Formatierungsobjektes aufbauen, enthält das Demotemplate einige XSLT-Templates für Inline- und für Blockelemente. Im Folgenden sind die XSLT-Templates für die Inline-Elemente dargestellt:

**Templates für Inline-Elemente**

~~~~ {.xml}
...
<xsl:template match="strong">
  <b><xsl:apply-templates /></b>
</xsl:template>

<xsl:template match="em">
  <i><xsl:apply-templates /></i>
</xsl:template>

<xsl:template match="br|p|b|i">
  <xsl:element name="{local-name()}">
    <xsl:copy-of select="@*"/>
    <xsl:apply-templates select="node()" />
  </xsl:element>
</xsl:template>
...
~~~~

Im obigen Code-Beispiel werden einfache Elemente wie `<br/>`, `<b>`, `<p>` und `<i>` durchgeschleift, während `<strong>` in `<b>` und `<em>` in `<i>` umgewandelt wird.

Das XSLT-Template mit dem Match auf das Ankerelement `<a>` wählt alle Elemente mit dem `href` -Attribut aus und setzt den Textknoten in die Ausgabe. Hinter die Ausgabe wird der Wert des `href` -Attributs in eckige Klammern gesetzt:

**Anker-Element**

~~~~ {.xml}
...
<xsl:template match="a[@href]">
  <xsl:apply-templates select="node()" />
  <xsl:text> [</xsl:text>
    <xsl:apply-templates select="@href"/>
  <xsl:text>]</xsl:text>
</xsl:template>
...
~~~~

Block-Level-Elemente

Die XSLT-Templates für Block-Level-Elemente setzen nur diejenigen HTML-Elemente aus dem Quelldokument in das Zieldokument ein, die durch den PDF-Ausgabefilter von papaya CMS unterstützt werden. Dabei werden `<table>` -Elemente, die sich innerhalb von Tabellen befinden, einfach ignoriert:

**Templates für Block-Level-Elemente**

~~~~ {.xml}
...
<xsl:template match="img">
  <xsl:element name="{local-name()}">
    <xsl:copy-of select="@src"/>
  </xsl:element>
</xsl:template>

<xsl:template match="table">
  <xsl:element name="{local-name()}">
    <xsl:copy-of select="@*[name() != 'align']"/>
    <xsl:attribute name="align">left</xsl:attribute>
    <xsl:apply-templates select="./tr|./*/tr"
                         mode="in-table"/>
  </xsl:element>
</xsl:template>

<xsl:template match="table" mode="in-table">
</xsl:template>

<xsl:template match="tr|td" mode="in-table">
  <xsl:element name="{local-name()}">
    <xsl:copy-of select="@*"/>
    <xsl:apply-templates select="node()"
                         mode="in-table"/>
  </xsl:element>
</xsl:template>

<xsl:template match="th" mode="in-table">
  <xsl:element name="{local-name()}">
    <xsl:copy-of select="@*[name() != 'align']"/>
    <xsl:attribute name="align">center</xsl:attribute>
    <b><xsl:apply-templates select="node()"
                            mode="in-table"/></b>
  </xsl:element>
</xsl:template>
...
~~~~

Bei den XSLT-Templates für ungeordnete Listen wird mit dem Attribut `bullet-chars` ein bestimmtes Symbol ausgewählt, das als Aufzählungszeichen benutzt wird. Die Elementknoten wie `<ol>` und `<li>` werden hingegen einfach neu eingefügt, sodass mögliche Attribute aus dem Quelldokument nicht mehr im Zieldokument (das papaya-Formatierungsobjekt) auftauchen können:

**XSLT-Templates für Listen**

~~~~ {.xml}
...
<xsl:template match="ul">
  <ul bullet-chars="">
    <xsl:for-each select="li">
      <li><xsl:value-of select="." /></li>
    </xsl:for-each>
  </ul>
</xsl:template>

<xsl:template match="ol">
  <ol>
    <xsl:for-each select="li">
      <li><xsl:value-of select="." /></li>
    </xsl:for-each>
  </ol>
</xsl:template>
...
~~~~

Die Elemente für Überschriften der ersten, zweiten und dritten Ordnung werden in das Zieldokument übernommen, indem einfach ein neues Element mit dem selben lokalen Elementnamen erzeugt wird. Da die Elemente für Überschriften unbedingt das `align` -Attribut benötigen, wird bei fehlendem `align` im Quelldokument ein `align` -Attribut mit dem Wert „left“ eingefügt. Der Textknoten des Elements wird schließlich durch die `apply-templates` -Anweisung an die XSLT-Templates mit passendem `match` -Muster weitergereicht.

**XSLT-Templates für HTML-Überschriften**

~~~~ {.xml}
...
<xsl:template match="h1|h2|h3">
  <xsl:element name="{local-name()}">
    <xsl:if test="not(@align)">
      <xsl:attribute name="align">left</xsl:attribute>
    </xsl:if>
    <xsl:copy-of select="@*"/>
    <xsl:apply-templates select="node()" />
  </xsl:element>
</xsl:template>
...
~~~~

[Kategorie:PDF-Template schreiben](export_de/Kategorie:PDF-Template_schreiben.md)