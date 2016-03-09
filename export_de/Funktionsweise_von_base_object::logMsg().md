---
title: Funktionsweise von base object::logMsg()
permalink: /Funktionsweise_von_base_object::logMsg()/
---

Die Methode `logMsg()` ist in der Klasse `base_object` implementiert. `logMsg()` verwendet seinerseits eine globale Instanz von `base_log` um die Daten in die Datenbank zu schreiben. Diese Funktion kann von Jeder Klasse genutzt werden, die von `base_object` abgeleitet ist. Klassen, die nicht von `base_object` abstammen, können die Methode `logMsg()` statisch aufrufen ( `base_object::logMsg()` ).

Das folgende Listing stellt vor, wie Sie `logMsg()` aufrufen können:

**Aufruf der Methode logMsg()**

~~~~ {.php}
$this->logMsg(
  $msgLevel,
  $msgType,
  $overviewMsg,
  $detailMsg,
  $addBacktrace,
  $backtraceOffset);
~~~~

Die Methode `logMsg()` verfügt über folgende Parameter:

|Log-Level|Bedeutung|
|---------|---------|
|`$msgLevel`|Grad der Fehlermeldung, siehe Tabelle "Fehlerebenen für Nachrichten im Systemprotokoll" in [Funktionsweise von base_object::logMsg()](/Funktionsweise_von_base_object::logMsg() ).|
|`$msgType`|Dieser Parameter beschreibt den Nachrichtentyp, siehe Tabelle "Nachrichtentypen aus base_object" in [Funktionsweise von base_object::logMsg()](/Funktionsweise_von_base_object::logMsg() ).|
|`$overviewMsg`|Kurzversion der Nachricht. Dieser Text erscheint in der Übersichtsliste im Protokoll.|
|`$detailMsg`|Ausführliche Nachricht. Dieser Text enthält alle Informationen, um den Fehler zu identifizieren oder die Nachricht möglichst informativ zu gestalten.|
|`$addBacktrace`|„TRUE“, wenn die Log-Nachricht ein Backtrace der Funktionsaufrufe erhalten soll, andernfalls „FALSE“.|
|`$backtraceOffset`|Integer-Wert, der den Offset im Backtrace angibt. Damit der Aufruf von `logMsg()` selbst nicht im Backtrace erscheint, kann ein Offset angegeben werden. Standard ist „2“.|

Fehler-Level
------------

Die Fehler-Level sind in `sys_error.php` definiert und im Prinzip selbsterklärend. In der folgenden Tabelle werden sie aufgeschlüsselt:

|Log-Level|Bedeutung|
|---------|---------|
|`MSG_INFO`|Einfache Infomeldungen werden in diesem Level protokolliert.|
|`MSG_WARNING`|In diesem Level werden Warnmeldungen protokolliert, die auf potentielle Probleme wie eine falsche Konfiguration oder fehlende Daten hinweisen.|
|`MSG_ERROR`|Eindeutige Fehler wie Query-Fehler in der Datenbank oder sonstige Inkonsistenzen werden in diesem Level protokolliert.|

Nachrichtentypen
----------------

Die Nachrichtentypen sind in der Datei `sys_base_object.php` definiert. Die folgende Tabelle schlüsselt alle Nachrichtentypen auf:

|Log-Level|Bedeutung|
|---------|---------|
|`PAPAYA_LOGTYPE_USER`|Logging von Benutzer-Ereignissen, beispielsweise Login oder Logout.|
|`PAPAYA_LOGTYPE_PAGES`|Logging von Seiten-Ereignissen, beispielsweise Veröffentlichen oder Privatstellen von Seiten.|
|`PAPAYA_LOGTYPE_DATABASE`|Logging von Datenbankfehlern, beispielsweise Query-Fehler durch fehlerhaftes SQL.|
|`PAPAYA_LOGTYPE_CALENDAR`|Logging für Kalender-Meldungen.|
|`PAPAYA_LOGTYPE_CRONJOBS`|Logging für papaya-Cronjobs.|
|`PAPAYA_LOGTYPE_SURFER`|Logging für Surfer und Community-Ereignisse, beispielsweise Login oder Logout.|
|`PAPAYA_LOGTYPE_SYSTEM`|Fehler in einer Klasse des Basissystems.|
|`PAPAYA_LOGTYPE_MODULES`|Fehler in eigenen Modulen.|

[Kategorie:Meldungen mit base_object::logMsg() protokollieren](Kategorie:Meldungen_mit_base_object::logMsg()_protokollieren )