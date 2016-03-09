---
title: Kategorie:Wie sieht es unter der Haube aus?
permalink: Kategorie:Wie_sieht_es_unter_der_Haube_aus?/
---

Dieses Kapitel beschreibt im Detail, wie ein Seitenaufruf durch den Webserver und papaya CMS verarbeitet wird. An einem Seitenrequest wird beispielhaft erklärt, wie die Seitenanfrage erst durch den Webserver verarbeitet und anschließend an papaya CMS weitergereicht wird. Die beteiligten papaya-Klassen und ihre Methoden werden in der Reihenfolge ihrer Aufrufe beschrieben.

Die einzelnen aufgerufenen Instanzen sind folgende:

1.  Webserver mit .htaccess-Datei, siehe [Webserver verarbeitet .htaccess-Regeln](/Webserver_verarbeitet_.htaccess-Regeln ).
2.  Aufruf von papaya CMS über die `index.php`, siehe [Aufruf von papaya CMS über index.php](/Aufruf_von_papaya_CMS_über_index.php ).
3.  Konfigurationsdatei `conf.inc.php` laden, siehe [Konfigurationsdatei conf.inc.php laden](/Konfigurationsdatei_conf.inc.php_laden ).
4.  Seiten- und Dateiausgabe durch `papaya_page.php` kontrollieren, siehe [Seiten- und Dateiausgabe durch papaya_page kontrollieren](/Seiten-_und_Dateiausgabe_durch_papaya_page_kontrollieren ).
5.  Konfigurationsoptionen aus der Datenbank laden ( `base_options.php` ), siehe [Konfigurationsoptionen aus der Datenbank laden (base_options.php)](/Konfigurationsoptionen_aus_der_Datenbank_laden_(base_options.php) ).
6.  Seiten-Content für Preview-Modus ( `papaya_topic.php` ) und Frontend ( `papaya_topic_public.php` ) ausgeben, siehe [Seiten-Content für Preview-Modus und Frontend ausgeben](/Seiten-Content_für_Preview-Modus_und_Frontend_ausgeben ).

[Kategorie:Wie funktioniert eigentlich papaya CMS?](Kategorie:Wie_funktioniert_eigentlich_papaya_CMS? )