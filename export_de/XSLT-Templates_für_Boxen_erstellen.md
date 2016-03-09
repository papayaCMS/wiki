---
title: XSLT-Templates für Boxen erstellen
permalink: /XSLT-Templates_für_Boxen_erstellen/
---

XSLT-Templates für Boxmodule unterscheiden sich von den Templates für Seitenmodule in einigen Punkten:

1.  Boxmodule müssen das Seitenmodul page_main.xsl nicht importieren.
2.  Boxmodule importieren stattdessen das Basistemplate `./base/boxes.xsl`.
3.  Die Seiten- und Boxtemplates werden unabhängig voneinander verarbeitet. Dabei wird die Ausgabe der Boxen in das Seiten-XML in CDATA-Abschnitten eingefügt.
4.  Da Boxen separat geparst werden und als CDATA-Abschnitte in das Seiten-XML eingefügt werden, genügt es, wenn Sie im Stylesheet für die Seitenmodule die XSLT-Anweisung `<xsl:value-of
              select="."/>` verwenden. Sie sollten dabei das Attribut `disable-output-escaping` einfügen und auf den Wert „yes“ setzen, damit der XSLT-Prozessor den HTML-Code nicht maskiert ausgibt.

Die Stylesheet-Dateien für Boxmodule im Demo-Template sind wesentlich einfacher aufgebaut als die für Seitenmodule. Sie enthalten in der Regel ein bis zwei Templates. Im Folgenden soll anhand der Stylesheet-Datei für das Boxmodul „Extended Page Link“ kurz vorgestellt werden, wie Sie ein Template für ein Boxmodul anlegen. Dazu gehen Sie wie folgt vor:

1.  Legen Sie im Verzeichnis `mein-Template/html/` die Stylesheet-Datei für das Boxmodul an. Für das Modul „Extended Page Link“ können Sie den Namen `box_pagelink.xsl` benutzen. Sie können natürlich auch eine andere Bezeichnung für diese Stylesheet-Datei wählen, sollten jedoch zumindest die Namenskonvention mit dem Präfix `box_` beibehalten.

1.  Erstellen Sie nun das XSLT-Grundgerüst: **XSLT-Grundgerüst in der Stylesheet-Datei box_pagelink.xsl anlegen**
    ~~~~ {.xml}
    <?xml version="1.0"?>
    <xsl:stylesheet version="1.0"
    xmlns:xsl="http://www.w3.org/1999/XSL/Transform">

    <xsl:template match="extended_pagelink">
    ...
    </xsl:template>

    </xsl:stylesheet>
    ~~~~

    Das einzige Template in diesem Stylesheet hat ein match-Attribut, dessen Suchmuster auf das oberste XML-Element der Boxausgabe passt.

2.  Legen Sie nun das Template für das oberste XML-Element der Box an: **Standard-Template für Boxen importieren**
    ~~~~ {.xml}
    ...
    <xsl:import href="./base/boxes.xsl"/>
    ...
    ~~~~

    Das zu importierende Stylesheet definiert alle notwendigen XSLT-Parameter.

3.  Fügen Sie am Kopf der Stylesheet-Datei noch den Kommentarblock mit der Liste der vom Stylesheet unterstützten Module ein: **Liste der vom Stylesheet unterstützten Module**
    ~~~~ {.xml}
    ...
    <!--
      @papaya:modules actionbox_pagelink_extended
    -->
    ...
    ~~~~

4.  Schreiben Sie nun das Template mit dem `match` -Attribut aus: **Template für das Modul „Extended Page Link“**
    ~~~~ {.xml}
    ...
    <xsl:template match="extended_pagelink">
      <xsl:apply-templates select="text-before/node()" />
      <xsl:text> </xsl:text>
      <a href="{link/@href}">
        <xsl:if test="popup">
          <xsl:attribute name="onclick">
            <xsl:text>return openPopup('</xsl:text>
              <xsl:value-of select="popup/@href" />
              <xsl:text>', '</xsl:text>
              <xsl:value-of select="popup/@name" />
              <xsl:text>', </xsl:text>
              <xsl:value-of select="popup/@width" />
              <xsl:text>, </xsl:text>
              <xsl:value-of select="popup/@height" />
              <xsl:text>, '</xsl:text>
              <xsl:value-of select="popup/@scrollbars" />
              <xsl:text>', '</xsl:text>
              <xsl:value-of select="popup/@resizeable" />
              <xsl:text>', '</xsl:text>
              <xsl:value-of select="popup/@toolbar" />
            <xsl:text>'); return false;</xsl:text>
          </xsl:attribute>
          <xsl:attribute name="target">_blank</xsl:attribute>
        </xsl:if>
        <xsl:apply-templates select="link/node()" />
      </a>
      <xsl:text> </xsl:text>
      <xsl:apply-templates select="text-after/node()" />
    </xsl:template>
    ...
    ~~~~

    Das Template gibt den Text aus, der vor dem Link steht ( `text-before/node()`.md), formatiert anschließend den Link selbst und gibt am Ende den Text aus, der nach dem Link folgt ( `text-after/node()`.md).

5.  Das vollständige Stylesheet sieht nun wie folgt aus: **Vollständiges Stylesheet für das Boxmodul „Extended Page Link“**
    ~~~~ {.xml}
    <?xml version="1.0"?>
    <xsl:stylesheet version="1.0"
       xmlns:xsl="http://www.w3.org/1999/XSL/Transform">
    <!--
      @papaya:modules actionbox_pagelink_extended
    -->

    <xsl:import href="./base/boxes.xsl" />

    <xsl:template match="extended_pagelink">
      <xsl:apply-templates select="text-before/node()" />
      <xsl:text> </xsl:text>
      <a href="{link/@href}">
        <xsl:if test="popup">
          <xsl:attribute name="onclick">
            <xsl:text>return openPopup('</xsl:text>
              <xsl:value-of select="popup/@href" />
              <xsl:text>', '</xsl:text>
              <xsl:value-of select="popup/@name" />
              <xsl:text>', </xsl:text>
              <xsl:value-of select="popup/@width" />
              <xsl:text>, </xsl:text>
              <xsl:value-of select="popup/@height" />
              <xsl:text>, '</xsl:text>
              <xsl:value-of select="popup/@scrollbars" />
              <xsl:text>', '</xsl:text>
              <xsl:value-of select="popup/@resizeable" />
              <xsl:text>', '</xsl:text>
              <xsl:value-of select="popup/@toolbar" />
            <xsl:text>'); return false;</xsl:text>
          </xsl:attribute>
          <xsl:attribute name="target">_blank</xsl:attribute>
        </xsl:if>
        <xsl:apply-templates select="link/node()" />
      </a>
      <xsl:text> </xsl:text>
      <xsl:apply-templates select="text-after/node()" />
    </xsl:template>

    </xsl:stylesheet>
    ~~~~

[Kategorie:Implementierungsphase: Webseitenvorlage erstellen](export_de/Kategorie:Implementierungsphase:_Webseitenvorlage_erstellen.md)