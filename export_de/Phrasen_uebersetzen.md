
Wenn Sie in Ihrem Stylesheet Phrasen übersetzen möchten, gehen Sie wie folgt vor:

1.  Importieren Sie die Templates für die Phrasenübersetzung. Diesen Schritt müssen Sie nicht durchführen, wenn Sie die `page_main.xsl` in Ihr Stylesheet importieren: 

**Templates für Phrasen importieren**

    ~~~~ {.xml}
    ..
    <xsl:import href="../_lang/language.xsl" />
    ...
    ~~~~

2.  Laden Sie die gewünschte Übersetzungsdatei in den Standard-Parameter (siehe [Templates und Parameter in ./html/page_main.xsl](Templates_und_Parameter_in_./html/page_main.xsl.md).):

`**Übersetzungsdatei in Standardparameter laden**

    ~~~~ {.xml}
    ...
    <!-- Datei mit den Übersetzungen in der aktuellen Content-Sprache -->
    <xsl:param name="LANGUAGE_MODULE_CURRENT"
               select="document(concat($PAGE_LANGUAGE'.xml'))" />

    <!-- Alternativdatei für den Fall, dass $LANGUAGE_MODULE_CURRENT
         nicht geladen werden kann. -->
    <xsl:param name="LANGUAGE_MODULE_FALLBACK"
               select="document('en-US.xml')"/>
    ...
    ~~~~

3.  Übersetzen Sie die Phrase mit dem Template `language-text`. Sie übergeben diesem Template die Phrase über den Parameter `text`: 

**Phrase mit getText-Template übersetzen.**

    ~~~~ {.xml}
    ...
    <xsl:if test="$item/@href and $item/@href != ''">
      <a href="{$item/@href}" class="more">
        <xsl:call-template name="language-text">
          <xsl:with-param name="text">MORE</xsl:with-param>
        </xsl:call-template>
      </a>
    </xsl:if>
    ...
    ~~~~

    Das Template gibt für die Phrase schließlich die Übersetzung zurück, wenn die Phrase gefunden worden ist. Andernfalls wird einfach die nicht übersetzte Phrase zurückgegeben.

[Kategorie:Übersetzungstemplate benutzen](export_de/Kategorie:Übersetzungstemplate_benutzen.md)
