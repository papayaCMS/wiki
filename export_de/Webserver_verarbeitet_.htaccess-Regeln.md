
papaya CMS wird mit einer standardisierten `.htaccess` -Datei ausgeliefert.

Wenn eine Seitenanfrage an den Webserver geht, interpretiert der Webserver (vorausgesetzt, es ist Apache) zuerst die `.htaccess` -Datei. Diese Datei enthält Regeln und Konfigurationsoptionen, die den aktuellen Seitenaufruf beeinflussen. Die `.htaccess` -Datei aus der papaya-Standarddistribution enthält hauptsächlich Rewrite-Regeln, mit denen die URLs umgeschrieben werden. In einer weiteren Option wird ein statisches HTML-Dokument definiert, das eine generische 404-Fehlermeldung darstellt. Weitere Optionen steuern die Ausgabe des Expires-Headers.

Das URL-Rewriting wird aus folgenden Gründen verwendet:

1.  Mit dem Rewriting von URLs lassen sich suchmaschinenfreundliche Links erzeugen, da bestimmte Informationen wie die Seiten-ID, die Sprachversion oder die ID der Kategorie nicht über URL-Parameter an die `index.html` übergeben werden.
2.  Statische Dateien und Verzeichnisse haben Vorrang vor virtuellen. Dieser Umstand wird durch entsprechende Rewrite-Regeln durchgesetzt, wobei statische Dateien (mit Ausnahme der Dateien in der MediaDB) direkt durch Apache ausgeliefert werden, ohne dass papaya CMS diese Aufgabe übernehmen müsste. Dies bringt einige Performance-Vorteile mit sich.
3.  Wenn im Browser des Nutzers Cookies deaktiviert sind, wird die Session-ID in der Regel als GET-Parameter an die URL gehängt. Damit die URLs aber immer noch leserlich bleiben, wird die Session-ID durch Rewriting in den Pfad der URL geschrieben. Dadurch geht die Session-ID nicht so leicht verloren und sie kann nicht so einfach geändert werden.

Für eine erste Übersicht ist die vollständige `.htaccess` -Datei im folgenden Listing dargestellt:

**.htaccess-Datei aus papaya CMS (Revision 20064)**

~~~~ {.apache}
RewriteEngine On

#remove session id
RewriteRule ^/?sid[a-z]*([a-zA-Z0-9,-]{20,40})(/.*) $2 [QSA]

#admin pages
RewriteCond %{REQUEST_FILENAME} !-d
RewriteCond %{REQUEST_FILENAME} !-f
RewriteRule ^/?papaya/module_([a-z\d_]+)\.[a-z]{3,4} /papaya/module.php?p_module=$1 [QSA,L]

#media files - public / static
RewriteRule ^/?[^./]*\.(thumb|media)\.((.).*) - [E=mediaFile:/papaya-files/$3/$2]
RewriteCond %{DOCUMENT_ROOT}%{ENV:mediaFile} -f
RewriteRule ^/?[^./]*\.(thumb|media)\.((.).*) /papaya-files/$3/$2 [L]

#media files - wrapper script
#RewriteCond %{REQUEST_FILENAME} !-f
RewriteRule ^/?([a-fA-F0-9]/)*[a-zA-Z0-9_-]+\.(media|thumb|download|popup|image)(\.(preview))?((\.([a-zA-Z0-9_]+))?(\.[a-zA-Z0-9_]+))  /index.php [QSA,L]

#output pages
RewriteCond %{REQUEST_FILENAME} !-d
RewriteCond %{REQUEST_FILENAME} !-f
RewriteRule ^/?[a-zA-Z0-9_-]+((\.[0-9]+)?\.[0-9]+)((\.[a-z]{2,5})?\.[a-z]+)((\.[0-9]+)?.preview)? /index.php [QSA,L]
RewriteCond %{REQUEST_FILENAME} !-d
RewriteCond %{REQUEST_FILENAME} !-f
RewriteRule ^/?index((\.[a-z]{2,5})?\.[a-z]+)((\.[0-9]+)?.preview)? /index.php [QSA,L]

ErrorDocument 404 /index.php

#optimize cache headers for static content (if possible)
FileETag none
<IfModule mod_expires.c>
  ExpiresActive on
  ExpiresDefault A2592000
</IfModule>
<IfModule mod_headers.c>
  <FilesMatch "\.(ico|pdf|flv|jpg|jpeg|png|gif|js|css|swf)$">
    Header set Cache-Control "public, max-age=2592000, pre-check=2592000"
  </images/FilesMatch>
</IfModule>
~~~~

Die Apache-Direktiven in der .htaccess-Datei

Mit der ersten Anweisung wird das Apachemodul `mod_rewrite` aktiviert. Andernfalls können keine Regeln definiert werden:

**Rewrite-Engine aktivieren**

~~~~ {.apache}
RewriteEngine On
~~~~

