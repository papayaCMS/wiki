---
title: XSLT-Templates für weitere Seitenmodule erstellen
permalink: /XSLT-Templates_für_weitere_Seitenmodule_erstellen/
---

Sie brauchen für jedes Seitenmodul lediglich die zentrale Stylesheet-Datei `page_main.xsl` zu importieren und das Template `content-area` zu überschreiben. Dies soll anhand der Stylesheetdatei `page_sitemap.xsl` für das Seitenmodul „Sitemap“ aus dem Demo-Template demonstriert werden:

1.  Erstellen Sie im Verzeichnis `mein-Template/html/` die Datei `page_sitemap.xsl`. Sie können natürlich auch jede andere Bezeichnung wählen, jedoch sollten Sie sich dabei an die Namenskonvention halten und für Seitentemplates das Präfix `page_` beibehalten.
2.  Erzeugen Sie das XSLT-Grundgerüst: **XSLT-Grundgerüst erzeugen**
    ~~~~ {.xml}
    <?xml version="1.0"?>
    <xsl:stylesheet version="1.0"
       xmlns:xsl="http://www.w3.org/1999/XSL/Transform">

    <xsl:template name="content-area">
    ...
    </xsl:template>

    <xsl:template name="module-content-sitemap">
    ...
    </xsl:template>

    <xsl:template name="page-styles">
    ...
    </xsl:template>

    </xsl:stylesheet>
    ~~~~

    Da im nächsten Schritt die zentrale Stylesheet-Datei importiert wird, sollten Sie die entsprechenden Templates `content-area` und `page-styles` überschreiben: Erstellen Sie das Template mit dem Namen `content-area`. Mit diesem Template wird der XML-Output des Sitemap-Moduls in das HTML-Format transformiert und an die richtige Stelle im HTML-Grundgerüst eingefügt. Erstellen Sie das Template mit dem Namen `page-styles`. Mit diesem Template können Sie CSS-Dateien mit modulspezifischen CSS-Formaten als Verknüpfung in den `<head>` -Bereich einfügen.

3.  Im nächsten Schritt fügen Sie die Importdeklarationen ein: **Import-Deklarationen in die Stylesheet-Datei einfügen**
    ~~~~ {.xml}
    ...
    <xsl:import href="./page_main.xsl"/>
    ...
    ~~~~

    Importieren Sie die XSLT-Datei `page_main.xsl`, in dem das HTML-Grundgerüst der Seite definiert ist. Dieses Stylesheet importiert wiederum `./base/defaults.xsl`, in dem die beiden Templates `content-area` und `page-styles` definiert sind.

4.  Fügen Sie die XSLT-Parameter ein, die Sie für Ihr Template abändern möchten. Einige dieser Parameter sind im XSLT-Framework des Demotemplates, insbesondere in der Datei `page_main.xsl`, mit bestimmten Werten vorbelegt, die Sie ändern können. **XSLT-Parameter einfügen**
    ~~~~ {.xml}
    ...
    <!-- disable the additional content column, even if the xml contains boxes for it -->
    <xsl:param name="DISABLE_ADDITIONAL_COLUMN" select="true()" />
    ...
    ~~~~

    Für dieses Beispiel soll der Parameter DISABLE_ADDITIONAL_COLUMN eingefügt und auf `true()` (Standard: `false()` ) gesetzt werden. Dadurch wird die rechte Spalte im Seitenlayout ausgeblendet, damit mehr Platz für die grafische Sitemap bleibt. Näheres zu den Parametern aus dem Stylesheet in `page_main.xsl` erfahren Sie in [Templates und Parameter in ./html/page_main.xsl](/Templates_und_Parameter_in_./html/page_main.xsl ).

5.  Schreiben Sie nun das Template `page-styles` aus: **Zusätzliche CSS-Datei als externe Ressource verlinken**
    ~~~~ {.xml}
    ...
    <xsl:template name="page-styles">
      <xsl:call-template name="link-style">
        <xsl:with-param name="file">style_sitemap.css</xsl:with-param>
      </xsl:call-template>
    </xsl:template>
    ...
    ~~~~

    Rufen Sie einfach das Template `link-style` auf und übergeben Sie für den Parameter `file` den Namen der CSS-Datei. Das Template bindet anschließend das Stylesheet in die Seite ein, ohne dass Sie sich um Pfadangaben oder ähnliches zu kümmern brauchen. Voraussetzung dafür ist freilich, dass Sie die CSS-Dateien alle direkt im Basisverzeichnis Ihres Themes gespeichert haben.

6.  Schreiben Sie nun das Template `content-area` aus: **Template für den Content-Bereich ausschreiben**
    ~~~~ {.xml}
    ...
    <xsl:template name="content-area">
      <xsl:param name="pageContent" select="content/topic"/>
      <xsl:choose>
        <xsl:when test="$pageContent/@module = 'content_sitemap'">
          <xsl:call-template name="module-content-sitemap">
            <xsl:with-param name="pageContent" select="$pageContent"/>
          </xsl:call-template>
        </xsl:when>
        <xsl:otherwise>
          <xsl:call-template name="module-content-default"/>
        </xsl:otherwise>
      </xsl:choose>
    </xsl:template>
    ...
    ~~~~

    Das Template für den Content-Bereich enthält eine Weiche mit einem Modultest. Wenn das Seitenmodul unterstützt wird, ruft dieses Template das Template `module-content-sitemap` auf, andernfalls wird das Standard-Template `module-content-default` aufgerufen.

