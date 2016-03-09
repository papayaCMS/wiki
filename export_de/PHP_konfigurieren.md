---
title: PHP konfigurieren
permalink: /PHP_konfigurieren/
---

Sie müssen folgende PHP-Erweiterungen aktivieren:

1.  Erweiterung für Grafikprozessor (ext/gd)
2.  Datenbankerweiterung, bspw. ext/mysqli für MySQL
3.  Erweiterung für XSLT, bspw. xsl
4.  Erweiterung für XML-RPC (ext/xmlrpc)

Darüber hinaus müssen Sie in PHP die Session-Funktion aktivieren und ggf. den Mailserver konfigurieren

Gegebenenfalls ist es notwendig, dass Sie im Konfigurationsabschnitt [mail function] der php.ini die Mailfunktion konfigurieren. Näheres zur Konfiguration der Mailfunktion erfahren Sie im PHP-Handbuch, siehe <http://www.php.net/manual/de/> .

[Category:Server konfigurieren](Category:Server_konfigurieren )