---
title: Konfiguration der Fehlerausgabe
permalink: /Konfiguration_der_Fehlerausgabe/
---

Im Abschnitt "Fehlersuche" der Konfiguration können Sie einstellen, welche Fehler ausgegeben werden und wie die Ausgabe erfolgen soll. Die vollständige Liste von Konfigurationsoptionen finden Sie in „papaya CMS 5: Handbuch für Administratoren“, Abschnitt 15.3.

|---|
|!|
|PAPAYA_DBG_SHOW_DEBUGS|Bestimmt, ob Aufrufe von `base_object::debug()` eine Ausgabe erzeugen sollen oder nicht. Auf Produktionsinstallationen sollte diese Option aus Sicherheitsgründen per `define()` in der `conf.inc.php` deaktiviert werden.|
|PAPAYA_DBG_PHP_SHOW_ERROR|Bestimmt, ob Fehlermeldungen ausgegeben werden oder nicht. Auf Produktionsinstallationen sollte diese Option aus Sicherheitsgründen per `define()` in der `conf.inc.php` deaktiviert werden.|
|PAPAYA_DBG_PHP_SHOW_FIREBUG|Bestimmt, ob die Ausgabe von Fehlermeldungen und `debug()` -Aufrufen in der Konsole der Firebug-Erweiterung erfolgen soll oder im Browser.|
|PAPAYA_DBG_PHP_ERROR|Bestimmt, welcher Fehlerlevel zum Einsatz kommen soll. Im Entwicklungsumfeld sollte dieser auf E_ALL stehen, im Produktivbetrieb können z.B. Notices ausgeschaltet werden. In der Regel werden Notices während der Entwicklung beseitigt. Dennoch können auch in Produktivumgebungen sehr viele Notice-Meldungen das Protokoll von papaya CMS überfluten, sodass schwerwiegendere Fehlermeldungen leicht übersehen werden können.|

[Kategorie:Fehlersuche in papaya CMS](export_de/Kategorie:Fehlersuche_in_papaya_CMS.md)