Wird die Session über die URL weitergegeben, entfernt die folgende Rewrite-Regel die Session-ID aus dem Query-String. Die Session-ID wird in der Regel an den Query-String gehängt, wenn Cookies deaktiviert sind, siehe dazu die Option PAPAYA_SESSION_ID_FALLBACK . Die Session ID wird entfernt, indem alles an den Ziel-Querystring angehängt wird, was nicht `sid[a-z]*` ist (QSA, query string append):

**Session-ID aus dem Query-String entfernen**

~~~~ {.apache}
#remove session id
RewriteRule ^/?sid[a-z]*([a-zA-Z0-9,-]{20,40})(/.*) $2 [QSA]
~~~~

Die folgenden Rewrite-Bedingungen gelten für Requests aus dem Admin-Bereich:

**Requests für den Admin-Bereich umschreiben**

~~~~ {.apache}
#admin pages
RewriteCond %{REQUEST_FILENAME} !-d
RewriteCond %{REQUEST_FILENAME} !-f
RewriteRule ^/?papaya/module_([a-z\d_]+)\.[a-z]{3,4} /papaya/module.php?p_module=$1 [QSA,L]
~~~~

Zunächst wird überprüft, ob die angeforderte Ressource nicht eine existierende Datei ( `!-f`.md) oder ein Verzeichnis ( `!-d`.md) ist. Physisch vorhandene Dateien und Verzeichnisse werden vorrangig ausgegeben, da sie verfügbar sein sollen. Wenn weder eine Datei noch ein Verzeichnis an dieser Stelle existiert, kann die nachfolgende Regel angewendet werden. Diese besagt, dass alle Aufrufe von `/papaya/module_<modulname>.ext` umgeschrieben werden in `/papaya/module.php?p_module=<modulname>`, wiederum angehängt an den Query-String (QSA). Wenn diese Regel trifft, werden alle weiteren Regeln ignoriert (L = last, letzte).

Die folgende Bedingung prüft, ob statische Mediadaten vorhanden sind. Ist das der Fall, werden diese ausgeliefert und das Rewriting abgeschlossen:

**Statische Mediadateien direkt ausliefern**

~~~~ {.apache}
#media files - public / static
RewriteRule ^/?[^./]*\.(thumb|media)\.((.).*) - [E=mediaFile:/papaya-files/$3/$2]
RewriteCond %{DOCUMENT_ROOT}%{ENV:mediaFile} -f
RewriteRule ^/?[^./]*\.(thumb|media)\.((.).*) /papaya-files/$3/$2 [L]
~~~~

Wenn ein Aufruf direkt Inhalte der MediadDB anfordert, wird dieser an `index.php` weitergeleitet. Der Query-String wird an die `index.php` angehängt und der Redirect beendet (L). URLs für MediaDB-Inhalte haben in der Regen die Struktur `[filename].[identifier].[filehash].[extension]`, was in der Rewrite-Regel durch einen regulären Ausdruck kodiert ist:

**MediaDB-Aufrufe an die index.php weiterleiten**

~~~~ {.apache}
#media files - wrapper script
RewriteRule ^/?([a-fA-F0-9]/)*[a-zA-Z0-9_-]+\.(media|thumb|download|popup|image)(\.(preview))?((\.([a-zA-Z0-9_]+))?(\.[a-zA-Z0-9_]+))  /index.php [QSA,L]
~~~~

Bei ganz normalen Seiten kommen diese zwei Regelsätze zum tragen. Es wird jeweils wieder sichergestellt, dass keine Datei oder ein Verzeichnis mit dem angegebenen Namen tatsächlich existiert ( `!-d`, `!-f`.md), denn diese werden vorrangig behandelt:

**URL-Umformung bei Seitenausgabe im Frontend**

~~~~ {.apache}
#output pages
RewriteCond %{REQUEST_FILENAME} !-d
RewriteCond %{REQUEST_FILENAME} !-f
RewriteRule ^/?[a-zA-Z0-9_-]+((\.[0-9]+)?\.[0-9]+)((\.[a-z]{2,5})?\.[a-z]+)((\.[0-9]+)?.preview)? /index.php [QSA,L]
RewriteCond %{REQUEST_FILENAME} !-d
RewriteCond %{REQUEST_FILENAME} !-f
RewriteRule ^/?index((\.[a-z]{2,5})?\.[a-z]+)((\.[0-9]+)?.preview)? /index.php [QSA,L]
~~~~

Die Regeln sind wieder als reguläre Ausdrücke kodiert. Die erste Regel trifft bei allen Aufrufen einer Ausgabeseite der folgenden Form:

**Erste Rewrite-Regel für folgende URL-Formen**

~~~~ {.apache}
filename.topicId.lngId.extension
filename.categoryId.topicId.lngId.extension.versiontime.preview
~~~~

Die zweite Regel trifft bei URLs, die folgende Form aufweisen:

