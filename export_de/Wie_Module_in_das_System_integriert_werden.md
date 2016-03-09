---
title: Wie Module in das System integriert werden
permalink: /Wie_Module_in_das_System_integriert_werden/
---

Die Integration der Module in papaya CMS hängt maßgeblich vom Modultyp ab. Zunächst einmal werden alle Module in der Datenbanktabelle `papaya_modules` sowie `papaya_modulegroups` registriert. Dazu wird die `modules.xml` des Pakets beim Modulscan eingelesen.

Die Tabelle `papaya_modulegroups` entspricht einem Paket und enthält folgende Informationen:

1.  Die ID des Pakets.
2.  Den Namen des Pakets.
3.  Den Dateisystempfad zu den Modulen.
4.  Die Liste der Datenbanktabellen, die das Paket benutzt.

In der Tabelle `papaya_modules` werden für jedes Modul die Angaben gespeichert, die in den Attributen des jeweiligen `<module>` -Elements in der `modules.xml` angegeben sind, siehe Tabelle "Moduleigenschaften" in [modules.xml erstellen](/modules.xml_erstellen ). Weitere zusätzliche Informationen sind:

1.  Die ID des Pakets, zu dem das Modul gehört.
2.  Der Name des Moduls, der optional für die jeweilige Installation in der Modulverwaltung bearbeitet werden kann.
3.  Der Status des Moduls, der aktiv oder inaktiv sein kann.

[export_de/Kategorie.md:Verzeichnisse und Metadaten für Pakete erstellen](export_de/Kategorie.md:Verzeichnisse_und_Metadaten_für_Pakete_erstellen )