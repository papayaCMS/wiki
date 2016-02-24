---
title: Druckfreundliches CSS einbinden
permalink: /Druckfreundliches_CSS_einbinden/
---

Sie erzeugen zuerst eine XSLT-Datei im Unterverzeichnis `./print` Ihres Template-Verzeichnisses. Sie können diese Datei beispielswiese `page_main_print.xsl` nennen:

**Print-Template anlegen**

~~~~ {.xml}
<?xml version="1.0"?>
<xsl:stylesheet version="1.0" xmlns:xsl="http://www.w3.org/1999/XSL/Transform" ns="http://www.w3.org/1999/xhtml">

<!-- @papaya:modules content_imgtopic -->

...
~~~~

Anschließend importieren Sie die Hauptdatei aus dem `./html` Unterverzeichnis:

**./html/page_main.xsl importieren**

~~~~ {.xml}
...
<xsl:import href="../html/page_main.xsl" />
...
~~~~

Schließlich definieren Sie noch einige Parameter um, damit die Navigationsspalte sowie die rechte Content-Spalte "Additional" ausgeblendet wird:

**Navigationsspalte und Spalte Additional ausblenden**

~~~~ {.xml}
...
<xsl:param name="DISABLE_NAVIGATION_COLUMN" select="true()"/>
<xsl:param name="DISABLE_ADDITIONAL_COLUMN" select="true()"/>
...
~~~~

Schließlich überladen Sie noch das Template `papaya-styles`, das die Haupt-CSS-Dateien einbindet:

**papaya-styles überladen**

~~~~ {.xml}
...
<xsl:template name="papaya-styles">
  <xsl:variable name="revisionQueryString" select="concat('?', $PAGE_WEBSITE_REVISION)" />
  <xsl:variable name="revisionQueryParam" select="concat('&', $PAGE_WEBSITE_REVISION)" />

  <!-- Das Print-Template wird im folgenden Element eingebunden: -->

  <link rel="stylesheet" type="text/css" href="{$PAGE_THEME_PATH}print.css{$revisionQueryString}"
    media="screen, projection, print, emboss" />
  <xsl:text disable-output-escaping="yes"><!--[if IE 7]></xsl:text>
  <link rel="stylesheet" type="text/css" href="{$PAGE_THEME_PATH}ie7.css{$revisionQueryString}"/>
  <xsl:text disable-output-escaping="yes"><![endif]--></xsl:text>
  <xsl:text disable-output-escaping="yes"><!--[if lt IE 7]></xsl:text>
  <link rel="stylesheet" type="text/css" href="{$PAGE_THEME_PATH}ie6win.css{$revisionQueryString}"/>
  <xsl:text disable-output-escaping="yes"><![endif]--></xsl:text>
</xsl:template>
...
~~~~

Im media-Attribut für das Print-Template `print.css` gegben Sie neben `screen` und `projection` noch `print` und `emboss` an.

Links können zudem für die Druckausgabe so formatiert werden, dass hinter dem Linktitel noch die URL in Klammern ausgegeben wird:

**LInks für die Druckausgabe formatieren**

~~~~ {.xml}
...
<xsl:template match="a[@href]">
  <a href="{@href}">
    <xsl:value-of select="."/>
    <xsl:text> (</xsl:text>
    <xsl:value-of select="$PAGE_BASE_URL"/>
    <xsl:value-of select="@href"/>
    <xsl:text>)</xsl:text>
  </a>
</xsl:template>
...
~~~~

Das komplette Stylesheet sieht wie folgt aus:

**Stylesheet für Druckvorschau**

~~~~ {.xml}
<?xml version="1.0"?>
<xsl:stylesheet version="1.0"
                xmlns:xsl="http://www.w3.org/1999/XSL/Transform"
                ns="http://www.w3.org/1999/xhtml">

<!-- @papaya:modules content_imgtopic -->

<xsl:import href="../html/page_main.xsl" />

<xsl:param name="DISABLE_NAVIGATION_COLUMN" select="true()"/>
<xsl:param name="DISABLE_ADDITIONAL_COLUMN" select="true()"/>

<xsl:template name="papaya-styles">
  <xsl:variable name="revisionQueryString" select="concat('?', $PAGE_WEBSITE_REVISION)" />
  <xsl:variable name="revisionQueryParam" select="concat('&', $PAGE_WEBSITE_REVISION)" />
  <link rel="stylesheet" type="text/css" href="{$PAGE_THEME_PATH}print.css{$revisionQueryString}"
        media="screen, projection, print, emboss" />
  <xsl:text disable-output-escaping="yes"><!--[if IE 7]></xsl:text>
  <link rel="stylesheet" type="text/css" href="{$PAGE_THEME_PATH}ie7.css{$revisionQueryString}"/>
  <xsl:text disable-output-escaping="yes"><![endif]--></xsl:text>
  <xsl:text disable-output-escaping="yes"><!--[if lt IE 7]></xsl:text>
  <link rel="stylesheet" type="text/css" href="{$PAGE_THEME_PATH}ie6win.css{$revisionQueryString}"/>
  <xsl:text disable-output-escaping="yes"><![endif]--></xsl:text>
</xsl:template>

<xsl:template match="a[@href]">
  <a href="{@href}">
    <xsl:value-of select="."/>
    <xsl:text> (</xsl:text>
    <xsl:value-of select="$PAGE_BASE_URL"/>
    <xsl:value-of select="@href"/>
    <xsl:text>)</xsl:text>
  </a>
</xsl:template>

</xsl:stylesheet>
~~~~

Ebenso wie `page_main.xsl` erzeugt `page_main_print.xsl` den HTML-Seitenbaum. Der wesentliche Unterschied ist jedoch der, dass dafür das HTML-Template importiert wird. Bestimmte Parameter werden anschließend überladen, um die rechte und linke Spalte im Content-Bereich zu deaktivieren. Anschließend wird das Template `papaya-styles` überladen, um nur die für die Printausgabe relevanten Templates zu laden. Dazu wird die CSS-Datei print.css geladen, die im media-Attribut noch die Werte `screen`, `projection`, `print` und `amboss` erhält.

[Kategorie:Print-Templates schreiben](/Kategorie:Print-Templates_schreiben "wikilink")