
Das PDF-Template schreiben Sie ebenso wie Webseiten-Templates in XSLT. Die Ausgabe ist jedoch kein HTML, sondern ein papaya-Formatierungsobjekt. Dieses Formatierungsobjekt ist eine XML-Datei, die vom Aufbau her ein wenig an XSL-FO angelehnt ist. Ähnlich wie bei XSL-FO enthält diese Datei also Anweisungen für den PDF-Prozessor, um die Inhalte in ein entsprechendes PDF zu setzen.

Anders als bei Webseiten-Templates müssen Sie für die PDF-Vorlage einen anderen Ausgabefilter benutzen. Der PDF-Ausgabefilter führt nach dem XSLT-Durchgang den PDF-Prozessor aus, um das PDF-Dokument zu setzen.

Als Ausgangsprojekt für eigene PDF-Templates können Sie das Standard-PDF-Template benutzen, das in den folgenden Unterabschnitten beschrieben wird. Wenn Sie ein eigenes Template erstellen, legen Sie in der Regel folgende XSLT-Templates an:

1.  Template für das Basisdokument erzeugen, siehe [Basisdokument erzeugen](Basisdokument_erzeugen.md).
2.  Zentrale Templates für den Inhaltsbereich definieren, siehe [Zentrales Template für den Inhaltsbereich](Zentrales_Template_für_den_Inhaltsbereich.md).
3.  Template für die Inhalte der Titelseite erzeugen, siehe [Inhalte der Titelseite](Inhalte_der_Titelseite.md).
4.  Template für die Inhalte der Standardseiten (Fließtext mit Bildern und Tabellen), siehe [Inhalte für den Fließtext](/Inhalte_für_den_Fließtext.md).
5.  Template für die Fußzeile anlegen, siehe [Inhalt der Fußzeile](Inhalt_der_Fußzeile.md).
6.  Optional: Template für die Inhalte der Schluss-Seite anlegen, siehe [Inhalte für die letzte Seite](Inhalte_für_die_letzte_Seite.md).

Um das Basisdokument zu erzeugen, müssen Sie zudem folgende Schritte durchführen:

1.  Seitentemplates aus der PDF-Musterdatei anlegen, siehe [Seitentemplates bestimmen](Seitentemplates_bestimmen.md).
2.  Seitenlayout bestimmen, siehe [Seitenlayout bestimmen](Seitenlayout_bestimmen.md).
3.  Schriften auswählen, siehe [Schriftfamilien und -schnitte auswählen](Schriftfamilien_und_-schnitte_auswählen.md).

[Kategorie:Vorlage für die PDF-Ausgabe erstellen](export_de/Kategorie:Vorlage_für_die_PDF-Ausgabe_erstellen.md)
