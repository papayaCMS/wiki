
Sie können sich die neueste Version von papaya CMS unter folgender URL herunterladen: [1](http://www.papaya-cms.com/Download)

Die Gzip-Datei enthält folgende Dateien und Verzeichnisse:

1.  `build.log`: Die Protokollausgabe des Scriptes, das die Gzip- oder Zip-Datei erzeugt hat.
2.  `files/`: Das Verzeichnis mit den Programmdateien von papaya CMS.
3.  `readme/`: Das Verzeichnis mit Hilfsscripten und Hilfetexten.

Das Verzeichnis files/

Das Verzeichnis `files/` enthält folgende Verzeichnisse und Dateien:

conf.inc.php
Basiskonfiguration für papaya CMS.

favicon.ico
Das Favourites-Icon (sollte durch Favourites-Icon Ihres Webprojektes ersetzt werden)

.htaccess
Enthält RewriteRule-Direktiven.

index.php
Seitenindex-Datei für das Frontent von papaya CMS.

papaya/
Enthält das Backend (Administrationsbereich) von papaya CMS.

papaya-data/
Enthält die XSLT-Templates sowie die Verzeichnisse der Mediendatenbank.

papaya-lib/
Enthält die Programmbibliothek von papaya CMS.

papaya-script/
Enthält Standard-Scripte für das Frontend von papaya CMS

papaya-themes/
Enthält die Themes (CSS, Schmuckgrafiken) für das Frontend von papaya CMS

Das Verzeichnis readme/

Das Verzeichnis `readme/` enthält folgende Dateien:

changelog.txt
Enthält eine Liste der behobenen Bugs und neuen Leistungsmerkmale, die seit der letzten Version hinzugekommen sind.

credits.txt
Danksagung. Personen, die an der Entwicklung von papaya CMS mitgewirkt haben, sowie externe Softwareprojekte, deren Produkte in papaya CMS genutzt werden.

gopapaya.php
Mit diesem Script können Sie überprüfen, ob der Server die Voraussetzungen für den Betrieb von papaya CMS erfüllt.

gpl.txt
Die Gnu General Public Licence.

.htaccess
Falls dieses Verzeichnis aus Versehen in das DocumentRoot kopiert wird: Enthält die Anweisung „deny from all“.

htaccess.tpl
Eine .htaccess-Vorlage mit Platzhaltern, falls Sie papaya CMS nicht direkt in DocumentRoot installieren möchten, sondern in ein Unterverzeichnis von DocumentRoot. Sie können die Platzhalter mit dem Namen des Unterverzeichnisses ersetzen.

liesmich.txt
Installationshilfe (Kurzanleitung) auf Deutsch.

readme.txt
Installationshilfe auf Englisch.

update_de.txt
Anweisungen (Deutsch), um eine bestehende papaya-Installation zu aktualisieren.

update_en.txt
Anweisungen (Englisch), um eine bestehende papaya-Installation zu aktualisieren.

Im nächsten Schritt kopieren Sie die Programmdateien aus dem Verzeichnis `files/` in das DocumentRoot-Verzeichnis.

[Kategorie:Papaya CMS installieren und konfigurieren](export_de/Kategorie:Papaya_CMS_installieren_und_konfigurieren.md)
