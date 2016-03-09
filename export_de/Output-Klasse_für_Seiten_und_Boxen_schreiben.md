---
title: Output-Klasse für Seiten und Boxen schreiben
permalink: /Output-Klasse_für_Seiten_und_Boxen_schreiben/
---

Wenn Seiten- und Boxmodule umfangreicher werden, kann es sein, dass Sie eigene Methoden zur Inhaltskonfiguration verwenden. Auch die Ausgabemöglichkeiten können sich vervielfältigen, so dass die Klasse sehr groß wird. Das ist aus mehreren Gründen ungünstig:

1.  Bei der Ausgabe wird der Code für die Konfiguration mitgeparst.
2.  Bei der Administration wird der Code für die Ausgabe mitgeparst.
3.  Die Klasse wird unübersichtlicher.

Der weitaus häufigere Fall ist der, dass die Ausgabelogik umfangreicher wird. Wenn die Ausgabelogik in eine separate Klasse ausgelagert wird, muss dieser Code beim Konfigurieren der Seite im Backend nicht mehr geladen werden. Zudem kann es sein, dass Sie mehrere Ausgabemodule erstellen wollen, die teilweise das selbe XML ausgeben. Eine zentrale Output-Klasse kann in diesem Fall Redundanzen vermeiden und die Performance verbessern.

Grundaufbau der Output-Klasse
-----------------------------

Die Output-Klasse kann beispielsweise von der Klasse `base_stickers` abgeleitet werden. Damit erbt sie alle Lademethoden der Elternklasse und kann weitere spezielle Ladefunktionen implementieren und die Ausgabe für die Seitendarstellung aufbereiten.

**Grundaufbau einer Output-Klasse für die Stickers-Anwendung**

~~~~ {.php}
...
class output_stickers extends base_stickers {

  function initialize(&$data) {
    $this->data = &$data;
  }

  function getOutput() {
    $result = '';
    if ($stickers = $this->getStickersByCollection($this->data['collection_id'])) {
      $result .= '<stickers>'.LF;
      foreach ($stickers as $stickerId => $sticker) {
        $result .= sprintf('<sticker id="%d">%s</sticker>'.LF, $stickerId,
          papaya_strings::getXHTMLString($sticker['sticker_text']));
      }
      $result .= '</stickers>'.LF;
    }
    return $result;
  }
}
...
~~~~

Die Methode `output_stickers::getOutput()` liefert eine Liste aller Sticker-Einträge einer Sammlung als XML-Dokument zurück. Dabei wird die `getStickersByCollection()` -Methode der Klasse `base_stickers` benutzt.

Je nachdem, welche verschiedenen Ausgaben geplant sind, lassen sich verschiedene Anwendungsfälle in unterschiedliche Ausgabemethoden implementieren. Beispielsweise ließe sich eine `getRandomOutput()` -Methode implementieren, um einen Zufallssticker für die Boxenausgabe auszugeben.

Einbindung der Output-Klasse in Seiten- oder Boxmodule
------------------------------------------------------

Um die Klasse eines Content-Moduls zu implementieren, instanziieren Sie die Output-Klasse und führen die `getOutputMethode()` aus:

**Seitenmodul mit output-Klasse**

~~~~ {.php}
...
require_once(PAPAYA_INCLUDE_PATH.'system/base_content.php');

class content_stickers extends base_content {

  var $editFields = array(
    // ...
 .md);

  function getParsedTeaser() {
    // ...
  }

  function getParsedData() {
    include_once(dirname(__FILE__).'/output_stickers.php');
    $outputObj = &new output_stickers;
    $outputObj->initialize($this->data);
    $outputObj->getOutput();
  }

}
...
~~~~

Die gesamte Logik für die Ausgabe sowie die Erzeugung der XML-Ausgaben wird nur noch eingebunden und ausgeführt, wenn die Klasse zur Ausgabe verwendet wird, nicht wenn der Seiteninhalt konfiguriert wird.

[Kategorie:Eigene Anwendungen schreiben](export_de/Kategorie:Eigene_Anwendungen_schreiben.md)