
Die Inhalte für die Fußzeile werden über das XSLT-Template `footer` eingefügt. Das folgende XSLT-Fragment stellt dieses Template kurz vor:

**XSLT-Template „footer“ mit den Inhalten der Fußzeile**

~~~~ {.xml}
...
<xsl:template name="footer">
  <xsl:param name="content"/>
  <xsl:if test="$content/@author!=''">
    <xsl:choose>
      <xsl:when test="$PAGE_LANGUAGE = 'de-DE'">
        <xsl:text>Autor: </xsl:text>
      </xsl:when>
      <xsl:otherwise>
        <xsl:text>Author: </xsl:text>
      </xsl:otherwise>
    </xsl:choose>
    <xsl:value-of select="$content/@author" />
    <xsl:if test="$content/@published or $content/@created != ''">
      <xsl:text> | </xsl:text>
    </xsl:if>
  </xsl:if>
  <xsl:if test="$content/@published != ''">
    <xsl:choose>
      <xsl:when test="$PAGE_LANGUAGE = 'de-DE'">
        <xsl:text>Erstellt am: </xsl:text>
      </xsl:when>
      <xsl:otherwise>
        <xsl:text>Created at: </xsl:text>
      </xsl:otherwise>
    </xsl:choose>
    <xsl:call-template name="formatShortDate">
       <xsl:with-param name="isodate"
                       select="$content/@created"/>
    </xsl:call-template>
    <xsl:if test="$content/@created != ''">
      <xsl:text> | </xsl:text>
    </xsl:if>
  </xsl:if>
  <xsl:if test="$content/@created != ''">
    <xsl:choose>
      <xsl:when test="$PAGE_LANGUAGE = 'de-DE'">
        <xsl:text>Letzte Aktualisierung: </xsl:text>
      </xsl:when>
      <xsl:otherwise>
        <xsl:text>Last update: </xsl:text>
      </xsl:otherwise>
    </xsl:choose>
    <xsl:call-template name="formatShortDate">
       <xsl:with-param name="isodate"
                       select="$content/@published"/>
    </xsl:call-template>
  </xsl:if>
</xsl:template>
...
~~~~

Das Template fügt einfach den Namen des Autors, das Datum der Veröffentlichung sowie das der letzten Änderung in die Fußzeile ein. Diese Inhalte werden dabei nur dann in das papaya-Formatierungsobjekt geschrieben, wenn die entsprechenden Daten im Quelldokument enthalten sind.

[Kategorie:PDF-Template schreiben](export_de/Kategorie:PDF-Template_schreiben.md)