---
title: Inhalte für den Fließtext
permalink: /Inhalte_für_den_Fließtext/
---

Um die Inhalte des Artikels in das papaya-Formatierungsobjekt einzufügen, müssen Sie in der Regel ein XSLT-Template erstellen, dass an die Ausgabe des Seitenmoduls angepasst ist. Im Demo-Template für die PDF-Ausgabe ist ein Template für das Seitenmodul „Topic with image“ enthalten:

**XSLT-Template für das Seitenmodul „Topic with image“**

~~~~ {.xml}
...
<xsl:template name="topic_default">
  <xsl:param name="content"/>

  <section page="default" page-break-before="yes">
    <data>
      <xsl:if test="$content/title">
        <xsl:call-template name="title">
          <xsl:with-param name="content" select="$content"/>
        </xsl:call-template>
        <br />
        <br />
      </xsl:if>

      <xsl:if test="$content/image//img">
        <xsl:apply-templates select="$content/image//img"/>
        <br />
        <br />
      </xsl:if>

      <xsl:if test="$content/subtitle">
        <xsl:call-template name="subtitle">
          <xsl:with-param name="content" select="$content"/>
        </xsl:call-template>
        <br />
        <br />
      </xsl:if>

      <xsl:choose>
        <xsl:when test="$content/text">
          <xsl:call-template name="text">
            <xsl:with-param name="content" select="$content"/>
          </xsl:call-template>
          <br />
          <br />
        </xsl:when>
        <xsl:otherwise>
          <xsl:if test="$content/teaser">
            <xsl:call-template name="teaser">
              <xsl:with-param name="content" select="$content"/>
            </xsl:call-template>
            <br />
            <br />
          </xsl:if>
        </xsl:otherwise>
      </xsl:choose>
    </data>
  </section>
</xsl:template>
...
~~~~

Das Template fügt die Inhalte in ein `<section>` -Element ein. Das `<section>` -Element wird über das Attribut `page` mit der entsprechenden Layoutdefinition aus dem Layoutbereich verknüpft. Über das Attribut `page-break-before="yes"` legen Sie fest, dass vor diesem Element ein Seitenumbruch durchgeführt wird. Dadurch beginnt der Artikel erst auf der zweiten Seite. Wenn Sie bei bestimmten Seitenmodulen mehr als ein `<section>` -Element in das papaya-Formatierungsobjekt einfügen, wird vor jedem neuen Abschnitt ein Seitenumbruch eingefügt. Dadurch beginnen die neuen Kapitel auf einer neuen Seite.

Hilfstemplates für die Inhalte
------------------------------

Aus dem XSLT-Template topic_default werden weitere Hilfstemplates aufgerufen, die die entsprechenden Inhalte wie Titel, Untertitel, Bildreferenz, Artikeltext und optional auch den Teasertext einbinden. Alle diese Inhalte sind im Quelldokument in Elemente wie `<title>` oder `<text>` enthalten und können daher durch einfache `value-of` -Anweisungen in das Zieldokument übernommen werden:

**Hilfstemplates für Titel und Untertitel**

~~~~ {.xml}
...
<xsl:template name="title">
  <b>
    <xsl:param name="content"/>
    <xsl:choose>
      <xsl:when test="$content/title/text() != ''">
        <xsl:value-of select="$content/title"
                      disable-output-escaping="yes"/>
      </xsl:when>
      <xsl:otherwise>
        <xsl:value-of select="$PAGE_TITLE"
                      disable-output-escaping="yes"/>
      </xsl:otherwise>
    </xsl:choose>
  </b>
</xsl:template>

<xsl:template name="subtitle">
  <xsl:param name="content"/>
  <xsl:if test="$content/subtitle/text() != ''">
    <xsl:value-of select="$content/subtitle"
                  disable-output-escaping="yes"/>
  </xsl:if>
</xsl:template>
...
~~~~

Elemente wie `<text>` und `<teaser>` können jedoch selbst weitere Elemente enthalten, die nicht so ohne weiteres in das Zieldokument übernommen werden können. Sie können sowohl Block-Level-Elemente wie `<h1>`, `<table>`, `<ul>` oder `<img>` enthalten oder Inline-Elemente wie `<strong>`. Damit diese Elemente durch die XSLT-Templates mit entsprechenden `match` -Muster bearbeitet werden können, wird die `apply-templates` -Anweisung eingefügt. Näheres zu den XSLT-Templates für Block-Level- und Inline-Elemente erfahren Sie in [Templates für Block-Level- und Inline-Elemente](/Templates_für_Block-Level-_und_Inline-Elemente ). Im folgenden Listing werden die Hilfstemplates für Text, Teaser und Bild dargestellt:

**Hilfstemplates für Text, Teaser und Bild**

~~~~ {.xml}
...
<xsl:template name="text">
  <xsl:param name="content"/>
  <xsl:if test="($content/text/text() != '')
   or (count($content/text/*) > 0)">
    <xsl:apply-templates
     select="$content/text/*|$content/text/text()"/>
  </xsl:if>
</xsl:template>

<xsl:template name="teaser">
  <xsl:param name="content"/>
  <xsl:if test="($content/teaser/text() != '')
   or (count($content/teaser/*) > 0)">
    <xsl:apply-templates
     select="$content/teaser/*|$content/teaser/text()"/>
  </xsl:if>
</xsl:template>

<xsl:template name="image">
  <xsl:param name="content"/>
  <xsl:if test="$content/image//img/@src != ''">
    <xsl:apply-templates select="$content/image//img"/>
  </xsl:if>
</xsl:template>
...
~~~~

[export_de/Kategorie.md:PDF-Template schreiben](export_de/Kategorie.md:PDF-Template_schreiben )