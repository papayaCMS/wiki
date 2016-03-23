
Wenn Sie immer alle Datensätze in einer Listview anzeigen, kann es schnell dazu führen, dass die Listview und damit die Lade- und Renderzeiten sehr lang werden. Sie können mit papaya CMS verhältnismäßig einfach ein Blättern in Listen realisieren.

Das folgende Beispiel stellt vor, wie Sie eine Liste mit einem Paging-Control erweitern können. Dazu wird für die in [Kategorie:Eigene Anwendungen schreiben](export_de/Kategorie:Eigene_Anwendungen_schreiben.md) erstellte Liste von Stickern ein Paging-Control implementiert. Dadurch werden immer nur eine bestimmte Anzahl Sticker je Seite angezeigt.

Um ein Paging-Control zu erstellen, gehen Sie wie folgt vor:

1.  Erstellen Sie die Methode, die das XML für das Paging-Control ausgibt: **XML für Paging-Control ausgeben**
    ~~~~ {.php}
    function getPagingBarXML() {
      if (isset($this->params['col_id'])) {
        include_once(PAPAYA_INCLUDE_PATH.'system/papaya_paging_buttons.php');
        papaya_paging_buttons::getPagingButtons(
          $this,
          NULL,
          $this->params['offset_sticker'],
          $this->params['limit'],
          $this->getNumberOfStickersInCollection($this->params['col_id']),
          9,
          'offset_sticker'
       .md);
      }
    }
    ~~~~

    Die Methode `getPagingBarXML()` gibt das Paging-Control nur dann aus, wenn der Parameter `col_id` gesetzt ist. Ist dies der Fall, wird eine Instanz der Klasse `papaya_paging_buttons` erzeugt.

2.  Laden Sie für die Paging-Toolbar nur den benötigten Teil der Daten. In der Stickers-Anwendung hat die Methode `getStickersByCollection()` entsprechende Parameter, die die Anzahl der Einträge sowie den Startpunkt angeben: **Nur benötigte Daten für eine Seite laden**
    ~~~~ {.php}
    function getStickersList() {
      $result = '';
      if (isset($this->params['col_id']) && $this->params['col_id'] > 0) {
        $stickers = $this->getStickersByCollection(
          $this->params['col_id'],
          $this->params['limit'],
          $this->params['offset_sticker']);
        // ...
      }
      return $result;
    }
    ~~~~

    Die Sticker-Einträge werden in der lokalen Variable `$stickers` gespeichert. Die Angaben für die ID der Sammlung, dem Limit sowie dem Offset werden alle aus dem `$params` -Array gelesen, um den passenden Abschnitt der Stickerliste aus der Datenbank zu laden.

3.  Fügen Sie die XML-Ausgabe der Methode `getPagingBarXML()` unterhalb des Elements `<listview>` ein: **Methode getPagingBarXML() einbinden**
    ~~~~ {.php}
    function getStickersList() {
      $result = '';
      if (isset($this->params['col_id']) && $this->params['col_id'] > 0) {
        $stickers = $this->getStickersByCollection(
          $this->params['col_id'],
          $this->params['limit'],
          $this->params['offset_sticker']);
        $result .= sprintf('<listview title="%s">'.LF,
          papaya_strings::escapeHTMLChars($this->_gt('Stickers')));
        if (is_array($stickers) && count($stickers) > 0) {
          // Including the paging bar
          $result .= $this->getPagingBarXML();
          // ...
        } else {
          $result .= sprintf(
            '<status>%s</status>'.LF,
            $this->_gt('No stickers in this collection.'));
        }
        $result .= '</listview>';
      }
      return $result;
    }
    ~~~~

4.  Halten Sie den Status der Paging-Toolbar in der Session fest. Die Parameter sollten in der Methode `initialize()` gesetzt und initialisiert werden: **Status der Paging-Toolbar in Session speichern**
    ~~~~ {.php}
    function initialize() {
      // ...
      $this->initializeSessionParam('offset_sticker');
      if (empty($this->params['offset_sticker'])) {
        $this->params['offset_sticker'] = 0;
      }
      $this->initializeSessionParam('limit');
      if (empty($this->params['limit'])) {
        $this->params['limit'] = 20;
      }
    }
    ~~~~

    Speichern Sie die verwendeten Parameter in der Session, damit sie nicht bei jedem Methodenaufruf mit übergeben werden müssen. Nähere Information zur Verwendung der Session finden Sie in [:Kategorie:POST/GET-Parameter lesen und Sessiondaten verwalten](export_de/Kategorie:POST/GET-Parameter_lesen_und_Sessiondaten_verwalten.md).

5.  Speichern Sie die Änderungen in den Dateien ab.

Details zur Methode papaya_paging_buttons::getPagingButtons()

Die Methode `papaya_paging_buttons::getPagingButtons()` hat folgende Methodensignatur:

**Signatur der Methode getPagingButtons()**

~~~~ {.php}
function getPagingButtons(
    &$aOwner,
    $baseParams,
    $offset,
    $step,
    $max,
    $groupCount = 9,
    $paramName = 'offset',
    $buttonAlign = NULL)
~~~~

In der folgenden Tabelle sind alle Parameter der Methode `getPagingButtons()` aus der Klasse `papaya_paging_buttons` aufgeschlüsselt:

|Parameter|Bedeutung|
|`&$aOwner`|Basisobjekt|
|`$baseParams`|Linkparameter|
|`$offset`|Aktueller Offset für die Paginierung|
|`$step`|Anzahl der Datensätze pro Seite|
|`$max`|Maximale Anzahl der Datensätze pro Seite|
|`$groupCount`|Maximale Anzahl der Seitenlinks, die dargestellt werden sollen. Standardwert ist „9“.|
|`$paramName`|Name des Formularparameters, in dem der Offset-Wert für den aktuellen Abschnitt der Liste (Seite) festgehalten wird. Der Standardwert ist `offset`.|
|`$buttonAlign`|Ausrichtung des Buttons. Standardwert ist `NULL`. Mögliche Werte sind `left`, `right`, `default`.|

[Kategorie:Widgets für die Benutzeroberfläche einsetzen](export_de/Kategorie:Widgets_für_die_Benutzeroberfläche_einsetzen.md)
