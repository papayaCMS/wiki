---
title: Funktionsweise von base object::logVariable()
permalink: /Funktionsweise_von_base_object::logVariable()/
---

Das folgende Listing stellt die Signatur der `base_object::logVariable()` -Methode vor:

**Signatur der Methode base_object::logMsg()**

~~~~ {.php}
function logVariable($level, $type, $title, $variable, $addBacktrace = FALSE, $backtraceOffset = 3) {
...
}
~~~~

Die Funktion `base_object::logVariable()` verfügt über folgende Parameter:

|Log-Level|Bedeutung|
|---------|---------|
|`$level`|Grad der Fehlermeldung, siehe Tabelle "Fehlerebenen für Nachrichten im Systemprotokoll" in [Funktionsweise von base_object::logMsg()](/Funktionsweise_von_base_object::logMsg() ).|
|`$type`|Dieser Parameter beschreibt den Nachrichtentyp, siehe Tabelle "Nachrichtentypen aus base_object" in [Funktionsweise von base_object::logMsg()](/Funktionsweise_von_base_object::logMsg() ).|
|`$title`|Titel der Variablenausgabe. Dieser Text erscheint in der Übersichtsliste im Protokoll.|
|`$variable`|Referenz zur Variablen, deren Inhalt in das Protokoll geschrieben werden soll.|
|`$addBacktrace`|„TRUE“, wenn die Log-Nachricht ein Backtrace der Funktionsaufrufe erhalten soll, andernfalls „FALSE“ (Standardeinstellung).|
|`$backtraceOffset`|Integer-Wert, der den Offset im Backtrace angibt. Damit der Aufruf von `base_object::logMsg()` selbst nicht im Backtrace erscheint, kann ein Offset angegeben werden. Standard ist „3“.|

[export_de/Kategorie:Fehlersuche mit base_object::logVariable()](export_de/Kategorie:Fehlersuche_mit_base_object::logVariable() )