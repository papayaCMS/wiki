---
title: PDF-Musterdatei erstellen
permalink: /PDF-Musterdatei_erstellen/
---

Die PDF-Datei besteht aus einem Titelblatt, einer Seite für Inhalte und einer Rückseite. Während Titelseite und Rückseite optional sind, muss die Inhaltsseite immer vorhanden sein. Die Seite für Inhalte dient als laufende Seite. Bei überfließenden Inhalten wird ein Seitenumbruch eingefügt, wobei das Layout dieser Inhaltsseite erhalten bleibt.

Titelseite definieren
---------------------

Auf der Titelseite sollten Sie einen Bereich festlegen, in den der Titel und Untertitel des Artikels gesetzt werden kann. Optional können Sie auch Bereiche für den Teaser-Text und für Bilder vorsehen. Für die Inhaltsseite können Sie einen Bereich festlegen, in den der Artikeltext gesetzt werden soll. Die folgende Abbildung zeigt die Titelseite der PDF-Musterdatei aus dem Demotemplate:

[miniatur|zentriert|1000px|Titelseite aus der PDF-Musterdatei](/Datei:PDFMusterdateiTitelseite.png "wikilink")

In der PDF-Musterdatei aus dem Demotemplate ist vorgesehen, dass die hellgrün hinterlegte Fläche mit dem Titel des Artikels gefüllt wird. Dazu müssen Sie sich beim Erstellen der PDF-Musterdatei die genaue Position dieses Feldes als Pixelangabe notieren.

Standardseite definieren
------------------------

Die Standardseite ist für die Inhalte des Artikels gedacht und dient als laufende Seite. Der Bereich für die Inhalte sollte groß genug gewählt sein, Damit die Texte inklusive Bilder ausreichend Platz haben. Gegebenenfalls können Sie am unteren Seitenrand ausreichend Platz für die Fußzeile lassen. Die folgende Abbildung zeigt die Standardseite aus der PDF-Musterdatei:

[miniatur|zentriert|1000px|Standardseite aus der PDF-Musterdatei](/Datei:PDFMusterdateiStandardseite.png "wikilink")

Zu beachten ist, dass Sie nur eine Standardseite anlegen müssen. Wenn der Inhalt sehr umfangreich ist, wird einfach ein Seitenumbruch eingefügt, sodass in der PDF-Datei eine neue Seite eingefügt wird, die das selbe Layout verwendet.

Rückseite definieren
--------------------

Die Rückseite kann optional dazu verwendet werden, um Nutzungs- und Urheberrechte darzustellen oder Quellen anzugeben. Nutzungs- und Urheberrechte können Sie dabei direkt in die PDF-Datei eingeben, sodass diese Seite später bei der PDF-Seitenausgabe lediglich angehängt werden muss. Die PDF-Musterdatei im Demotemplate von papaya CMS enthält jedoch keine Rückseite, sodass an dieser Stelle kein Beispiel abgebildet werden kann.

Größe und Position aller Inhaltsbereiche notieren
-------------------------------------------------

Sie sollten sich die genauen Maße und die Größe der rechteckigen Bereiche notieren, in die die Inhalte gesetzt werden. Wenn Sie später das XSLT-Template schreiben, können Sie dadurch die Texte in die richtige Position setzen. Andernfalls kann es sehr leicht vorkommen, dass Texte über Layoutgrafiken gesetzt werden oder außerhalb des Seitenbereiches rutschen.

[Kategorie:Vorlage für die PDF-Ausgabe erstellen](/Kategorie:Vorlage_für_die_PDF-Ausgabe_erstellen "wikilink")