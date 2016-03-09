---
title: Datenbank anlegen
permalink: /Datenbank_anlegen/
---

Legen Sie eine Datenbank für papaya CMS an. Sie sollten für diese Datenbank zudem einen Nutzer anlegen, der die vollen Administrationsrechte über diese Datenbank verfügt. Achten Sie darauf, dass als Zeichensatz UTF-8 ausgewählt wird. Das folgende Beispiel zeigt, wie Sie über die Konsole eine korrekte MySQL-Datenbank für papaya CMS anlegen können:

**Datenbank für papaya CMS anlegen**

~~~~ {.sql}
CREATE DATABASE IF NOT EXISTS datenbankname
       CHARACTER SET utf8
       COLLATE utf8_general_ci;
GRANT ALL PRIVILEGES ON datenbankname.*
       TO 'benutzername'@'localhost'
       IDENTIFIED BY 'passwort';
~~~~

[Kategorie:Server konfigurieren](export_de/Kategorie:Server_konfigurieren.md)