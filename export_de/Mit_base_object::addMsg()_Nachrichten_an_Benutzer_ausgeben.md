---
title: Mit base object::addMsg() Nachrichten an Benutzer ausgeben
permalink: /Mit_base_object::addMsg()_Nachrichten_an_Benutzer_ausgeben/
---

Dieser Abschnitt erklärt, wie Sie mit `base_object::addMsg()` Benachrichtigungen an Benutzer ausgeben können. Die Nachricht wird im Content-Bereich der Seite als Dialog angezeigt. Das folgende Listing stellt vor, wie die Funktion aufgerufen wird:

**Nachrichten an Benutzer ausgeben**

~~~~ {.php}
$this->addMsg(
  MSG_INFO,
  sprintf($this->_gt('Added new product "%s".'), $this->params['product_name'])
);
~~~~

Der erste Parameter ist einer der bekannten Nachrichtenlevel, siehe Tabelle "Fehlerebenen für Nachrichten im Systemprotokoll" in [Funktionsweise von base_object::logMsg()](/Funktionsweise_von_base_object::logMsg() ). Der zweite Parameter erhält den Nachrichtentext, der mit `$this->_gt()` in die aktuelle Content-Sprache des Backends übersetzt wird.

Meldung zusätzlich im Fehlerprotokoll protokollieren
----------------------------------------------------

Wenn Sie die Nachricht zusätzlich noch ins Systemprotokoll eintragen möchten, müssen Sie zwei weitere Parameter anfügen:

**Benutzer-Nachrichten zusätzlich ins Systemprotokoll schreiben**

~~~~ {.php}
$this->addMsg(
  MSG_INFO,
  sprintf($this->_gt('Added new product "%s".'), $this->params['product_name']),
  TRUE,
  PAPAYA_LOGTYPE_MODULES
);
~~~~

Der dritte Parameter, standardmäßig `FALSE`, bestimmt, ob die Nachricht auch in das Systemprotokoll geschrieben werden soll. Dazu müssen Sie also `TRUE` übergeben.

Details zur Methode base_object::addMsg()
------------------------------------------

Die Methode `base_object::addMsg()` ist mit folgenden Parametern definiert:

**Methodensignatur von base_object::addMsg()**

~~~~ {.php}
function addMsg($level, $text, $log = FALSE, $type = PAPAYA_LOGTYPE_SYSTEM) {
...
}
~~~~

|Parameter|Bedeutung|
|---------|---------|
|`$level`|Fehlerebene, siehe Tabelle "Fehlerebenen für Nachrichten im Systemprotokoll" in [Funktionsweise von base_object::logMsg()](/Funktionsweise_von_base_object::logMsg() ).|
|`$text`|Text der Meldung. Gegen Sie nicht direkt den Text der Meldung ein, sondern übersetzen Sie diesen mit `$this->_gt()`.|
|`$log`|"TRUE", wenn die Nachricht auch in das Systemprotokoll geschrieben werden soll, andernfalls "FALSE" (Standardeinstellung).|
|`$type`|Auswahl eines Nachrichtentyps, siehe Tabelle "Nachrichtentypen aus base_object" in [Funktionsweise von base_object::logMsg()](/Funktionsweise_von_base_object::logMsg() ). Standard ist PAPAYA_LOGTYPE_SYSTEM .|

[export_de/Kategorie:Fehlersuche in papaya CMS](export_de/Kategorie:Fehlersuche_in_papaya_CMS )