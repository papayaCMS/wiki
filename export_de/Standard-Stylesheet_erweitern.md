
Die XSLT-Templates für die Toolbar können Sie am einfachsten im Standard-Stylesheet `page_general.xsl` einbinden, damit die Toolbar auf möglichst allen Seiten erscheint. Dazu müssen Sie die Stylesheet-Datei mit den Toolbar-Templates importieren:

**Stylesheet-Datei für Toolbar einbinden**

~~~~ {.xml}
...
<xsl:import href="toolbar.xsl"/>
...
~~~~

siehe folgende Abbildung:

**Auszug aus page_general.xsl**

~~~~ {.xml}
...
<xsl:template match="page">
<html lang="{$PAGE_LANGUAGE}">
  <head>
    ...
  </head>
<body>
    <div id="content">
      <xsl:if test="count(boxes/box[@group = 'right']) = 0">
        <xsl:attribute name="class">largeContent</xsl:attribute>
      </xsl:if>
      <xsl:call-template name="link.toolbar" />
      <xsl:call-template name="content_area"/>
      <div id="footer">
        powered by <a
          href="http://www.papaya-cms.com/">papaya CMS</a>
      </div>
    </div>
...
~~~~

Das Template `link.toolbar` wird im obigen Code-Ausschnitt zwei Mal aufgerufen: vor und nach dem Template `content_area`. Da `content_area` in allen Stylesheets für die Seitenmodule überschrieben wird, erscheint die Toolbar dadurch auf allen Seiten unabhängig vom Seitenmodul.

[Kategorie:Ausgaben verlinken](../export_de/Kategorie:Ausgaben_verlinken.md)