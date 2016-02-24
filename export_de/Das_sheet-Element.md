---
title: Das sheet-Element
permalink: /Das_sheet-Element/
---

XML-Ausgabe
-----------

Verfügbare Elemente
-------------------

Es gibt drei Bereiche der Ausgabe: Header-Zeilen, Info-Zeile und Text. Headerzeilen können CSS-Klassen zugewiesen bekommen, headertitle führt zu einer Überschrift in der Ausgabe. Die Info-Zeile ist grau abgesetzt. Der Text kann entweder reinen Text enthalten, dann wird er mit einer dickengleichen Schrift ausgegeben (teletype) oder, sobald ein HTML-Paragraph (p) enthalten ist, wird eine HTML-Ausgabe erzeugt. Beide Varianten können Links und Bilder enthalten.

Für längere Texte sollte nicht die Phrasen-Übersetzung von papaya verwendet werden, sondern `_gtfile()`.

|---|
|!|
|sheet/@width|Breite der Ausgabe. Wenn leer, wird die Standardspaltenbreite des Layouts verwendet.|
|sheet/header/lines/line|Headerzeilen|
|sheet/header/lines/line/@class|Klassenangabe zur Headerzeile, `headertitle` führt zu einer Überschrift in der Ausgabe.|
|sheet/header/infos/line|Mehrere `line` s werden nicht umbrochen, kann weitere Informationen enthalten.|
|sheet/text|Kann einfachen Text mit HTML enthalten, sobald ein p enthalten ist, wird es als Fließtext angezeigt, andernfalls als "teletype". Um einen gleichmäßigen Abstand zu erhalten empfiehlt es sich das p mit `style="padding: 0px 10px;"` zu versehen.|

[Kategorie:Die einzelnen Komponenten im Detail](/Kategorie:Die_einzelnen_Komponenten_im_Detail "wikilink")