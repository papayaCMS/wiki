
Nahezu alle Abfragen sind dynamisch und verwenden Platzhalter um kontextabhängige Parameter in die Abfrage einzubringen. Es ist daher nicht möglich eine Abfrage einfach aus dem Quelltext herauszukopieren. Um SQL-Abfragen auszugeben, können Sie die Methode `base_db::databaseDebugNextQuery()` benutzen. Führen Sie diese Methode aus, bevor Sie beispielsweise mit `base_db::databaseQueryFmt()` eine Abfrage ausführen. Wenn Sie mehrere aufeinanderfolgende Abfragen analysieren möchten, können Sie als Parameter die Zahl der auszugebenden Abfragen angeben. Beispielsweise gibt der Aufruf von `databaseQueryFmt(3)` die drei nächsten ausgeführten Abfragen aus, unabhängig von ihrem Klassen-Kontext. Wenn Sie keinen Parameter übergeben, wird nur die nächste Abfrage ausgegeben.

Im folgenden Screenshot wird die Ausgabe einer Debugmeldung mit databaseDebugNextQuery() dargestellt:

[miniatur|zentriert|1000px|Beispiel einer Ausgabe mit base_db::databaseDebugNextQuery()](/images/File:base_db_databasedebugnextquery.png)
=======
![File:Base_db_databasedebugnextquery.png](images/Base_db_databasedebugnextquery.png)
>>>>>>> a2efb5b3261d70ebc0ed214a6131387e209c4f80

In der linken oberen Seite der Ausgabe wird der Name der Klasse dargestellt, die den Aufruf ausgelöst hat. Ferner enthält dieses Feld noch die Nummer der Query und gibt den Verbindungstyp aus. Im obigen Beispiel handelt es sich um eine Leseverbindung („read connection“) In der oberen rechten Seite der Ausgabe steht der Zeitbedarf der Abfrage und in Klammern die Gesamtdauer aller ausgegebenen Abfragen. Anschließend wird die komplette SQL-Abfrage angezeigt. Direkt unterhalb der SQL-Codeausgabe wird ausgegeben, wieviele Datensätze von wievielen möglichen geholt wurden. Es folgt die Ausgabe der `EXPLAIN` -Query und ganz unten das Backtrace.

[Kategorie:Datenbankzugriffe optimieren](export_de/Kategorie:Datenbankzugriffe_optimieren.md)
