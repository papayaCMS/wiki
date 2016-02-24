---
title: Template-Parameter
permalink: /Template-Parameter/
---

Die Parameter werden an unterschiedlichen Stellen konfiguriert. Teilweise kommen die Informationen aus der `conf.inc.php`, teilweise aus den papaya-Einstellungen.

|Parameter|Bedeutung|
|---------|---------|
|COLUMNWIDTH_CENTER|Bestimmt die Breite der mittleren Inhaltsspalte|
|COLUMNWIDTH_LEFT|Bestimmt die Breite der linken Inhaltsspalte|
|COLUMNWIDTH_RIGHT|Bestimmt die Breite der rechten Inhaltsspalte|
|PAGE_BASE_URL|Basis-URL des Backends, im Normalfall /papaya, könnte aber auch /my-cms/admin oder anders lauten.|
|PAGE_ICON|Icon des aktuellen Bereiches / der aktuellen Anwendung. Bei Anwendungen wird automatisch die 22x22px Version des in der modules.xml angegebenen Icons verwendet.|
|PAGE_LANGUAGE|Kürzel der aktuell verwendeten Sprache, z.B. de-DE.|
|PAGE_MODE|Bestimmt, ob die Ausgabe ein Frameset, ein Frame oder eine normale Seite ist (wird beim MediaDB-Browser verwendet).|
|PAGE_PATH_SKIN|Pfad zum Papaya Skin, default ist 'skins/default'.|
|PAGE_PROJECT|Enthält den Namen des Projektes, wie es unter Einstellungen benannt wurde.|
|PAGE_SELF|Enthält den Scriptnamen, also z.B. mediadb.php|
|PAGE_TITLE|Titel des aktuellen Breiches / der aktuellen Anwendung.|
|PAGE_TODAY|Aktuelles Datum/Uhrzeit|
|PAGE_URL|Komplette URL zur aktuellen Seite|
|PAGE_USER|Aktuell angemeldeter Benutzer|
|PAGE_WEB_PATH|Enthält den Pfad zur Ausgabe von papaya, also wo die /index.php liegt|
|PAPAYA_DBG_DEVMODE|Ob der Debugmodus angeschaltet ist oder nicht. Darüber kann man Debugausgaben im Template steuern.|
|PAPAYA_LOGINPAGE|Ist true, wenn die Loginseite ausgegeben wird.|
|PAPAYA_UI_LANGUAGE|Aktuell eingestellte Sprache für das Backend.|
|PAPAYA_USE_JS_WRAPPER|Ob JavaScript zusammengefasst werden soll (default = true)|
|PAPAYA_USE_JS_GZIP|Ob JavaScript komprimiert werden soll (default = true)|
|PAPAYA_USE_OVERLIB|Ob die overlib für hints verwendet werden soll. (default = true)|
|PAPAYA_USE_RICHTEXT|Ob der Richtex-Editor verwendet werden soll|
|PAPAYA_USE_RICHTEXT_EDITOR|Welcher Richtext-Editor verwendet werden soll (fckeditor oder tinymce)|
|PAPAYA_USE_TINYMCE_GZIP|Ob TinyMCE komprimiert ausgeliefert werden soll (default = false)|
|PAPAYA_USE_TINYMCE_VERSION|Welche TinyMCE Version verwendet werden soll (2 oder 3)|
|PAPAYA_WEBSITE_REVISION|Papaya Revision, entspricht PAPAYA_WEBSITE_REVISION, wird in /revision.inc.php beim Export gesetzt.|
|PAPAYA_VERSION|Die papaya Version, entspricht PAPAYA_VERSION_STRING, im Normalfall "5"|

Die folgenden Parameter sind nur für die Frontend-Ausgabe von Interesse, hier stehen sie nur der Vollständigkeit halber und sollten ggf. in Themes/Templates übernommen werden, falls nicht schon vorhanden.

|Parameter|Bedeutung|
|---------|---------|
|PAGE_MODE_PUBLIC|FRONTEND: Ob die Seite öffentlich ist oder nicht.|
|PAGE_OUTPUTMODE_CURRENT|FRONTEND: Der aktuelle Ausgabemodus der Seite (html, pdf, ...)|
|PAGE_OUTPUTMODE_DEFAULT|FRONTEND: Der Standardausgabemodus der Seite|
|PAGE_THEME|FRONTEND: Welches Theme verwendet wird, entspricht der papaya Konstante PAPAYA_LAYOUT_THEME|
|PAGE_THEME_PATH|FRONTEND: relativer Pfad zum aktuellen Theme, z.B. /my-cms/papaya-themes/my-theme/ oder CDN (Content Delivery Network) Theme-URL|
|PAGE_THEME_PATH_LOCAL|FRONTEND: relativer Pfad zum aktuellen Theme, z.B. /papaya-themes/default-theme/|

[miniatur|zentriert|1000px|PAGE_ICON und PAGE_TITLE](/images/File:admin-template-titel-icon.png "wikilink")

[Kategorie:Übersicht über die verfügbaren Komponenten](/Kategorie:Übersicht_über_die_verfügbaren_Komponenten "wikilink")