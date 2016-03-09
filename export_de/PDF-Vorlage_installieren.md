---
title: PDF-Vorlage installieren
permalink: /PDF-Vorlage_installieren/
---

Um die Vorlage zu installieren, gehen Sie wie folgt vor:

1.  Legen Sie ggf. in Ihrem Templateverzeichnis in `/papaya-data/templates/standard-html/` die Verzeichnisse für die PDF-Vorlage an. Näheres zur Verzeichnisstruktur erfahren Sie in [PDF-Vorlagenkonzept](/PDF-Vorlagenkonzept ).
2.  Kopieren Sie die PDF-Musterdatei in das Unterverzeichnis `/pdf/template`.
3.  Kopieren Sie das XSLT-Template in das Verzeichnis `/pdf`.
4.  Wenn Sie zusätzliche Fonts erzeugt haben, kopieren Sie diese in das Verzeichnis `/pdf/fonts`. Anstelle von `/fonts` können Sie auch jeden anderen beliebigen Namen benutzen. Dieser muss jedoch der Angabe im Layoutbereich des Formatierungsobjekts entsprechen, siehe [Schriftfamilien und -schnitte auswählen](/Schriftfamilien_und_-schnitte_auswählen ).
5.  Erstellen Sie einen Ausgabefilter für PDF. Als Dateiendung empfiehlt sich `.pdf`, der Content-Type ist `application/pdf`.

Sie können nun Ansichten mit dem Ausgabefilter für PDF verknüpfen. Das Standardtemplate enthält ein XSLT-Stylesheet für die Seitenmodule „export_de/Kategorie with image“ und „Topic with image“.

[export_de/Kategorie:Vorlage für die PDF-Ausgabe erstellen](export_de/Kategorie:Vorlage_für_die_PDF-Ausgabe_erstellen )