---
title: Apache konfigurieren
permalink: /Apache_konfigurieren/
---

Sie müssen folgende Apache-Module aktivieren:

1.  Rewrite-Modul aktivieren (mod_rewrite)
2.  PHP als Modul oder CGI aktivieren (wird von PHP mitgeliefert)

papaya CMS wird darüber hinaus mit `.htaccess` -Dateien ausgeliefert. Sie müssen also entweder die AllowOverride-Direktive anpassen, damit diese Dateien ausgewertet werden können, oder die Inhalte der `.htaccess` -Datei in die zentrale Apache-Konfigurationsdatei inkludieren. Näheres erfahren Sie in Ihrer Apache-Dokumentation.

[Kategorie:Server konfigurieren](/Kategorie:Server_konfigurieren "wikilink")