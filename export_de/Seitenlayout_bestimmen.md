
Das Seitenlayout wird innerhalb des `<pages>` -Elements bestimmt. Über das Attribut `margin` geben Sie dabei die Außenränder im Uhrzeigersinn an, wobei der erste Wert den Abstand des bedruckbaren Bereiches nach oben bestimmt. Im Attribut `font` geben Sie die Familie und die Größe der Grundschrift an, während Sie im Attribut `align` die standardmäßige Textausrichtung festlegen.

Mit den einzelnen `<page>` -Elementen innerhalb `<pages>` bestimmen Sie das Layout der Titelseite, der Schlussseite und der Standardseite. Die Standardseite ( `name =
      "default"`.md) muss dabei unbedingt definiert werden, während sowohl die Titel- als auch die Schluss-Seite optional sind. Im Attribut „template“ wählen Sie dabei ein Seiten-Template aus dem `<template>` -Element aus.

Sie geben dabei `<page>` -Elemente ein, in der Sie den „beschreibbaren“ Bereich angeben. In diesem Bereich kann also der Inhalt der papaya-Artikel eingebunden werden:

**Seitenlayout bestimmen**

~~~~ {.xml}
...
<pages font="helvetica 9" margin="30 10 30 10" align="left">
  <page name="cover"
        mode="elements"
        template="cover"
        font="helvetica 12">
    <element for="title"
             margin="90 50 115 10"
             align="right"/>
    <element for="subtitle"
             margin="100 50 90 10"
             align="right"
             font="helvetica 11"/>
  </page>

  <page name="default" template="content">
    <column margin="35 110 20 8.4"
            align="left"/>
    <column margin="35 8.4 20 110"
            align="left"/>
    <footer margin="280 8.4 10 8.4"
            align="right"
            font="helvetica 7"/>
  </page>
</pages>
...
~~~~

Element-Modus

Das `<page>` -Element für die Titelseite ( `name = "cover"`.md) enthält zwei `<element>` -Tags, mit denen der Titel und der Untertitel von Artikeln eingebunden werden. Um `<element>` -Tags benutzen zu können, müssen Sie jedoch im `<page>` -Tag den Modus aktivieren ( `mode =
      "element"`.md).

Standardmodus

Das `<page>` -Element ist standardmäßig im Modus für Fließtext ( `mode = "default"`.md) eingestellt. Das bedeutet, dass der Inhalt in Form von Spalten (Kolumnen) in die Seite gesetzt wird. Die Anzahl und Breite der Spalten bestimmen Sie dabei mit `<column>` -Elementen. Die Größe einer Spalten bestimmen Sie mit dem `margin` -Attribut, das die druckbare Fläche in Form von Seitenabständen festlegt. Die Angaben erfolgen im Uhrzeigersinn (oben, rechts, unten, links).

Fußzeile

Im Standardmodus können Sie auch mit dem `<footer>` -Element eine Fußzeile bestimmen. Dazu fügen Sie einfach ein `<footer>` -Element in das `<page>` -Tag ein. Das `<footer>` -Element enthält ebenso wie das `<column>` -Element die Attribute `margin` und `align`. Zusätzlich jedoch können Sie mit dem `font` -Attribut die Schrift bestimmen.

[Kategorie:PDF-Template schreiben](export_de/Kategorie:PDF-Template_schreiben.md)