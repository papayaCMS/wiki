
Phrasen sind Texte für Feldbeschriftungen, Dialogmeldungen, Titel für Abschnitte, Tabellenspalten und Buttons. Während bestimmte Texte automatisch übersetzt werden, müssen Sie in anderen Fällen explizit Übersetzungsmethoden aus der Klasse `base_object` ausführen. Die Klasse `base_object` stellt zu diesem Zweck folgende Methoden zur Verfügung:

1.  `$this->_gt($phrase, $module)`, um eine Phrasen zu übersetzen, siehe [\$this-_gt() benutzen]($this-_gt()_benutzen.md).
2.  `$this->_gtf($phrase, $vals, $module)`, um eine Phrase mit Platzhaltern zu übersetzen, siehe [$this-_gtf() benutzen]($this-_gtf()_benutzen.md).
3.  `$this->_gtfile($fileName)`, um einen längeren Textabschnitt im Backend sprachabhängig auszugeben, siehe [$this-_gtfile() benutzen]($this-_gtfile()_benutzen.md).

Sprachkonventionen bei der Eingabe von Phrasen beachten

Wenn Sie Phrasen eingeben, sollten Sie einige wesentliche Regeln beachten:

1.  Standardsprache ist *American English*. Bei fehlender Übersetzung wird die Phrase aus dem Quelltext genommen. Sie sollten die Phrasen daher nicht in einer anderen Sprache eingeben, da es andernfalls zu Ausgaben mit gemischten Sprachen kommen kann.
2.  Setzen Sie niemals Phrasen aus anderen Phrasen zusammen, wie dies im flogenden Beispiel gemacht wurde: **Schlechtes Beispiel für Phrasenübersetzung**
    ~~~~ {.php}
    $this->label = sprintf(
      $this->_gt('%s has been changed.'),
      $this->_gt('Kategorie')
   .md);
    ~~~~

    Beide Phrasen werden getrennt voneinander übersetzt und anschließend mit `sprintf()` verknüpft. Bei Übersetzungen kann es in bestimmten Sprachen zu Problemen kommen, da bei der getrennten Übersetzung die grammatischen Relationen nicht beachtet werden können. Das obige Beispiel für eine Phrasenübersetzung kann wie folgt verbessert werden: **Gutes Beispiel für Phrasenübersetzung**

    ~~~~ {.php}
    $this->label = $this->_gt(
      'Kategorie hs been changed.'
   .md);
    ~~~~

[Kategorie:Module für das Backend programmieren](export_de/Kategorie:Module_für_das_Backend_programmieren.md)
