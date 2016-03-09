---
title: Variablen mit base object::debug() inspizieren
permalink: /Variablen_mit_base_object::debug()_inspizieren/
---

Sie können die Methode `base_object::debug()` mit beliebig vielen Parametern aufrufen. Wird kein Parameter übergeben, besteht die Ausgabe lediglich aus dem Backtrace (sofern aktiviert) und dem Speicherverbrauch sowie der Zeit seit dem letzten `debug()` -Aufruf. Sie können Strings und Zahlen übergeben, Variablen oder Objekte. Die meisten Klassen, die Sie schreiben, sind indirekt von `base_object` abgeleitet. Daher können Sie `debug()` einfach auf dem aktuellen Objekt mit `$this->debug()` aufrufen. `debug()` lässt sich aber auch als Klassenmethode von `base_object` mit `base_object::debug()` aufrufen.

**Beispielaufrufe von base_object::debug()**

~~~~ {.php}
...
$this->debug();
$this->debug(1);
base_object::debug('test', 1);
$this->debug($this, $var, 'blah');
...
~~~~

Der folgende Screenshot stellt die Ausgabe der Debugaufrufe aus dem obigen Listing dar:

<<<<<<< HEAD
[miniatur|zentriert|1000px|Beispiel einer Ausgabe mit base_object::debug()](/images/File:base_object_debug_beispielausgabe.png )
=======
![File:Base_object_debug_beispielausgabe.png](images/Base_object_debug_beispielausgabe.png)
>>>>>>> a2efb5b3261d70ebc0ed214a6131387e209c4f80

Das Backtrace enthält auch die Zeilenangaben. Das macht es leicht den Programmablauf nachzuvollziehen. Die Angaben„Memory Diff“ und „Time Diff“ beziehen sich auf den jeweils vorhergegangenen `debug()` -Aufruf. Werden mehrere Parameter übergeben, werden Sie direkt nacheinander ausgegeben und das Backtrace wird nur einmal ausgegeben.

Die Ausgabe von Objekten ist üblicherweise sehr lang. In dem Beispiel sehen Sie auch, warum es so wichtig ist, die Debugausgabe auf Livesystemen abzuschalten. Datenbank-Zugangsdaten und Informationen über die Pfadstruktur können ausgegeben werden, sodass potentielle Angreifer ohne viel Aufwand wichtige Informationen über das System erhalten würden.

[Kategorie:Fehlersuche in papaya CMS](export_de/Kategorie:Fehlersuche_in_papaya_CMS )
