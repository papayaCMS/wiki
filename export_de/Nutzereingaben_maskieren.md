---
title: Nutzereingaben maskieren
permalink: /Nutzereingaben_maskieren/
---

Nutzereingaben sollten maskiert werden, um diverse Sicherheitslücken wie beispielsweise SQL-Injection-Lücken sowie Cross-Site-Scripting-Lücken zu verhindert. Eine SQL-Injection bezeichnet das Einschleusen von Datenbankbefehlen durch einen Angreifer, um beispielsweise Daten zu löschen, zu stehlen oder zu manipulieren. Bei einer Cross-Site-Scripting-Attacke werden Nutzereingaben unkontrolliert in einer Webseite ausgegeben. Dies ist zum Beispiel bei Weblogs möglich, in denen Nutzereingaben als Kommentar zum Blogeintrag dargestellt werden. Wird die Eingabe nicht maskiert, kann ein Angreifer beispielsweise Schadcode in Form von bestimmten JavaScript-Programmen in die Seite einschleusen, der in Browsern von Besuchern ausgeführt wird.

Nutzerdaten werden in der Regel automatisch durch Methoden wie `initializeParams()` maskiert. Näheres dazu erfahren Sie in [:export_de/Kategorie.md:POST/GET-Parameter lesen und Sessiondaten verwalten](/:export_de/Kategorie.md:POST/GET-Parameter_lesen_und_Sessiondaten_verwalten ).

Sie können jedoch auch bei der Ausgabe der Daten dafür sorgen, dass fremde Nutzereingaben maskiert ausgegeben werden, siehe [Text bei der Ausgabe maskieren](/Text_bei_der_Ausgabe_maskieren ).

[export_de/Kategorie.md:Content ausgeben und Nutzereingaben maskieren](export_de/Kategorie.md:Content_ausgeben_und_Nutzereingaben_maskieren )