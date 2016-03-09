---
title: Übersetzungsdateien in XSLT-Parameter laden
permalink: /Übersetzungsdateien_in_XSLT-Parameter_laden/
---

Damit die von Ihen angelegten Übersetzungsdateien auch benutzt werden, müssen Sie die folgenden Parameter in Ihrem Stylesheet definieren und die Dokumente darin laden:

1.  Laden Sie eine Übersetzungsdatei für die aktuelle Content-Sprache.
2.  Laden Sie eine Übersetzungsdatei in der Standard-Sprache Ihres Webprojektes. Diese Datei wird als *Fallback* benötigt, falls für eine Content-Sprache keine Übersetzugsdatei angelegt worden ist.

Folgendes Listing stellt vor, wie die entsprechenden Übersetzungsdateien in den Parametern geladen werden:

**Übersetzungsdateien laden**

~~~~ {.xml}
...
<!-- Übersetzungsdatei in der aktuellen Content-Sprache laden -->
<xsl:param name="LANGUAGE_MODULE_CURRENT"
           select="document(concat($PAGE_LANGUAGE'.xml'))" />

<!-- Fallback-Übersetzungsdatei in der Standardsprache laden -->
<xsl:param name="LANGUAGE_MODULE_FALLBACK" select="'en-US.xml'"/>
...
~~~~

Das obige Beispiel lädt die Übersetzungsdatei in der aktuellen Content-Sprache. Die Content-Sprache ist im Parameter PAGE_LANGUAGE hinterlegt, die zu diesem Zweck ausgelesen wird. Die Fallback-Sprache wird hingegen aus einer fest vorgegebenen Datei geladen. Sie sollten sicherstellen, dass die von Ihnen angegebene Datei auch tatsächlich angelegt worden ist, damit dieser Sicherungsschritt auch sinnvoll eingesetzt werden kann. Andernfalls kann es passieren, dass die nicht übersetzte Phrase zurückgegeben wird, wenn das Phrasensystem keine Übersetzung finden kann.

[Kategorie:Das Übersetzungsframework benutzen](export_de/Kategorie:Das_Übersetzungsframework_benutzen.md)