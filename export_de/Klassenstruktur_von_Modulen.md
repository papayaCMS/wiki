
Die Klassenhierarchie von Modulen sieht wie folgt aus:

![File: Klassenhierarchie von Modulen](images/PapayaPluginsBaseSystem.png)

Das bedeutet, alle Module sind von `base_object` und `base_plugin` abgeleitet und je nach Modultyp noch von einer weiteren Elternklasse. Damit stehen uns zum einen die Methoden der Elternklassen zur Verfügung, zum anderen müssen ein paar der Methoden, die dort nur als Interface angegeben sind, im eigenen Modul implementiert werden. Welche Methoden für welche Modultypen das sind, erfahren Sie in [Kategorie:Basismodule für papaya CMS programmieren](export_de/Kategorie:Basismodule_für_papaya_CMS_programmieren.md).

[Kategorie:Der modulare Aufbau von papaya CMS](export_de/Kategorie:Der_modulare_Aufbau_von_papaya_CMS.md)
