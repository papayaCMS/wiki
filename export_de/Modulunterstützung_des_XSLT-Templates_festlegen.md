---
title: Modulunterstützung des XSLT-Templates festlegen
permalink: /Modulunterstützung_des_XSLT-Templates_festlegen/
---

Wenn Sie ein XSLT-Template für ein Box- oder ein Seitenmodul schreiben, ist das Template in der Regel für ein bestimmtes Modul oder mehrere bestimmte Module ausgelegt. Sie können die jeweiligen Modulabhängigkeiten in ihrem Template festhalten, indem Sie am Anfang des Stylesheets einen Kommentar einfügen, der eine Liste aller unterstützten Module enthält. papaya CMS liest diese Liste ein und zeigt dem Nutzer im Backend an, wenn eine Ansicht mit einem inkompatiblen XSLT-Template verknüpft worden ist.

Die Kommentarzeile mit der Modulliste hat folgendes Format:

**Kommentarzeile mit Modulen (papaya_main.xsl)**

~~~~ {.xml}
...
<xsl:stylesheet version="1.0" xmlns:xsl="http://www.w3.org/1999/XSL/Transform"
  ns="http://www.w3.org/TR/REC-html40">
<!--
  @papaya:modules content_imgtopic, content_errorpage,
  content_categimg, content_tagcateg, content_xhtml
-->
...
~~~~

Der Kommentar direkt unterhalb des `<xsl:stylesheet>` -Elements beginnt mit den String `@papaya:modules` und enthält eine kommaseparierte Liste mit den Namen der Module, die durch dieses Template unterstützt werden.

[Kategorie:Implementierungsphase: Webseitenvorlage erstellen](export_de/Kategorie:Implementierungsphase:_Webseitenvorlage_erstellen.md)