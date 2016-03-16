
Box- und Seitenmodule liefern die Inhalte im XML-Format aus. Das XSLT-Template hat die Funktion, dieses XML in ein Formatierungsobjekt umzuwandeln. Das Formatierungsobjekt ist ein spezielles XML-Dokument, in dem festgelegt ist, wie Seiteninhalte, Überschriften, Bilder, Tabellen und sonstige Inhalte formatiert werden sollen. Der PDF-Prozessor von papaya CMS liest dieses Dokument ein und setzt die strukturierten Inhalte in ein PDF-Dokument. Die Inhalte werden dabei anhand der Angaben im Formatierungsobjekt formatiert.

Für das Setzen der Inhalte nutzt der PDF-Prozessor die PDF-Musterdatei als Vorlage. Die PDF-Datei erfüllt dabei im gewissen Sinne die Funktion eines Themes, da sie zum Beispiel Layoutgrafiken enthalten kann. Anders als bei Themes kann eine PDF-Datei jedoch keine Formatierungen oder Schriftfamilien vorgeben, in denen die Inhalte gesetzt werden sollen. Diese Layoutangaben werden direkt im papaya-Formatierungsobjekt geschrieben.

Die HTML-Vorlage enthält für jedes benutzte Seiten- und Boxmodul eine separate XSLT-Datei. Das selbe Prinzip der Modularisierung können Sie auch bei den PDF-Templates anwenden. Allerdings enthält die Standarddistribution von papaya CMS nur ein zentrales Template für die Standardmodule „Kategorie with image“ und „Topic with image“. Je nach Bedarf können Sie jedoch auf einfache Weise eigene Erweiterungen schreiben, wobei Sie das bereits vorhandene XSLT-Template nutzen können.

Verzeichnisstruktur der PDF-Vorlage

Die Vorlage für die PDF-Ausgabe ist im Templateset mit dem Namen `/standard-html/` enthalten. Dieses Verzeichnis enthält neben dem Standardverzeichnis für die HTML-Ausgabe ein Verzeichnis mit dem Namen /pdf/. Dieses Verzeichnis ist wie folgt aufgebaut:

/pdf/
Enthält die Templates für die PDF-Ausgabe.

/pdf/lang/
Enthält XSL-Dokumente mit Templates für sprachspezifische Formatierungen von Zahlen, Datumsangaben und Uhrzeiten.

/pdf/template/
Enthält die PDF-Datei, die als Muster bei der Erzeugung der PDF-Ausgabe dient.

/pdf/fonts/
Enthält zusätzliche Schriftdateien und dazugehörige Schriftmetrikdateien, die von Benutzern erzeugt worden sind.

Wenn Sie ein eigenes PDF-Template erstellen möchten, müssen Sie in Ihrem Template-Verzeichnis die entsprechende Verzeichnisstruktur anlegen:

1.  Im Templateverzeichnis das Unterverzeichnis `/pdf/`
2.  Innerhalb von `/pdf/` das Verzeichnis `/template/` mit der PDF-Musterdatei

[Kategorie:Vorlage für die PDF-Ausgabe erstellen](export_de/Kategorie:Vorlage_für_die_PDF-Ausgabe_erstellen.md)