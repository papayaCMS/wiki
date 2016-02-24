---
title: Elemente
permalink: /Elemente/
---

Im Default Skins-Ordner ist die `style.xsl` das Haupttemplate. Dort wird die grundlegende Struktur der Seite festgelegt und weitere Sub-Templates eingebunden. Im Unterordner `controls` liegen unterteilt die einzelnen Elemente, die verwendet werden können. Im Unterordner lang liegen Sprachdefinitionen, für die wenigen Stellen, an denen die Ausgabe nicht über das Phrasenmodul übersetzt werden können oder sollen.

calendar.xsl
------------

Dieses Template erstellt eine Monatsübersicht für einen Kalender.

dialogs.xsl
-----------

Dieses Template erstellt ein Formular, das z.B. mit `base_dialog` generiert wurde.

[miniatur|zentriert|1000px|Beispiel für ein Formular](/images/File:formular-example.png "wikilink")

generics.xsl
------------

Dieses Template enthält die Ausgabe für Basis-HTML Elemente wie a, b, div, span, img, script, noscript, object, Flash-Objekt, checkbox, dann papaya-spezifisches wie glyph, icon-url sowie die Hilfs-Templates float-fix, replace-string und escape-quotes-js, noescape und @\*|node().

grid.xsl
--------

Dieses Template wird derzeit (1/2009) ausschließlich im Who-is-Who verwendet.

iconpanel.xsl
-------------

Dieses Template erstellt eine vertikale Liste von Icons. Benutzt wird dieses Template in der Online-Hilfe sowie im Nachrichtensystem von papaya CMS.

[miniatur|zentriert|1000px|Beispielausgabe für iconpanel](/images/File:backend-komponenten-iconpanel-example.png "wikilink")

listview.xsl
------------

Dieses Template erstellt eine tabellarische Liste. Diese sehr flexible Komponente wird zusammen mit „Dialog“ wohl am häufigsten verwendet. Im Modus "tile" wird dieses Template auch für die Ausgabe der Iconübersicht verwendet.

[miniatur|zentriert|1000px|einfache Listview](/images/File:listview-simple.png "wikilink")

[miniatur|zentriert|1000px|aufklappbare Listview mit Nodes](/images/File:listview-node.png "wikilink")

[miniatur|zentriert|1000px|komplexe Listview mit Paging, Spaltenüberschriften, Sortierung, Formular und Footer](/images/File:listview-complex.png "wikilink")

[miniatur|zentriert|1000px|Listview im "tiled" Modus](/images/File:listview-tiled-example.png "wikilink")

login.xsl
---------

Dieses Template wird ausschließlich für das Loginformular für das Backend verwendet.

[miniatur|zentriert|1000px|Das Loginformular](/images/File:login-example.png "wikilink")

menus.xsl
---------

Dieses Template generiert eine Menü- oder Werkzeugleiste mit Icon/Text-Buttons.

[miniatur|zentriert|1000px|Menü- oder Toolbar](/images/File:toolbar-example.png "wikilink")

messages.xsl
------------

Dieses Template generiert Nachrichtenboxen und Bestätigungsdialoge.

[miniatur|zentriert|1000px|Nachrichtenbox mit einer Nachricht](/images/File:message-example-single.png "wikilink")

[miniatur|zentriert|1000px|Nachrichtenbox mit mehreren Nachrichten](/images/File:message-example-multiple.png "wikilink")

panel.xsl
---------

Dieses Template generiert Blöcke (Panels), die in den einzelnen Spalten des Layouts ausgegeben werden. Intern wird Panel für die meisten Ausgabeblöcke verwendet.

sheet.xsl
---------

Dieses Template generiert einen größeren Informationsblock.

[miniatur|zentriert|1000px|Beispielausgabe für "sheet"](/images/File:sheet-example.png "wikilink")

[miniatur|zentriert|1000px|Umfangreiches Beispiel für "sheet"](/images/File:sheet-full-example.png "wikilink")

[miniatur|zentriert|1000px|Teletype-Beispiel für "sheet"](/images/File:sheet-teletype-example.png "wikilink")

thumbnails.xsl
--------------

Dieses Template generiert eine Thumbnail-Liste. Das wird für den Datei-Browser verwendet.

[miniatur|zentriert|1000px|Beispielausgabe für Thumbnail-Liste](/images/File:thumbnail-browser-example.png "wikilink")

[Kategorie:Übersicht über die verfügbaren Komponenten](/Kategorie:Übersicht_über_die_verfügbaren_Komponenten "wikilink")