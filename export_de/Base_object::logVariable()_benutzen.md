---
title: Base object::logVariable() benutzen
permalink: /Base_object::logVariable()_benutzen/
---

Im folgenden Beispielprogramm wird der Inhalt einer Variablen durch `base_object::logVariable()` protokolliert. Denkbar wäre der Fall, in dem der Aufruf der Funktion fehlschlägt, mit der die Inhalte einer Variablen in die Datenbank geschrieben werden. Um den Zustand der Variablen näher zu untersuchen, kann man ihren Inhalt in das Systemprotokoll schreiben:

**Variable protokollieren**

~~~~ {.php}
...
$dataSet = $this->createFromXML($myXMLvar);
if (!$this->updateRecord($dataSet)) {
  $this->logVariable(
    MSG_ERROR,
    MSG_LOGTYPE_MODULES,
    "example_class::updateRecord() failed",
    $dataSet,
    TRUE,
    3
  );
}
...
~~~~

[Kategorie:Fehlersuche mit base_object::logVariable()](Kategorie:Fehlersuche_mit_base_object::logVariable() )