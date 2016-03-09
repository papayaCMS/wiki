---
title: Klassenstruktur von Modulen
permalink: /Klassenstruktur_von_Modulen/
---

Die Klassenhierarchie von Modulen sieht wie folgt aus:

<<<<<<< HEAD
[miniatur|zentriert|1000px|Klassenhierarchie von Modulen](/images/File:PapayaPluginsBaseSystem.png )
=======
![File:PapayaPluginsBaseSystem.png](images/PapayaPluginsBaseSystem.png)
>>>>>>> a2efb5b3261d70ebc0ed214a6131387e209c4f80

Das bedeutet, alle Module sind von `base_object` und `base_plugin` abgeleitet und je nach Modultyp noch von einer weiteren Elternklasse. Damit stehen uns zum einen die Methoden der Elternklassen zur Verfügung, zum anderen müssen ein paar der Methoden, die dort nur als Interface angegeben sind, im eigenen Modul implementiert werden. Welche Methoden für welche Modultypen das sind, erfahren Sie in [:Kategorie:Basismodule für papaya CMS programmieren](/:export_de/Kategorie:Basismodule_für_papaya_CMS_programmieren ).

[Kategorie:Der modulare Aufbau von papaya CMS](export_de/Kategorie:Der_modulare_Aufbau_von_papaya_CMS )
