---
title: Aufruf von papaya CMS über index.php
permalink: /Aufruf_von_papaya_CMS_über_index.php/
---

Wenn Apache keine lokale Datei ausliefert, werden alle Anfragen an die `index.php` von papaya CMS weitergegeben. Die `index.php` ist das Hauptscript, das vom Webserver angesprochen wird, sobald eine Seite oder Datei ausgegeben werden soll. Das Script befindet sich im Installationsverzeichnis von papaya CMS, beispielsweise im `DocumentRoot`.

Die `index.php` bindet zunächst die Datei `conf.inc.php` ein. Diese Datei enthält die Basiskonfiguration der Webseite. Zur Basiskonfiguration gehören die Zugangsdaten zur Datenbank und grundlegende Konfigurationsoptionen (siehe [Konfigurationsdatei conf.inc.php laden](/Konfigurationsdatei_conf.inc.php_laden ) ).

Zu Beginn der `index.php` wird bestimmt, wie Fehlermeldungen ausgegeben werden:

**Bestimmung des Errorhandlings**

~~~~ {.php}
if (defined('PAPAYA_DBG_DEVMODE') && PAPAYA_DBG_DEVMODE) {
  $PAPAYA_FOUND_LIBRARY = include_once(PAPAYA_INCLUDE_PATH.'system/papaya_page.php');
} else {
  $PAPAYA_FOUND_LIBRARY = @include_once(PAPAYA_INCLUDE_PATH.'system/papaya_page.php');
}
~~~~

Wenn der Entwicklermodus ( PAPAYA_DBG_DEVMODE ) aktiv ist, wird keine Fehlerunterdrückung beim Einbinden der `papaya_page.php` verwendet. Die Klasse `papaya_page` ist für die Seiten- und Dateiauslieferung zuständig. Bei aktivierter Fehlerausgabe werden bei nicht vorhandenen Dateien oder Seiten umfangreiche Fehlerbehandlungen und Weiterleitungen durchgeführt. Mehr dazu erfahren Sie in [Seiten- und Dateiausgabe durch papaya_page kontrollieren](/Seiten-_und_Dateiausgabe_durch_papaya_page_kontrollieren ).

Falls das Basissystem von papaya CMS nicht gefunden wurde oder der Wartungsmodus (maintenance) aktiviert ist, wird ein entsprechender Status-Code (503) ausgegeben. Dafür wird zunächst getestet, ob PHP im CGI-Modus betrieben wird. Ist dies der Fall, sendet die `index.php` einen Status-Header, ansonsten einen HTTP-Header. Der HTTP-Fehlercode ist 503 und besagt, dass der Dienst zurzeit nicht verfügbar ist:

**503-Fehlermeldung bei falscher Konfiguration**

~~~~ {.php}
if ((!$PAPAYA_FOUND_LIBRARY) || (defined('PAPAYA_MAINTENANCE_MODE') && PAPAYA_MAINTENANCE_MODE)) {
if (php_sapi_name() == 'cgi' || php_sapi_name() == 'fast-cgi') {
    @header('Status: 503 Service Unavailable');
  } else {
    @header('HTTP/1.1 503 Service Unavailable');
  }
~~~~

Im folgenden Schritt wird geprüft, ob der Wartungsmodus aktiviert worden ist:

**Überprüfung des Wartungsmodus**

~~~~ {.php}
 if (defined('PAPAYA_MAINTENANCE_MODE') && PAPAYA_MAINTENANCE_MODE &&
      defined('PAPAYA_ERRORDOCUMENT_MAINTENANCE') && file_exists(PAPAYA_ERRORDOCUMENT_MAINTENANCE) &&
    is_file(PAPAYA_ERRORDOCUMENT_MAINTENANCE) && is_readable(PAPAYA_ERRORDOCUMENT_MAINTENANCE)) {
    header('Content-type: text/html; charset=utf-8;');
    readfile(PAPAYA_ERRORDOCUMENT_MAINTENANCE);
  } elseif (defined('PAPAYA_ERRORDOCUMENT_503') && file_exists(PAPAYA_ERRORDOCUMENT_503) &&
    is_file(PAPAYA_ERRORDOCUMENT_503) && is_readable(PAPAYA_ERRORDOCUMENT_503)) {
    header('Content-type: text/html; charset=utf-8;');
    readfile(PAPAYA_ERRORDOCUMENT_503);
  } else {
    echo 'Service Unavailable';
  }
~~~~

Ist der Wartungsmodus aktiviert und wurde eine existierende Datei als Fehlerdokument definiert, wird diese ausgeliefert. Alternativ wird das 503-Fehlerdokument ausgegeben, sofern es vorhanden ist. Wenn alle Stricke reißen, wird alternativ der Text "Service unavailable" ausgegeben.

Wenn das Basissystem gefunden werden konnte und der Wartungsmodus nicht aktiviert worden ist, wird versucht, die Revisionsdatei `revision.inc.php` einzubinden:

**Revsisionsdatei revision.inc.php einbinden**

~~~~ {.php}
} else {
  if ((!defined('PAPAYA_WEBSITE_REVISION')) && is_readable('revision.inc.php')) {
    require_once('./revision.inc.php');
  }
  $PAPAYA_PAGE = &new papaya_page();
  $PAPAYA_PAGE->execute();
  $PAPAYA_PAGE->get();
}
~~~~

Die Revisionsdatei gibt Auskunft über den Versionsstand einer papaya-Installation. Im letzten Block wird eine Instanz von `papaya_page` erstellt. Zunächst wird die Methode `papaya_page::execute()` aufgerufen, anschließend die Methode `papaya_page::get()`. Die letzte Methode erzeugt die Ausgabe für die aufgerufene URL.

Im folgenden Abschnitt wird die Konfigurationsdatei `conf.inc.php` besprochen.

[Kategorie:Wie sieht es unter der Haube aus?](export_de/Kategorie:Wie_sieht_es_unter_der_Haube_aus? )