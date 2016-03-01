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

[File:formular-example.png](images/formular-example.png)

generics.xsl
------------

Dieses Template enthält die Ausgabe für Basis-HTML Elemente wie a, b, div, span, img, script, noscript, object, Flash-Objekt, checkbox, dann papaya-spezifisches wie glyph, icon-url sowie die Hilfs-Templates float-fix, replace-string und escape-quotes-js, noescape und @\*|node().

grid.xsl
--------

Dieses Template wird derzeit (1/2009) ausschließlich im Who-is-Who verwendet.

iconpanel.xsl
-------------

Dieses Template erstellt eine vertikale Liste von Icons. Benutzt wird dieses Template in der Online-Hilfe sowie im Nachrichtensystem von papaya CMS.

![File:backend-komponenten-iconpanel-example.png](images/backend-komponenten-iconpanel-example.png)

listview.xsl
------------

Dieses Template erstellt eine tabellarische Liste. Diese sehr flexible Komponente wird zusammen mit „Dialog“ wohl am häufigsten verwendet. Im Modus "tile" wird dieses Template auch für die Ausgabe der Iconübersicht verwendet.

![File:listview-simple.png](images/listview-simple.png)

![File:listview-node.png](images/listview-node.png)

![File:listview-complex.png](images/listview-complex.png)

![File:listview-tiled-example.png](images/listview-tiled-example.png)

login.xsl
---------

Dieses Template wird ausschließlich für das Loginformular für das Backend verwendet.

![File:login-example.png](images/login-example.png)

menus.xsl
---------

Dieses Template generiert eine Menü- oder Werkzeugleiste mit Icon/Text-Buttons.

![File:toolbar-example.png](images/toolbar-example.png)

messages.xsl
------------

Dieses Template generiert Nachrichtenboxen und Bestätigungsdialoge.

![File:message-example-single.png](images/message-example-single.png)

![File:message-example-multiple.png](images/message-example-multiple.png)

panel.xsl
---------

Dieses Template generiert Blöcke (Panels), die in den einzelnen Spalten des Layouts ausgegeben werden. Intern wird Panel für die meisten Ausgabeblöcke verwendet.

sheet.xsl
---------

Dieses Template generiert einen größeren Informationsblock.

![File:sheet-example.png](images/sheet-example.png)

![File:sheet-full-example.png](images/sheet-full-example.png)

[miniatur|zentriert|1000px|Teletype-Beispiel für (/images/sheet-teletype-example.pgn

thumbnails.xsl
--------------

Dieses Template generiert eine Thumbnail-Liste. Das wird für den Datei-Browser verwendet.

![File:thumbnail-browser-example.png](/images/thumbnail-browser-example.png)

[Kategorie:Übersicht über die verfügbaren Komponenten](/Kategorie:Übersicht_über_die_verfügbaren_Komponenten "wikilink")
