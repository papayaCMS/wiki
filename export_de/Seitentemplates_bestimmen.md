---
title: Seitentemplates bestimmen
permalink: /Seitentemplates_bestimmen/
---

Als Grundlage für den PDF-Export dient die PDF-Musterdatei. Damit die Template-Engine von papaya CMS die Inhalte auch in die richtige Seite der Musterdatei setzen kann, müssen Sie Seitentemplates für die Titelseite, die Standardinhalte sowie die Schlussseite bestimmen. In diesen Seitentemplates erfolgt die Zuordnung Ein Seitentemplate besteht dabei aus folgenden Elementen:

1.  Attribut „name“: Name der Seitenvorlage, die bei der Bestimmung des Seitenlayouts aufgerufen wird.
2.  Attribut „file“: Name der PDF-Musterdatei.
3.  Attribut „page“: Nummer der Seite, in die der Inhalt gesetzt werden soll.

Der folgende Ausschnitt stellt die Definition der Seitentemplates vor:

**Seitentemplates bestimmen**

~~~~ {.xml}
...
<templates>
  <template name="cover"
            file="template/{$languageIdent}.pdf"
            page="1" />
  <template name="content"
            file="template/{$languageIdent}.pdf"
            page="2" />
  <template name="final"
            file="template/{$languageIdent}.pdf"
            page="2" />
</templates>
...
~~~~

[Kategorie:PDF-Template schreiben](/Kategorie:PDF-Template_schreiben )