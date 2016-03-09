---
title: Basisdokument erzeugen
permalink: /Basisdokument_erzeugen/
---

Das Basisdokument wird im Template erzeugt, das ein *match* auf den Wurzelknoten enthält. Sie definieren in diesem Template sowohl das Layout als auch den Bereich, in dem der Inhalt gesetzt wird. Per *call-template* werden weitere Templates aufgerufen, die den Artikelinhalt in das Ausgabedokument einsetzen.

Das folgende Beispiel zeigt das Haupttemplate mit dem *match* auf das Wurzelelement „/“:

**Haupttemplate mit match auf „/“**

~~~~ {.xml}
...
<xsl:template match="/">
  <article lang="{$PAGE_LANGUAGE}" charset="UTF-8">
    <layout>

      <fonts>
        ...
      </fonts>

      <templates>
        ...
      </templates>

      <pages>
        ...
      </pages>

    </layout>

    <xsl:call-template name="content_area" />

  </article>
</xsl:template>
...
~~~~

Das oben dargestellte Basistemplate enthält die Layoutdefinitionen im Element `<layout>`. Dieses enthält folgende Unterelemente:

|Element|Bedeutung|
|-------|---------|
|`<fonts>`|Definiert Schriften, siehe [Schriftfamilien und -schnitte auswählen](/Schriftfamilien_und_-schnitte_auswählen ).|
|`<templates>`|Bindet die PDF-Musterdatei über Seitentemplates ein, siehe [Seitentemplates bestimmen](/Seitentemplates_bestimmen ).|
|`<pages>`|Bestimmt das Layout für die Titel-, Standard- und Schluss-Seite, siehe [Seitenlayout bestimmen](/Seitenlayout_bestimmen ).|

Nach dem Layoutbereich wird mit der Anweisung `call-template` das Template „content_area“ aufgerufen, das die Inhalte in das papaya-Formatierungsobjekt einfügt. Näheres dazu erfahren Sie in [Zentrales Template für den Inhaltsbereich](/Zentrales_Template_für_den_Inhaltsbereich ).

[Kategorie:PDF-Template schreiben](Kategorie:PDF-Template_schreiben )