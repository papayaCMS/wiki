---
title: Element cover und final
permalink: /Element_cover_und_final/
---

<cover>
-------

Mit dem `<cover>` -Element bestimmen Sie, welche Inhalte auf der Titelseite erscheinen sollen. Zu diesem Zweck weisen Sie dem Element entsprechende `<element>` -Tags zu, die Inhalte wie Titel, Untertitel oder Teaser enthalten können. Sie können nur `<element>` -Tags benutzen, die im entsprechenden `<page>` -Element für das `<cover>` -Tag definiert worden sind. Das <cover>-Element selbst enthält keine Attribute.

<final>
-------

Das `<final>` -Element stellt die Inhalte für die letzte Seite dar. Diese Inhalte werden in `<element>` -Tags eingefügt, die im `<page>` -Element für dieses `<final>` -Tag definiert worden sind. Sie können nur `<element>` -Tags benutzen, die in diesem `<page>` -Element definiert worden sind. Das `<final>` -Element selbst enthält keine Attribute.

<element>
---------

Das `<element>` -Tag umschließt Inhalte, die auf der Seite genau positioniert werden. Die exakte Position wird dabei im Layoutbereich definiert. Das `<element>` -Tag enthält genau ein Attribut:

|Attribut|Bedeutung|
|--------|---------|
|id|Zuordnung zum `<element>` -Tag, das im `<page>` -Element enthalten ist. Das `<page>` -Element wiederum wird im `<layout>` -Element definiert, das die Layoutdefinition enthält. Der Wert des id-Attributs entspricht dem Wert des for-Attributs aus dem Layoutbereich.|

[Kategorie:papaya-Formatierungsobjekt](export_de/Kategorie:papaya-Formatierungsobjekt.md)