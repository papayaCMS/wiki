---
title: Installation der Webseitenvorlage
permalink: /Installation_der_Webseitenvorlage/
---

In der Installationsphase installieren Sie das Template-Theme-Set auf den Produktionsserver. Falls Sie die Seitenvorlage bestehend aus Template und Theme in einer lokalen papaya-Installation entwickelt haben, müssen Sie einfach nur die nötigen Verzeichnisse per FTP auf den Live-Server kopieren. Es sind nur folgende Schritte notwendig:

1.  Sie kopieren das Verzeichnis mit dem XSLT-Template in das Verzeichnis `papaya-data/templates/`.
2.  Sie kopieren das Verzeichnis mit dem passenden Theme in das Verzeichnis `papaya-theme/`.
3.  Sie kopieren die Datei `info.xml` in das Basisverzeichnis Ihres XSLT-Templates.

Nachdem Sie das Verzeichnis kopiert haben, aktivieren Sie Template und Theme, indem Sie die entsprechenden Einstellungen wie im folgenden Abschnitt beschrieben durchführen.

Neues Template-Theme-Set aktivieren
-----------------------------------

Um die neue Webseitenvorlage zu verwenden oder auch nur, um sie zu testen, müssen Sie die Layouteinstellungen in der Systemkonfiguration Ihrer papaya-Installation anpassen. Dazu wählen Sie die entsprechenden Verzeichnisse in der Optionengruppe „Layout“ aus:

[miniatur|zentriert|1000px|Layouteinstellungen ändern](/images/File:XMLpapayaLayoutAnpassen.png )

Näheres zur Optionengruppe „Layout“ aus der Systemkonfiguration erfahren Sie in "papaya CMS: Handbuch für Administratoren", Kapitel 12.2.5.

[Kategorie:Webseitenvorlage erstellen](/Kategorie:Webseitenvorlage_erstellen )