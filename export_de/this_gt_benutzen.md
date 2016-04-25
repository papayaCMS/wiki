
Wenn Sie eine einfache Phrase für die Benutzeroberfläche im Backend übersetzen möchten, können Sie die Methode `$this->_gt()` benutzen. „gt“ steht für „get text“ und ist ein einfacher Phrasenübersetzer. Das folgende Listing stellt die Signatur der `_gt()` -Methode vor:

**Signatur der Methode base_object::_gt()**

~~~~ {.php}
function _gt($phrase, $module = NULL) {
...
}
~~~~

Der Parameter `$phrase` enthält den zu übersetzenden Text in englischer Sprache. Der zweite Parameter `$module` ist optional und enthält den Modulnamen. Wird kein Modulname angegeben, wird der Scriptname unterhalb von `./papaya/` verwendet.

Im folgenden Listing wird dargestellt, wie Sie die Methode `_gt()` benutzen können:

**Phrase mit \$this-\>_gt() übersetzen**

~~~~ {.php}
...
$this->addMsg(MSG_WARNING, $this->_gt('Please enter search criteria.'));
...
~~~~

Im obigen Beispiel wird die Phrase eines Warndialogs übersetzt. Dabei wird die Ausgabe von `$this->_gt()` direkt für den zweiten Parameter der Methode `$this->addMsg()` verwendet.

[Kategorie:Phrasen übersetzen](../export_de/Kategorie:Phrasen_uebersetzen.md)
