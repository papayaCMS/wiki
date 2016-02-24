---
title: XSLT-Templates für die Linkleiste schreiben
permalink: /XSLT-Templates_für_die_Linkleiste_schreiben/
---

Um die verschiedenen Ausgabemodi leicht zugänglich zu machen, können Sie oberhalb des Artikels eine Linkleiste darstellen. Mit dieser Linkleiste kann der Nutzer je nach Bedarf eine entsprechende Ansicht aufrufen, um sich die druckfreundliche Ausgabe oder die PDF-Version der Seite anzuschauen. Zu diesem Zweck können Sie im Hauptstylesheet eine Toolbar erzeugen:

**Toolbar erzeugen**

~~~~ {.xml}
...
...
<xsl:template name="link.toolbar">
  <ul class="toolbar">
    <xsl:call-template name="print.view" />
    <xsl:call-template name="pdf.view" />
    <xsl:call-template name="rss.view" />
    <!-- Rufe andere Templates auf, die
         weitere Funktionen zur Verfügung
         stellen.                       -->
  </ul>
</xsl:template>
...
~~~~

Sie können das XSLT-Template `link.toolbar` sowohl über als auch unter den Artikeltext einbinden. Der Nutzer Ihrer Website kann dadurch bei längeren Artikeln jederzeit die entsprechende Funktion erreichen, um die Ansicht umzuschalten.

Das Template `link.toolbar` erzeugt eine Leiste mit Links, die verschiedene Funktionen für den Nutzer zur Verfügung stellt. Der Nutzer kann sich den aktuellen Artikel als PDF-Datei herunterladen

Druckfreundliche Ausgabe verlinken
----------------------------------

Damit die Nutzer Ihrer Webseiten auf einfache Weise in die druckfreundliche Ausgabe umschalten können, sollten Sie in Ihre Standardseiten entsprechende Links einfügen. Zu diesem Zweck können Sie ein entsprechendes XSLT-Template schreiben, das die gewünschte Print-Ausgabe als Link darstellt:

**Print-Ausgabe in Webausgabe verlinken**

~~~~ {.xml}
...
<xsl:template name="print.view">
  <xsl:variable name="printview"
                select="/page/views/viewmode[@ext = 'print']" />
  <xsl:if test="$printview">
    <li>
      <a href="{$printview/@href}">Druckansicht</a>
    </li>
  </xsl:if>
</xsl:template>
...
~~~~

Das Template `print.view` überprüft, ob die Ansicht für die druckfreundliche Ausgabe mit der Seite verknüpft ist. Falls dies der Fall ist, wird ein entsprechender Link erzeugt. Dabei müssen Sie den passenden Link nicht umständlich selbst erstellen, da er bereits im Seiten-XML als `href` -Attribut des `<viewmode>` -Elements enthalten ist. Sie können ihn also einfach in die Ausgabe „durchschleifen“.

PDF-Ausgabe verlinken
---------------------

Sie binden die PDF-Ausgabe auf die gleiche Art und Weise ein, mit der Sie die Print-Ausgabe eingebunden haben:

**PDF-Ausgabe verlinken**

~~~~ {.xml}
...
<xsl:template name="pdf.view">
  <xsl:variable name="pdfview"
                select="/page/views/viewmode[@ext = 'pdf']" />
  <xsl:if test="$pdfview">
    <li>
      <a href="{$pdfview/@href}">PDF-Export</a>
    </li>
  </xsl:if>
</xsl:template>
...
~~~~

RSS-Ausgabe verlinken
---------------------

Auf einigen Übersichtsseiten können Sie zusätzlich RSS-Feeds als Links ausgeben. Mit diesen Feeds haben interessierte Nutzer die Möglichkeit, über Aktualisierungen ständig auf dem Laufenden zu bleiben. Dieser Link wird dabei auch nur bei den Seiten verlinkt, deren Ansicht mit dem RSS-Ausgabefilter verknüpft worden ist.

**RSS-Ausgabe verlinken**

~~~~ {.xml}
...
<xsl:template name="rss.view">
  <xsl:variable name="rssview"
                select="/page/views/viewmode[@ext = 'rss']"/>
  <xsl:if test="$rssview">
    <li>
      <a href="{$rssview/@href}">RSS-Feed</a>
    </li>
  </xsl:if>
</xsl:template>
...
~~~~

Komplette XSLT-Stylesheetdatei
------------------------------

Alle Templates für die Toolbar können Sie in einer einzigen Stylesheetdatei zusammenfassen und anschließend in jedes gewünschte Template per `<xsl:import>` -Anweisung einbinden. Die komplette XSLT-Stylesheetdatei ist im folgenden Listing abgebildet:

**Komplettes Toolbar-Stylesheet**

~~~~ {.xml}
<?xml version="1.0"?>
<xsl:stylesheet version="1.0"
                xmlns:xsl="http://www.w3.org/1999/XSL/Transform"
                ns="http://www.w3.org/TR/REC-html40">

<xsl:template name="link.toolbar">
  <ul class="toolbar">
    <xsl:call-template name="print.view" />
    <xsl:call-template name="pdf.view" />
    <xsl:call-template name="rss.view" />
    <!-- Rufe andere Templates auf, die
         weitere Funktionen zur Verfügung
         stellen.                       -->
  </ul>
</xsl:template>

<xsl:template name="print.view">
  <xsl:variable name="printview"
                select="/page/views/viewmode[@ext = 'print']" />
  <xsl:if test="$printview">
    <li>
      <a href="{$printview/@href}">Druckansicht</a>
    </li>
  </xsl:if>
</xsl:template>

<xsl:template name="pdf.view">
  <xsl:variable name="pdfview"
                select="/page/views/viewmode[@ext = 'pdf']" />
  <xsl:if test="$pdfview">
    <li>
      <a href="{$pdfview/@href}">PDF-Export</a>
    </li>
  </xsl:if>
</xsl:template>

<xsl:template name="rss.view">
  <xsl:variable name="rssview"
                select="/page/views/viewmode[@ext = 'rss']"/>
  <xsl:if test="$rssview">
    <li>
      <a href="{$rssview/@href}">RSS-Feed</a>
    </li>
  </xsl:if>
</xsl:template>

</xsl:stylesheet>
~~~~

Im folgenden Abschnitt können Sie erfahren, wie Sie die Stylesheet-Datei im Demo-Template einbinden können.

[Kategorie:Ausgaben verlinken](/Kategorie:Ausgaben_verlinken "wikilink")