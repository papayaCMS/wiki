---
title: Verzeichnisstruktur für Paket erstellen
permalink: /Verzeichnisstruktur_für_Paket_erstellen/
---

Um eigene Module zu entwickeln, müssen Sie zunächst die Paketstruktur erstellen. Diese Paketstruktur besteht aus einem Basisverzeichnis und Metadaten, in denen wesentliche Merkmale wie Typ oder Dateiname der einzelnen Module festgehalten werden. Ein Paket besteht dabei aus mindestens einem Modul, kann aber beliebig viele Module beliebigen Typs enthalten.

Die grundlegende Verzeichnisstruktur für eigene Module ist dabei immer die selbe. Neben den PHP-Dateien enthält dieses Verzeichnis auch Metadaten in Form der `modules.xml` sowie Tabellenstrukturbeschreibungen. Diese Informationen werden von papaya CMS benötigt, damit die Module in der Modulverwaltung registriert und anschließend verwendet werden können.

Pakete für papaya CMS müssen in ein Verzeichnis unterhalb von `./papaya-lib/modules` abgelegt werden. Dabei sollten eigene Pakete und Pakete von Drittanbietern von den Basispaketen unterschieden werden, die im papaya-Release enthalten sind (Verzeichnisse `free` und `_base` ). Zu diesem Zweck sollten Sie eigene Pakete innerhalb von `./papaya-lib/modules/external/` installieren.

**Beispiel für ein Verzeichnis mit eigenem Paket**

    <nowiki>./papaya-lib/modules/external/my_module</nowiki>

Paketverzeichnis anlegen
------------------------

Um ein Paketverzeichnis anzulegen, gehen Sie wie folgt vor:

1.  Legen Sie im Verzeichnis . `/papaya-lib/modules/external/` das Verzeichnis für Ihr Paket an.
2.  Erstellen Sie innerhalb des Paketverzeichnisses das Unterverzeichnis `DATA`.
3.  Erstellen Sie innerhalb des Paketverzeichnisses das Unterverzeichnis `pics`.
4.  Erstellen Sie innerhalb des Unterverzeichnisses `pics` die Unterverzeichnisse `16x16`, `22x22` und `48x48`.

Nachdem Sie die Verzeichnisse angelegt haben, sollte Ihr Paketverzeichnis folgende Struktur aufweisen:

**Struktur eines Paket-Verzeichnisses**

    <nowiki>./
    ./DATA/
    ./pics/
    ./pics/16x16/
    ./pics/22x22/
    ./pics/48x48/</nowiki>

Die Unterverzeichnisse und Metadateien des Pakets werden in folgender Tabelle ausführlich beschrieben.

|Paketinhalt|Funktion|
|-----------|--------|
|./modules.xml|Diese XML-Datei enthält alle wesentlichen Informationen zu jedem Modul des Pakets. Dazu gehört der Name der Quellcodedatei, der Name der PHP-Klasse, die GUID des Moduls sowie der Modultyp.|
|./DATA/|Dieses optionale Verzeichnis enthält für jede Datenbanktabelle, die das Modul benutzt, eine separate XML-Datei. In dieser XML-Datei wird die Struktur der jeweiligen Datenbanktabelle beschrieben.|
|./pics/|Dieses optionale Unterverzeichnis enthält Icons für Backend-Anwendungen von papaya CMS. Dazu gehört beispielsweise das Icon für das Admin-Modul sowie Icons, die für spezielle Funktionen im Bearbeitungsmenü der Anwendung benutzt werden können. Das `pics` -Unterverzeichnis enthält für die drei Standard-Icongrößen in papaya CMS jeweils Unterverzeichnisse ( `16x16`, `22x22`, `48x48` ).|

Anschließend können Sie damit beginnen, Module für papaya CMS zu schreiben. Damit diese Module in papaya CMS verwendet werden können, müssen Sie die `modules.xml` -Datei erstellen. Näheres dazu erfahren Sie im folgenden Abschnitt.

[Kategorie:Verzeichnisse und Metadaten für Pakete erstellen](/Kategorie:Verzeichnisse_und_Metadaten_für_Pakete_erstellen )