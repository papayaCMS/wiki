---
title: Konfigurationsdatei conf.inc.php laden
permalink: /Konfigurationsdatei_conf.inc.php_laden/
---

Die Datei `conf.inc.php` enthält die grundlegende Konfiguration für eine papaya-Installation. Sie befindet sich im selben Verzeichnis wie die Datei `index.php`. Außer Konstantendefinitionen und Kommentaren sollte diese Datei nichts enthalten. Die `conf.inc.php` enthält alle Optionen, die nicht über das Backend einstellbar sind. Zusätzlich können für Entwicklungs- oder Demoserver Optionen definiert werden, die nicht verändert oder überladen werden sollen. papaya CMS prüft, ob eine Konstante existiert, bevor es diese mit dem Wert aus der Tabelle `papaya_options` oder dem Standardwert füllt.

Die Kommentare erklären die meisten Optionen ausreichend. Näheres zu den Optionen erfahren Sie in „papaya CMS 5: Handbuch für Administratoren“, Abschnitt 15.11.

[Kategorie:Wie sieht es unter der Haube aus?](/Kategorie:Wie_sieht_es_unter_der_Haube_aus? "wikilink")