---
title: Klassenstruktur von Modulen
permalink: /Klassenstruktur_von_Modulen/
---

Die Klassenhierarchie von Modulen sieht wie folgt aus:

[miniatur|zentriert|1000px|Klassenhierarchie von Modulen](/images/File:PapayaPluginsBaseSystem.png "wikilink")

Das bedeutet, alle Module sind von `base_object` und `base_plugin` abgeleitet und je nach Modultyp noch von einer weiteren Elternklasse. Damit stehen uns zum einen die Methoden der Elternklassen zur Verfügung, zum anderen müssen ein paar der Methoden, die dort nur als Interface angegeben sind, im eigenen Modul implementiert werden. Welche Methoden für welche Modultypen das sind, erfahren Sie in [:Kategorie:Basismodule für papaya CMS programmieren](/:Kategorie:Basismodule_für_papaya_CMS_programmieren "wikilink").

[Kategorie:Der modulare Aufbau von papaya CMS](/Kategorie:Der_modulare_Aufbau_von_papaya_CMS "wikilink")