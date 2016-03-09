---
title: $this- gtf() benutzen
permalink: /$this-_gtf()_benutzen/
---

Wenn Sie einen Text mit Platzhaltern im Backend sprachabhängig ausgeben möchten, können Sie die Methode `$this->_gtf()` benutzen. „gtf“ steht für „get text formatted“ und funktioniert im Prinzip wie `_gt()`. Der Unterschied besteht darin, dass die Methode so ähnlich wie `sprintf()` funktioniert. Das folgende Listing stellt die Signatur der Methode `base_object::_gtf()` vor:

**Signatur der Methode base_object::_gtf()**

~~~~ {.php}
function _gtf($phrase, $vals, $module = NULL) {
...
}
~~~~

Der erste Parameter `$phrase` enthält den zu übersetzenden Text in englischer Sprache. Der Text enthält Platzhalter, wie sie auch in der klassischen C-Funktion `sprintf()` benutzt werden (beispielsweise `%s` oder `%d` ). Der zweite Parameter `$vals` ist ein Array mit den Werten, die anstelle der Platzhalter in den String hineinformatiert werden sollen. Der dritte Parameter `$module` ist optional und enthält den Modulnamen. Wird kein Modulname angegeben, wird der Scriptname unterhalb von `./papaya/` verwendet.

Das folgende Listing stellt dar, wie mit der Methode `$this->_gtf()` eine Phrase übersetzt wird:

**Phrase mit \$this-\>_gtf() übersetzen**

~~~~ {.php}
...
$msg = $this->_gtf(
  'Do you really want to delete collection "%s" #%d?',
  array(
    $this->collections[$this->params['col_id']]['collection_title'],
    $this->params['col_id']
  )
);
$this->dialog = &new base_msgdialog($this, $this->paramName, $hidden, $msg,
'warning');
...
~~~~

Mit der Methode `$this->_gtf()` wird der Text eines Fragedialogs übersetzt. Als zweiter Parameter wird ein Array übergeben, das die Werte für den String- und den Dezimalplatzhalter in der Phrase enthält. Anschließend wird eine Instanz der Klasse `base_msgdialog` mit der übersetzten Phrase angelegt, die in der Variablen `$msg` gespeichert worden ist.

[Kategorie:Phrasen übersetzen](/Kategorie:Phrasen_übersetzen )