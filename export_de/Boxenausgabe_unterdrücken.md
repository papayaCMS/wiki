
Zu einer druckfreundlichen Ausgabe gehört auch, dass Sie alle Boxen aus der Seitenausgabe entfernen. Boxen stellen in der Regel Funktionen wie die Navigation, die Seitensuche oder weiterführende Links dar, die in einem Ausdruck keine Funktion mehr haben. Eine druckfreundliche Ausgabe sollte also lediglich den Artikeltext inklusive Titel, Untertitel und ggf. Seitenlogo enthalten.

Um die Boxenausgabe im Default-Template zu unterdrücken, müssen Sie die `page_main.xsl` importieren und zwei Parameter überladen:

**Auszug aus dem Print-Template**

~~~~ {.xml}
...
<xsl:param name="DISABLE_NAVIGATION_COLUMN" select="true()"/>
<xsl:param name="DISABLE_ADDITIONAL_COLUMN" select="true()"/>
...
~~~~

Weitere Elemente lassen sich einfach über CSS ausblenden, siehe [Druckfreundliches CSS schreiben](/Druckfreundliches_CSS_schreiben.md).

[Kategorie:Print-Templates schreiben](export_de/Kategorie:Print-Templates_schreiben.md)