7.  Das Modul module-content-sitemap enthält die eigentlichen Regeln, mit denen das XML des Seitenmoduls umgewandelt wird: **Template für Sitemap-Modul**
    ~~~~ {.xml}
    ..
    <xsl:template name="module-content-sitemap">
      <xsl:param name="pageContent"/>
      <h1><xsl:value-of select="$pageContent/title"/></h1>
      <div class="contentSitemap">
        <xsl:call-template name="module-content-sitemap-items">
          <xsl:with-param name="items" select="$pageContent/sitemap/mapitem"/>
        </xsl:call-template>
      </div>
    </xsl:template>
    ...
    ~~~~

    Das Template module-content-sitemap ruft selbst noch das Template `module-content-sitemaps-items` auf, mit denen die Sitemap erstellt wird: **Template für Sitemap-Items**

    ~~~~ {.xml}
    ..
    <xsl:template name="module-content-sitemap-items">
      <xsl:param name="items"/>
      <xsl:if test="count($items) > 0">
        <ul>
          <xsl:for-each select="$items">
            <xsl:variable name="lastItem" select="position() = last()" />
            <xsl:call-template name="module-content-sitemap-item">
              <xsl:with-param name="item" select="."/>
              <xsl:with-param name="lastItem" select="$lastItem"/>
            </xsl:call-template>
          </xsl:for-each>
        </ul>
      </xsl:if>
    </xsl:template>
    ...
    ~~~~

    Für jedes Item ruft dieses Template wiederum das Template `module-content-sitemap-item` auf: **Template für einzelne Sitemap-Items**

    ~~~~ {.xml}
    ...
    <xsl:template name="module-content-sitemap-item">
      <xsl:param name="item"/>
      <xsl:param name="lastItem" select="false()" />
      <li>
        <xsl:if test="$lastItem">
          <xsl:attribute name="class">last</xsl:attribute>
        </xsl:if>
        <a href="{$item/@href}">
          <xsl:if test="$item/@target and $item/@target != '' and $item/@target != '_self'">
            <xsl:attribute name="target"><xsl:value-of select="$item/@target" /></xsl:attribute>
          </xsl:if>
          <xsl:value-of select="$item/@title"/>
        </a>
        <xsl:call-template name="module-content-sitemap-items">
          <xsl:with-param name="items" select="$item/mapitem"/>
        </xsl:call-template>
      </li>
    </xsl:template>
    ...
    ~~~~

8.  Das vollständige Stylesheet sieht nun wie folgt aus: **Vollständiges Stylesheet page_sitemap.xsl**
    ~~~~ {.xml}
    <?xml version="1.0"?>
    <xsl:stylesheet version="1.0"
    xmlns:xsl="http://www.w3.org/1999/XSL/Transform">

    <xsl:import href="page_main.xsl" />

    <!-- disable the additional content column, even if the xml
         contains boxes for it -->
    <xsl:param name="DISABLE_ADDITIONAL_COLUMN" select="true()" />

    <xsl:template name="content-area">
      <xsl:param name="pageContent" select="content/topic"/>
      <xsl:choose>
        <xsl:when test="$pageContent/@module = 'content_sitemap'">
          <xsl:call-template name="module-content-sitemap">
            <xsl:with-param name="pageContent" select="$pageContent"/>
          </xsl:call-template>
        </xsl:when>
        <xsl:otherwise>
          <xsl:call-template name="module-content-default"/>
        </xsl:otherwise>
      </xsl:choose>
    </xsl:template>

    <xsl:template name="module-content-sitemap">
      <xsl:param name="pageContent"/>
      <h1><xsl:value-of select="$pageContent/title"/></h1>
      <div class="contentSitemap">
        <xsl:call-template name="module-content-sitemap-items">
          <xsl:with-param name="items" select="$pageContent/sitemap/mapitem"/>
        </xsl:call-template>
      </div>
    </xsl:template>

    <xsl:template name="module-content-sitemap-items">
      <xsl:param name="items"/>
      <xsl:if test="count($items) > 0">
        <ul>
          <xsl:for-each select="$items">
            <xsl:variable name="lastItem" select="position() = last()" />
            <xsl:call-template name="module-content-sitemap-item">
              <xsl:with-param name="item" select="."/>
              <xsl:with-param name="lastItem" select="$lastItem"/>
            </xsl:call-template>
          </xsl:for-each>
        </ul>
      </xsl:if>
    </xsl:template>

    <xsl:template name="module-content-sitemap-item">
      <xsl:param name="item"/>
      <xsl:param name="lastItem" select="false()" />
      <li>
        <xsl:if test="$lastItem">
          <xsl:attribute name="class">last</xsl:attribute>
        </xsl:if>
        <a href="{$item/@href}">
          <xsl:if test="$item/@target and $item/@target != ''
                        and $item/@target != '_self'">
            <xsl:attribute name="target">
              <xsl:value-of select="$item/@target" />
            </xsl:attribute>
          </xsl:if>
          <xsl:value-of select="$item/@title"/>
        </a>
        <xsl:call-template name="module-content-sitemap-items">
          <xsl:with-param name="items" select="$item/mapitem"/>
        </xsl:call-template>
      </li>
    </xsl:template>

    <xsl:template name="page-styles">
      <xsl:call-template name="link-style">
        <xsl:with-param name="file">style_sitemap.css</xsl:with-param>
      </xsl:call-template>
    </xsl:template>

    </xsl:stylesheet>
    ~~~~

[export_de/Kategorie.md:Implementierungsphase: Webseitenvorlage erstellen](export_de/Kategorie.md:Implementierungsphase:_Webseitenvorlage_erstellen )