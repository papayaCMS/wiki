
Phrasen werden mit dem Template `language-text` übersetzt, das im Stylesheet `./_lang/language.xsl` definiert ist. Näheres zu diesem Template erfahren Sie in [Templates und Parameter in ./_lang/language.xsl](Templates_und_Parameter_in_./_lang/language.xsl.md). Das Template macht im Grunde genommen nichts anderes als zu testen, ob es für die übergebene Phrase in einer der Übersetzungsdateien eine entsprechende Übersetzung gibt. Falls keine der angegebenen Dateien eine Übersetzung enthält, wird einfach die Phrase zurückgegeben. Die Parameter der Übersetzungsdateien werden dabei in folgender Reihenfolge durchsucht:

1.  LANGUAGE_TEXTS_CURRENT
2.  LANGUAGE_MODULE_CURRENT
3.  LANGUAGE_DEFAULTS_CURRENT
4.  LANGUAGE_TEXTS_FALLBACK
5.  LANGUAGE_MODULE_FALLBACK
6.  LANGUAGE_DEFAULTS_FALLBACK

[Kategorie:Übersetzungstemplate benutzen](../export_de/Kategorie:Übersetzungstemplate_benutzen.md)
