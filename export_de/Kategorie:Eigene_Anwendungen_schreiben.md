Anwendungen sind Programme, die papaya CMS um spezielle Funktionen erweitern. Anwendungen können aus verschiedenen Modulen für die Seiten- oder Boxausgabe bestehen. Sie enthalten jedoch insbesondere spezielle Administrationsmodule, mit denen die Benutzeroberfläche im Backend aufgebaut wird. Diese Administrationsmodule stellen Ein- und Ausgabeschnittstellen zur Verfügung, mit denen der Benutzer Daten eingeben und bearbeiten oder Einstellungen vornehmen kann.

Folgende Schritte sind notwendig um eine eigene Anwendung zu erstellen:

1.  Erstellen Sie ein Basismodul, das Daten in die Datenbank einfügen, sie aktualisieren und auslesen kann, siehe [Basismodul für Datenbankzugriffe schreiben](Basismodul_fuer_Datenbankzugriffe_schreiben.md).
2.  Erstellen Sie ein Administrationsmodul, in dem Sie bestehende Datensätze ansehen und ändern sowie neue Datensätze hinzufügen können, siehe [Administrationsmodul schreiben](Administrationsmodul_schreiben.md).
3.  Fügen Sie dem Administrationsmodul optional einige zusätzliche Konfigurationsoptionen hinzu, siehe [Moduloptionen hinzufügen](Moduloptionen_hinzufügen.md).
4.  Erstellen Sie ein Ausgabemodul, dass die Daten aus der Datenbank ausliest und in eine XML-Ausgabe überführt, siehe [Output-Klasse für Seiten und Boxen schreiben](Output-Klasse_für_Seiten_und_Boxen_schreiben.md).
5.  Erstellen Sie ein Konnektor-Modul, mit dem Sie die Daten für beliebige andere Module zur Verfügung stellen, siehe [Konnektoren - Schnittstellen für andere Anwendungen schaffen](Konnektoren_-_Schnittstellen_für_andere_Anwendungen_schaffen.md).

Das im Folgenden beschriebene Beispielprogramm soll Sammlungen (Collections) einfacher Text- oder Bildbausteine (Stickers), verwalten und diese ausgeben können.

