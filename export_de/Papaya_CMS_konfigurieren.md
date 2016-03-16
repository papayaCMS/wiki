
Nachdem Sie die Programmdateien auf den Server kopiert haben, müssen Sie grundlegende Einstellungen in der Datei `conf.inc.php` konfigurieren. Zur Grundkonfiguration gehört bspw. die Datenbankverbindung. Sie finden die `conf.inc.php` im Basisverzeichnis von DocumentRoot. Falls Sie papaya CMS in ein Unterverzeichnis von DocumentRoot installiert haben, finden Sie die `conf.inc.php` im entsprechenden Unterverzeichnis.

In der folgenden Tabelle sind die Konfigurationsoptionen dargestellt, die Sie in der `conf.inc.php` einstellen können:

|Konstante|Bedeutung|
|PAPAYA_INCLUDE_PATH|Pfad zum Verzeichnis `papaya-lib/`.|
|PAPAYA_DB_URI|Benutzername, Passwort, Host, Datenbankname.|
|PAPAYA_DB_URI_WRITE|Bei MySQL-Multiclustering: Benutzername, Passwort, Host und Datenbankname des Masterservers. Im Standardfall: NULL.|
|PAPAYA_DB_TBL_OPTIONS|Name der Optionstabelle (inklusive Tabellenpräfix). In diese Tabelle werden die Konfigurationsoptionen Ihrer papaya-Installation gespeichert.|
|PAPAYA_DB_TABLEPREFIX|Präfix der Datenbanktabellen. Die Standardeinstellung lautet „papaya“.|
|PAPAYA_MAINTENANCE_MODE|„TRUE“, um den Wartungsmodus zu aktivieren, andernfalls „FALSE“. Im Wartungsmodus werden keine Seiten ausgeliefert. papaya liefert stattdessen eine HTML-Seite aus, die Sie in der PAPAYA_ERRORDOCUMENT_MAINTENANCE angeben müssen.|
|PAPAYA_ERRORDOCUMENT_MAINTENANCE|Vollständigen Pfad zur HTML-Datei angeben, die im Wartungsmodus angezeigt wird. Diese Datei sollte entsprechende Infos für die Nutzer der Site enthalten.|
|PAPAYA_ERRORDOCUMENT_503|Vollständiger Pfad zur HTML-Datei, die bei einer 503-Fehlermeldung ausgegeben werden soll.|
|PAPAYA_PASSWORD_METHOD|Auswahl des Hashing-Algorithmus. Zur Verfügung stehen:

1.  md5
2.  sha1
3.  sha256 (nur bei installierter Suhosin-Erweiterung)|
|PAPAYA_PASSWORD_PREFIX|Präfix, das dem Passwort vor dem Hashing hinzugefügt wird.|
|PAPAYA_PASSWORD_SUFFFIX|Suffix, das dem Passwort vor dem Hashing angehängt wird.|
|PAPAYA_DBG_DEVMODE|„TRUE“, wenn Debuginformationen ausgegeben werden sollen, andernfalls „FALSE“.|
|PAPAYA_PATH_DATA|Absoluter Pfad zum Verzeichnis `papaya-data/`.|

[Kategorie:papaya CMS installieren und konfigurieren](export_de/Kategorie:papaya_CMS_installieren_und_konfigurieren.md)