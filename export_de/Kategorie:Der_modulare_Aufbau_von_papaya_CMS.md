---
title: export_de/Kategorie.md:Der modulare Aufbau von papaya CMS
permalink: export_de/Kategorie.md:Der_modulare_Aufbau_von_papaya_CMS/
---

Das Modulkonzept in papaya CMS
------------------------------

In papaya CMS sind Funktionen an vielen Stellen als Modul gekapselt. Das hat viele Vorteile. So lassen sich eigene Module erstellen, die anstelle der standardmäßig vorhandenen zum Einsatz kommen. Dadurch kann das Verhalten des Systems in der Tiefe beeinflusst werden, ohne dass es inkompatibel für Aktualisierungen wird. Des weiteren kann ein Modul komplett neu geschrieben werden, wenn es die Umstände erfordern. Lediglich die Methoden müssen gleich bleiben, die darin aufgerufen werden.

Module werden in Paketen verwaltet. Ein Paket ist zunächst einmal nichts anderes als ein Verzeichnis, in dem sich die PHP-Dateien befinden. Jedes Paket besitzt zudem eine XML-Datei, `modules.xml`, in der alle Module des Pakets im Detail beschrieben sind. Näheres zur Struktur der `modules.xml` können Sie in [modules.xml erstellen](/modules.xml_erstellen ) erfahren.

[export_de/Kategorie.md:Wie funktioniert eigentlich papaya CMS?](export_de/Kategorie.md:Wie_funktioniert_eigentlich_papaya_CMS? )