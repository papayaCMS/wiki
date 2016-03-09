---
title: $this- gtfile() benutzen
permalink: /$this-_gtfile()_benutzen/
---

Wenn Sie einen längeren Text im Backend sprachabhängig ausgeben möchten, können Sie die Methode `$this->_gtfile()` benutzen. Diese Methode liest Textdateien ein, die im sprachspezifischen Verzeichnis innerhalb des papaya-Verzeichnisses gespeichert sind.

**Signatur der Methode base_object::_gtfile()**

~~~~ {.php}
function _gtfile($fileName){
...
}
~~~~

Der Parameter `$fileName` enthält den Namen der Datei, die sich im Verzeichnis `PAPAYA_PATH_WEB.PAPAYA_PATH_ADMIN.'/data/'.ISO-LNG` befindet. Beispielsweise ist die Datei `sitemap_info.txt` bei aktivierter deutscher Backendsprache unter folgendem Pfad zu finden:

**Beispielpfad für die Datei sitemap_info.txt**

~~~~ {.php}
./papaya/data/de-DE/sitemap_info.txt
~~~~

Die Methode `_gtFile()` wird wie folgt aufgerufen:

**Sprachspezifische Textdatei mit \$this-\>_gtfile() einbinden**

~~~~ {.php}
$this->layout->addRight(
  sprintf(
    '<sheet width="350" align="center">'.
    '<text><div style="padding: 0px 5px 0px 5px; ">'.
    '%s</div></text></sheet>',
    $this->_gtfile('linkdb_info.txt')
);
~~~~

Sie müssen lediglich den Namen der Textdatei angeben. Der Pfad bis zur Datei wird durch die Implementation der Methode automatisch ergänzt.

[export_de/Kategorie:Phrasen übersetzen](export_de/Kategorie:Phrasen_übersetzen )