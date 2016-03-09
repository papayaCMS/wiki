---
title: Das glyph-Element
permalink: /Das_glyph-Element/
---

XML-Ausgabe
-----------

Struktur der HTML-Ausgabe
-------------------------

|Attribut|Bedeutung|
|--------|---------|
|src|Pfad zum Icon. Entweder 1. per `./pfad/zu/icon.png` relativ zum Adminverzeichnis oder 2. als `module:GUID/pics/my_icon.png` im `<Modulverzeichnis>/pics/<size>/`, oder 3. als `actions-edit.png` relativ zu `<Adminverzeichnis>/pics/icons/<size>/`|
|hint|Alternativtext und title-Attribut des img-Tags|
|id|eindeutige ID, sofern benötigt|
|onclick|`onClick` -Handler für Javascript Aktionen|
|size|Größe des Icons, 16, 22 oder 48 sind die Standardgrößen, default ist 16|

[Kategorie:Die einzelnen Komponenten im Detail](export_de/Kategorie:Die_einzelnen_Komponenten_im_Detail )