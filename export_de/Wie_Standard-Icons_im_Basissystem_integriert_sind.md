---
title: Wie Standard-Icons im Basissystem integriert sind
permalink: /Wie_Standard-Icons_im_Basissystem_integriert_sind/
---

Die Icons des papaya-Basissystems sind in der Datei `./papaya/inc.glyphs.php` registriert. Die globale Variable `$PAPAYA_IMAGES` ist ein Array, dessen Schlüssel die Bezeichner der Icons bilden. Die Bezeichner setzen sich dabei aus dem Namen der Gruppe und des Icons zusammen, die durch einen Bindestrich miteinander verbunden sind. Das Array `$PAPAYA_IMAGES` enthält dabei den physischen Dateinamen als Wert.

In `./papaya/module.php` wird der Instanz des eigenen Backend-Moduls eine Referenz auf `$PAPAYA_IMAGES` als `$images` -Attribut gesetzt. Bei Bedarf kann das Attribut an Unterklassen als Referenz weitergegeben werden:

**Referenz auf \$images-Array weitergeben**

~~~~ {.php}
$myObj->images = &$this->images;
~~~~

[export_de/Kategorie:Icons für Bearbeitungsmenü oder Adminanwendung benutzen](export_de/Kategorie:Icons_für_Bearbeitungsmenü_oder_Adminanwendung_benutzen )