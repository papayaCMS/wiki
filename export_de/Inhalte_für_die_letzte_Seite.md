
Für die Schluss-Seite können Sie optional ein XSLT-Template schreiben, das mit der Anweisung `call-template` aufgerufen werden kann. Im unten vorgestellten Beispiel heißt dieses Template `final`:

**XSLT-Template für die Schluss-Seite**

~~~~ {.xml}
...
<xsl:template name="final">
  <xsl:param name="content" />
  <element id="finalStuff">
    <!-- Call template that inserts legal notice -->
    <xsl:call-template name="legalNotice"/>
  </element>
</xsl:template>
...
~~~~

Das oben dargestellte Template für die Schluss-Seite können Sie dazu benutzen, um einen Text über Urheber- und Nutzungsrechte einzufügen. Sie können jedoch diesen Text auch direkt in die PDF-Musterdatei einbauen, sodass Sie nicht für jeden Artikel extra eine Seite mit den selben Inhalten einfügen müssen.

[Kategorie:PDF-Template schreiben](export_de/Kategorie:PDF-Template_schreiben.md)