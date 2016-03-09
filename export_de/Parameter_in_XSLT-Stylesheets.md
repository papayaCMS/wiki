---
title: Parameter in XSLT-Stylesheets
permalink: /Parameter_in_XSLT-Stylesheets/
---

Der Ausgabefilter startet die XSLT-Stylesheetdatei standardmäßig mit einer Reihe von Parametern. Diese Parameter können Sie in Ihren XSLT-Templates benutzen, um bspw. den Hostnamen benutzen zu können. Die Parameter und ihre Bedeutung sind in der folgenden Tabelle aufgelistet:

|Parameter|Bedeutung|
|---------|---------|
|PAGE_BASE_URL|Die Basis-URL der papaya-Installation, beispielsweise <http://www.domain.tld> .|
|PAGE_LANGUAGE|Die aktuelle Content-Sprache der Seite.|
|PAGE_MODE_PUBLIC|1 (TRUE), wenn die Seite veröffentlicht ist, 0 (FALSE), wenn die Seite in der Seitenvorschau angesehen wird.|
|PAGE_OUTPUTMODE_CURRENT|Aktuelles Ausgabeformat der Seite. Die Konstante enthält die Dateiendung, also „html“ für die Webseitenausgabe oder „pdf“ für die PDF-Ausgabe.|
|PAGE_OUTPUTMODE_DEFAULT|Standard-Ausgabeformat der Seite. Die Konstante enthält die Dateiendung, also „html“ für die Webseitenausgabe oder „pdf“ für die PDF-Ausgabe.|
|PAGE_THEME|Name des Theme-Verzeichnisses unterhalb von `papaya-themes/`.|
|PAGE_THEME_PATH|Relative URL-Pfadangabe zum Themes-Verzeichnis, ausgehend vom DocumentRoot. Falls in papaya CMS jedoch die Konstante PAPAYA_CDN_THEMES mit einer URL belegt worden ist, enthält PAGE_THEME_PATH die absolute URL zum Themes-Verzeichnis unter der in PAPAYA_CDN_THEMES angegebenen URL.|
|PAGE_THEME_PATH_LOCAL|Relative URL-Pfadangabe zum Themes-Verzeichnis im Server-Dateisystem, ausgehend vom DocumentRoot.|
|PAGE_TITLE|Titel des aktuellen Dokuments.|
|PAGE_TODAY|Aktuelles Datum im ISO-Format (Jahr-Monat-Tag Stunde:Minuten:Sekunden).|
|PAGE_URL|URL der Seite.|
|PAGE_WEB_PATH|Basis-URL der papaya-Installation. Wenn papaya CMS in das DocumentRoot installiert worden ist, wird `/` ausgegeben. Wenn papaya CMS in ein Unterverzeichnis installiert worden ist,wird das entsprechende Unterverzeichnis ausgegeben.|
|PAGE_WEBSITE_REVISION|Enthält den aktuellen Revisionsstring der papaya-Installation. Die Entwicklungsversion enthält beispielsweise immer den String "DEV". Der Parameter PAGE_WEBSITE_REVISION wird in der optionalen Datei `revision.inc.php` definiert.|
|PAPAYA_CDN_THEMES|URL zum Themes-Verzeichnis, das auf einem anderen Server liegt, beispielsweise <http://www.andere-domain.tld/papaya-themes/>.|
|PAPAYA_DBG_DEVMODE|„TRUE“, wenn der Entwicklungsmodus aktiviert ist, andernfalls „FALSE“. Der Typ dieses Parameters ist boolean.|
|PAPAYA_VERSION|Versionsstring von papaya CMS.|
|PAPAYA_WEBSITE_REVISION|Wert der gleichnamigen PHP-Konstanten in papaya CMS. Diese Konstante kann beispielsweise die aktuelle Revisionsnummer aus einem Versionierungssystem enthalten oder Textinhalte wie „Dev“.|
|SYSTEM_TIME|Aktuelle Lokalzeit.|
|SYSTEM_TIME_OFFSET|Differenz der Lokalzeit zur UTC.|

[export_de/Kategorie:Formatvorlagen in papaya CMS](export_de/Kategorie:Formatvorlagen_in_papaya_CMS )