**Zweite Rewrite-Regel für folgende URL-Formen**

~~~~ {.apache}
index.lngId.extension.versiontime.preview
~~~~

In beiden Fällen wird der Querystring angehängt (QSA) und der Redirect beendet (L).

Die Bedeutung der einzelnen Felder ist in der folgenden Tabelle aufgeschlüsselt:

|Feldname|Bedeutung|
|filename|Entspricht dem Dateinamensteil der angeforderten Ressource.|
|categoryId|Optional: Eine numerische ID, die beispielsweise in der Kataloganwendung einer Katalog-ID entspricht.|
|topicId|Numerische ID, die der Seiten-ID entspricht. Wenn diese ID weggelassen wird, stellt papaya CMS automatisch die Standardseite dar.|
|lngId|Zwei- bis fünfstellige Buchstabenfolge, die die Content-Sprache kodiert. Auch diese Angabe ist optional. Wenn die lngId weggelassen wird, liefert papaya CMS die Standardsprachversion der Seite aus.|
|extension|Dateiendung, welche das Ausgabeformat der Seite bestimmt. Eine gültige Dateiendung wird bei der Konfiguration des Ausgabefilters definiert. Standardmäßig wird die Endung `html` für Webseiten angegeben.|
|versiontime|Optional kann ein UNIX-Timestamp angehängt werden. Dadurch wird die zu dieser angegebenen Zeit veröffentlichte Seitenversion dargestellt.|
|preview|Wenn Sie diese Endung an die URL hängen, sehen Sie die Vorschau der noch unveröffentlichten Seite. Diese Ansicht ist nur dann zugänglich, wenn Sie gleichzeitig im Backend von papaya CMS angemeldet sind.|

Diese Zeile weist Apache an, beim HTTP-Statuscode „404“ (Datei nicht gefunden), die `index.php` aufzurufen, damit sich papaya CMS um den Fehler kümmern kann:

**Fehlerbehandlung an papaya CMS delegieren**

~~~~ {.apache}
ErrorDocument 404 /index.php
~~~~

Diese Direktive weist Apache an, keinen ETag für Dateien zu vergeben:

**FileETag-Ausgabe durch Apache deaktivieren**

~~~~ {.apache}
#optimize cache headers for static content (if possible)
FileETag none
~~~~

Es gibt zwei Gründe, warum die ETag-Ausgabe durch Apache deaktiviert wird:

1.  papaya CMS übernimmt selbst die Ausgabe von ETags.
2.  Standardmäßig benutzt Apache für die ETag-Berechnung die Inode-Nummer der Dateien, was beim Einsatz von gespiegelten Servern hinter einem Load-Balancer zu falschen Ergebnissen führt, da die Inode-Nummern der gleichen angeforderten Datei auf verschiedenen Servern unterschiedlich sind.

Falls das Apache-Modul `mod_expires` verfügbar ist, wird die Ausgabe der Expires- und Cache-Control-Header zunächst einmal aktiviert. Mit dem an `ExpiresDefault` übergebenen Wert wird angegeben, dass das angeforderte Dokument 2.592.000 Sekunden (entspricht 30 Tagen) nach dem letzten Zugriff durch den Client (Code „A“) neu geladen werden kann, da der Cache nach Ablauf dieser Zeit ungültig wird. Davon unberührt bleiben Expires-Header, die von papaya CMS für verschiedene Dateien speziell gesetzt werden:

**Apache-Modul mod_expires aktivieren**

~~~~ {.apache}
<IfModule mod_expires.c>
  ExpiresActive on
  ExpiresDefault A2592000
</IfModule>
~~~~

Wenn das Apache-Modul `mod_headers` verfügbar ist, wird ein Cache-Control-Header für alle Dateien vom Typ ico, PDF, flv, jpg, png, gif, js, css sowie swf gesetzt:

**Cache-Control-Header setzen**

~~~~ {.apache}
<IfModule mod_headers.c>
  <FilesMatch "\.(ico|pdf|flv|jpg|jpeg|png|gif|js|css|swf)$">
    Header set Cache-Control "public, max-age=2592000, pre-check=2592000"
  </FilesMatch>
</IfModule>
~~~~

Die Cache-Zeit wird dabei jeweils auf 2.592.000 Sekunden (30 Tage) gesetzt (max-age). Mit "public" wird darüber hinaus angegeben, dass die angeforderte Ressource durch alle Zwischenstationen im Cache vorgehalten werden darf. `pre-check` definiert einen Update-Intervall in Sekunden, in dem der Client eine Ressource aktualisieren muss. Eine Ressource wird dabei erst nach dem Update angezeigt, sodass der Nutzer im Client den Updatevorgang ggf. in Form eines Nachladens beobachten kann.

[Kategorie:Wie sieht es unter der Haube aus?](export_de/Kategorie:Wie_sieht_es_unter_der_Haube_aus?.md)
