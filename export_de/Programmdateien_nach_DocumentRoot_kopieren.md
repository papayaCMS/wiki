
Standardinstallation

Sie können alle Dateien und Verzeichnisse aus dem `files/` -Verzeichnis in das DocumentRoot-Verzeichnis kopieren. Bedenken Sie dabei, dass das Verzeichnis `papaya-data/` beschreibbar sein muss. papaya CMS erstellt in `papaya-data/` das Verzeichnis für die Mediendatenbank, in das Bilder und sonstige Multimediaressourcen gespeichert werden.

Sicherheit erhöhen: papaya-data/ und papaya-lib/ auslagern

Um die Sicherheit Ihrer papaya-Installation zu erhöhen, können Sie die Verzeichnisse `/papaya-data` sowie `/papaya-lib` außerhalb des DocumentRoot-Verzeichnisses unterbringen. Wenn also Ihr DocumentRoot-Verzeichnis `/var/www/htdocs` ist, können Sie für die papaya-Programmbibliothek ein Verzeichnis wie `/var/www/files` anlegen.

papaya CMS in ein Unterverzeichnis installieren

Sie können papaya CMS auch in ein Unterverzeichnis von DocumentRoot installieren. Sie müssen in diesem Fall jedoch die .htaccess-Datei anpassen. Zu diesem Zweck wird papaya CMS mit einer `.htaccess` -Datei ausgeliefert, die Platzhalter für Verzeichnisse enthält. Sie finden diese Templatedatei im Verzeichnis `readme/` unter dem Namen `htaccess.tpl`.

Sie können die Platzhalter mit dem Namen des Unterverzeichnisses austauschen, indem Sie die `htaccess.tpl` mit einen Texteditor Ihrer Wahl öffnen und die Suchen-Und-Ersetzen-Funktion benutzen. Anschließend speichern Sie die Datei im DocumentRoot unter dem Namen `.htaccess` ab.

Unter Linux oder Unix können Sie folgendes elegante Kommando in die Konsole eingeben (Sie müssen sich dazu im `readme/` -Verzeichnis befinden):

**Platzhalter durch Verzeichnisnamen ersetzen**

    <nowiki>sed -e "s#{%webpath_pages%}#Verzeichnisnamen/#g" htaccess.tpl > /absoluter/pfad/zum/DocumentRoot/.htaccess</nowiki>

Der Streameditor sed liest die Templatedatei `htaccess.tpl` ein und führt den angegebenen regulären Ausdruck aus. Der Ausgabestrom wird anschließend in eine Textdatei umgeleitet, die direkt mit dem passenden Namen im DocumentRoot angelegt wird. Näheres zu sed erfahren Sie, wenn Sie „man sed“ in die Konsole eingeben.

[Kategorie:papaya CMS installieren und konfigurieren](export_de/Kategorie:papaya_CMS_installieren_und_konfigurieren.md)