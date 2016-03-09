---
title: Zentrales Template für den Inhaltsbereich
permalink: /Zentrales_Template_für_den_Inhaltsbereich/
---

Das zentrale Template für den Inhaltsbereich hat den Namen `content_area` und wird im Template für das Basisdokument aufgerufen, siehe [Basisdokument erzeugen](/Basisdokument_erzeugen ). Es enthält die Elemente `<cover>`, `<content>` und `<footer>`. Für alle diese Elemente werden weitere Templates aufgerufen, in denen die Inhalte eingefügt werden. Das folgende XSLT-Fragment zeigt das zentrale Template:

**XSLT-Template für den Inhaltsbereich**

~~~~ {.xml}
...
<xsl:template name="content_area">
  <xsl:variable name="module" select="/page/content/topic/@module"/>
  <xsl:variable name="content" select="/page/content/topic"/>

  <cover>
    <xsl:call-template name="cover">
      <xsl:with-param name="content" select="$content"/>
    </xsl:call-template>
  </cover>

  <content>
    <xsl:choose>
      <!-- Hook your customized module handlers here. -->
      <xsl:when test="$module = ''">
      </xsl:when>
      <!-- Anything else will be parsed using the topic_default template. -->
      <xsl:otherwise>
        <xsl:call-template name="topic_default">
          <xsl:with-param name="content" select="$content"/>
        </xsl:call-template>
      </xsl:otherwise>
    </xsl:choose>
  </content>

  <footer>
    <xsl:call-template name="footer">
      <xsl:with-param name="content" select="$content"/>
    </xsl:call-template>
  </footer>

  <!--<final>
  </final>-->
</xsl:template>
...
~~~~

Folgende Templates werden im obigen Beispiel aufgerufen:

1.  `cover`: Template für die Inhalte der Titelseite.
2.  `topic_default`: Template für das Seitenmodul „Topic with image“.
3.  `footer`: Template für die Fußzeile.
4.  `final`: Template für die letzte Seite.

Bereich <cover>
---------------

Im Bereich <cover> wird das XSLT-Template aufgerufen, das die Inhalte für die Titelseite in das papaya-Formatierungsobjekt setzt. Näheres zu diesem Template erfahren Sie in [Inhalte der Titelseite](/Inhalte_der_Titelseite ).

Bereich <content>
-----------------

Im Bereich `<content>` wird ähnlich wie in den Stylesheets für die HTML-Vorlage ein angepasstes XSLT-Template für das Seitenmodul aufgerufen. Im obigen Beispiel handelt es sich um das Template `topic_default`, das die Inhalte des Seitenmoduls „Topic with image“ in das papaya-Formatierungsobjekt einfügen kann. Näheres zu diesem Template erfahren Sie in [Inhalte für den Fließtext](/Inhalte_für_den_Fließtext ).

==Bereich

<footer>
== Im Bereich `<footer>` wird das XSLT-Template aufgerufen, das die Inhalte für die Fußzeile setzt. Näheres zu diesem Template erfahren Sie in [Inhalt der Fußzeile](/Inhalt_der_Fußzeile ).

[Kategorie:PDF-Template schreiben](Kategorie:PDF-Template_schreiben )