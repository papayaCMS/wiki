---
title: export_de/Kategorie.md:Datenbankzugriffe optimieren
permalink: export_de/Kategorie.md:Datenbankzugriffe_optimieren/
---

Optimierte Datenbankabfragen können die Performance spürbar verbessern. Mit papaya CMS können Sie eine SQL-Abfrage anzeigen und durch ein Explain analysieren lassen. Explain ist eine Funktion des SQL-Datenbankservers, der die Zugriffsstrategie des Servers für eine mit Explain gekennzeichnete SQL-Abfrage ausgibt. Die Implementation ist dabei von Server zu Server unterschiedlich, weshalb diese Funktion in papaya CMS durch die Datenbankabstraktion ausgeführt wird. Ferner können Sie sich die Anzahl der SQL-Abfragen bei einem Seitenaufruf anzeigen lassen und langsame Abfragen für die spätere Analyse protokollieren.

Wenn Sie die Konfigurationsoption PAPAYA_DBG_DATABASE_EXPLAIN aktivieren, wird für alle Datenbankabfragen der Explain-Modus aktiviert. Die Ausgabe ist oft sehr lang, doch auch aufschlussreich. Sie sehen beispielsweise, welche Querys bei einer Seitenerstellung lange dauern und können dort ansetzen, die Abfrage mittels Umstellung oder durch Verwendung von Indices zu beschleunigen.

[export_de/Kategorie.md:Fehlersuche in papaya CMS](export_de/Kategorie.md:Fehlersuche_in_papaya_CMS )