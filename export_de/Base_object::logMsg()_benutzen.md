---
title: Base object::logMsg() benutzen
permalink: /Base_object::logMsg()_benutzen/
---

Sie können `logMsg()` benutzen, um alle möglichen Arten von Informationen zu protokollieren. Beispielsweise könnten Sie in der Anwendung eines Warenwirtschaftssystems eine Warnmeldung in das Systemprotokoll schreiben, wenn die Anzahl der Waren unter einen bestimmten Wert fallen oder wenn alle Waren eines bestimmten Typs ausverkauft sind:

**logMsg() benutzen**

~~~~ {.php}
function getShelfContentXML() {
  $result = '';
  $shelf = $this->loadShelfData();
  if (isset($shelf) && is_array($shelf) && count($shelf) > 0) {
    $result .= '<shelf>';
    foreach ($shelf as $product => $count) {
      $result .= sprintf('<product name="%s" count="%s" />'.LF,
        papaya_strings::escapeHTMLChars($product),
        papaya_strings::escapeHTMLChars($count));
      if ($count = 0) {
        $this->logMsg(MSG_WARNING, MSG_LOGTYPE_MODULES,
          sprintf($this->_gt('No products in stock: "%s"!'), $product),
          sprintf($this->_gt('You don't have any "%s"s. Order new products quickly!'), $product));
      } elseif ($count <= 10) {
        $this->logMsg(MSG_INFO, MSG_LOGTYPE_MODULES,
          sprintf($this->_gt('Low amount of products in stock: "%s".'), $product),
          sprintf($this->_gt('You will soon run out of "%s"s. Please order new products.'), $product));
      }
    }
    $result .= '</shelf>';
  }
}
~~~~

Mit `$this->loadShelfData()` werden die Daten aus der Datenbank geladen. Die Implementation dieser Funktion ist in diesem Tutorial nicht so wichtig. Der Rückgabewert der Funktion ist ein assoziatives Array von Warennamen als Schlüssel und ihrer Anzahl als Wert. Wenn diese Funktion ein Array zurückgibt, wird mit der foreach-Schleife durch die Array-Elemente iteriert. Nach der XML-Ausgabe der Einträge wird überprüft, ob überhaupt noch Waren dieses Typs im Lager sind:

**Warnmeldung in Protokoll eintragen**

~~~~ {.php}
if ($count = 0) {
  $this->logMsg(MSG_WARNING, MSG_LOGTYPE_MODULES,
    sprintf($this->_gt('No products in stock: "%s"!'),
      $product),
    sprintf($this->_gt('You don't have any "%s"s. Order new products quickly!'),
      $product));
~~~~

Wenn nicht, wird eine Warnung ausgelöst, die in das Systemprotokoll geschrieben wird. Die Warnung hat die Gewichtung MSG_WARNING und den Typ MSG_LOGTYPE_MODULES . Mit `$this->_gt()` wird eine Übersetzung für den angegebenen Text aus dem papaya-Phrasensystem geholt.

Wenn weniger als zehn Stück der vorliegenden Ware im Regal vorhanden sind, wird eine Hinweismeldung in das Protokoll geschrieben:

**Infomeldung in Protokoll eintragen**

~~~~ {.php}
} elseif ($count <= 10) {
  $this->logMsg(MSG_INFO, MSG_LOGTYPE_MODULES,
    sprintf($this->_gt('Low amount of products in stock: "%s".'), $product),
    sprintf($this->_gt('You will soon run out of "%s"s. Please order new products.'), $product)
  );
~~~~

In diesem Fall wird MSG_INFO statt MSG_WARNING verwendet. In der Nachrichtenübersicht des Systemprotokolls wird ein anderes Symbol dargestellt.

Durch die unterschiedliche Gewichtung können Sie Nachrichten nach bestimmten Leveln filtern. Näheres dazu erfahren Sie in „papaya CMS 5: Handbuch für Administratoren“, Kapitel 19.

[Kategorie:Meldungen mit base_object::logMsg() protokollieren](/Kategorie:Meldungen_mit_base_object::logMsg()_protokollieren )