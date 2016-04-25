
Um ein papaya-Modul oder eine ganze Anwendung als Paket auszuliefern, gehen Sie wie folgt vor:

1.  Erstellen Sie ein spezielles Exportverzeichnis mit der kompletten Verzeichnisstruktur, in das die Dateien auf dem Zielsystem installiert werden sollen. Der Name des Exportverzeichnisses sollte dem Format <Modulname>-<Version>.<Unterversion> entsprechen, beispielsweise `example-module-0.1`.
2.  In dieses Exportverzeichnis kopieren Sie die Dateien Ihres Pakets. Sofern Sie ein Versionsverwaltungssystem verwenden, entfernen Sie unbedingt alle von diesem System erstellten Metadaten (z.B. die `.svn` -Verzeichnisse.md). Versionsverwaltungssysteme wie Subversion bieten auch Exportfunktionen an, die keine Metadaten erstellen.
3.  Fügen Sie dem Paket eine Datei mit den Lizenzangaben bei. Für papaya CMS gilt die GPL 2.
4.  Wenn Ihr Paket Seiten- oder Boxmodule enthält, sollten Sie Beispieltemplates beifügen. Diese Templates können durch die Nutzer einfach an die eigene Webseitenvorlage angepasst werden.
5.  Legen Sie Ihrem Programmpaket auch eine `README.txt` bei. Die `README.txt` ist eine Textdatei, in der Sie Installationsanweisungen für den Nutzer angeben.

Lizenzangaben beachten

papaya CMS unterliegt der GPL (GNU General Public License). Diese verpflichtet Sie als Nutzer und Entwickler von Erweiterungen, bei der Weitergabe des Programmcodes bestimmte Regeln einzuhalten. Dazu gehört, dass Sie alle Ableitungen des papaya-Basissystems ebenso unter die GPL stellen und den Quellcode frei zur Verfügung stellen. Mit Ableitungen sind entweder direkte Änderungen am Basissystem von papaya CMS gemeint oder eigene Module und Anwendungen, die Sie von den Basisklassen ableiten.

Wenn Sie also Ihre Anwendung zum Download anbieten möchten, sollten Sie ihrem Programmcode auch eine Kopie der GPL beilegen.

Dateien und Verzeichnisse in ein TGZ- oder ZIP-Archiv packen

Um das Paket weiterzugeben, sollten Sie es in ein TGZ- oder ZIP-Archiv packen. Sie können dazu Ihr Lieblingspackprogramm verwenden, beispielsweise WinZip, 7zip, file-roller, ark oder die Kommandozeilenbefehle tar oder zip.

Stellen Sie sicher, dass das Archiv das Paketverzeichnis mit Versionsnummer explizit enthält, damit beim Entpacken die Dateien in einem eigenen Verzeichnis landen. Beim Packen mit `tar` oder `zip` sollten Sie sich eine Ebene über dem Exportverzeichnis befinden. Bei GUI-Programmen wählen Sie bitte das Exportverzeichnis aus, nicht alle einzelnen darin enthaltenen Dateien und Verzeichnisse.

Das folgende Listing stellt dar, wie Sie ein Paket mit dem Packprogramm `tar` erstellen können:

**Paket mit tar erstellen**

    <nowiki>$ tar cvzf example-module-0.1.tgz example-module-0.1</nowiki>

Das folgende Listing stellt dar, wie Sie das Paket mit dem Packprogramm `zip` erstellen können:

**Paket mit zip erstellen**

    <nowiki>$ zip -r example-module-0.1.zip example-module-0.1</nowiki>

[Kategorie:Verzeichnisse und Metadaten für Pakete erstellen](../export_de/Kategorie\:Verzeichnisse_und_Metadaten_fuer_Pakete_erstellen.md)
