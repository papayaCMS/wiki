---
title: Konnektoren - Schnittstellen für andere Anwendungen schaffen
permalink: /Konnektoren_-_Schnittstellen_für_andere_Anwendungen_schaffen/
---

Wenn Ihre Anwendung Schnittstellen für andere Applikationen in papaya CMS anbieten soll, können Sie dazu eigene Konnektoren benutzen. Für die Beispielanwendung "Stickers" wird im folgenden vorgestellt, wie eine solche Schnittstelle aussehen kann, über die andere Anwendungen eine Übersicht der Sammlungen oder eine Anzahl Sticker abfragen können, ohne die interne Struktur der Stickers-Anwendung zu kennen.

Um eine Konnektorklasse zu erstellen, gehen Sie wie folgt vor:

1.  Erstellen Sie eine Klasse `connector_stickers`, die von `base_plugin` abgeleitet ist. Eine Konnektorklasse sollte immer den Namenspräfix `connector` haben: **Konnektorklasse erstellen**
    ~~~~ {.php}
    require_once(PAPAYA_INCLUDE_PATH.'base_plugin.php');

    class connector_stickers extends base_plugin {

    }
    ~~~~

2.  Erstellen Sie den Konstruktor der Konnektorklasse. Im Konstruktor sollte die Basisklasse mit den Datenbankmethoden eingebunden und als Attribut `$this->stickersObj` instanziiert werden: **Konstruktoren der Konnektorklasse**
    ~~~~ {.php}
    function __construct() {
      parent::__construct();
      include_once(dirname(__FILE__).'/base_stickers.php');
      $this->stickersObj = &new base_stickers;
    }
    ~~~~

3.  Erstellen Sie alle Schnittstellen-Methoden in der Klasse, die sie zur Verfügung stellen wollen. Im folgenden Beispiel sind das die Methoden `getCollections()`, `getStickersByCollection()` und `getRandomSticker()`, die die entsprechenden Methoden auf dem Basisobjekt ausführen: **Schnittstellenmethoden implementieren**
    ~~~~ {.php}
    function getCollections() {
      return $this->stickersObj->getCollections();
    }

    function getStickersByCollection($collectionId, $limit, $offset) {
      return $this->stickersObj->getStickersByCollection($collectionId, $limit, $offset);
    }

    function getRandomSticker($collectionId) {
      return $this->stickersObj->getRandomSticker($collectionId);
    }
    ~~~~

4.  Speichern Sie die Datei ab. Denken Sie daran, dass sich die Namenskonvention auch auf den Dateinamen auswirkt, sodass der Dateiname ebenso wie die Klasse mit dem Präfix `connector_` beginnen sollte.
5.  Registrieren Sie die Klasse als Konnektor in der `modules.xml`. Näheres dazu erfahren Sie in [modules.xml erstellen](/modules.xml_erstellen ).

Basisklasse anpassen
--------------------

Manchmal ist es notwendig, die Basisklasse zu erweitern, weil der Konnektor zusätzliche Funktionen zur Verfügung stellen soll. Um die Anzahl der Sticker in einer Sammlung beispielsweise für eine seitenweise Darstellung zu ermitteln, können Sie die Basisklasse um entsprechende Funktionen erweitern. Dazu gehen Sie wie folgt vor:

1.  Erweitern Sie die Methode `base_stickers::getStickersByCollection()` um das Attribut `absCount`, in das die komplette Anzahl aller Sticker gespeichert wird. Die Methode `connector_stickers::getStickersByCollection()` muss also entsprechend erweitert werden: **Schnittstellenmethode getStickersByCollection() um Zähler erweitern**
    ~~~~ {.php}
    function getStickersByCollection($collectionId, $limit, $offset) {
      if ($stickers = $this->stickersObj->getStickersByCollection($collectionId, $limit, $offset)) {
        $this->_stickersCount[$collectionId] = $this->stickersObj->absCount;
        return $stickers
      } else {
       $this->_stickersCount[$collectionId] = 0;
      }
    }
    ~~~~

    Wenn die Abfrage erfolgreich war, wird die Anzahl der ausgelesenen Sticker im Attribut `$_stickersCount` gespeichert. Falls die Abfrage fehlschlägt, wird der Wert des Attributs auf 0 gesetzt (die Sammlung ist leer).

2.  Erweitern Sie die Klasse `connector_stickers` um die Methode `getNumberOfStickersInCollection()`: **Schnittstellenmethode getNumberOfStickersInCollection() implementieren**
    ~~~~ {.php}
    function getNumberOfStickersInCollection($collectionId) {
      if (!isset($this->_stickersCount[$collectionId])) {
        $this->_stickersCount[$collectionId] = $this->getStickersCount($collectionId);
      }
      if (isset($this->_stickersCount[$collectionId])) {
        return $this->_stickersCount[$collectionId];
      }
      return FALSE;
    }
    ~~~~

    Wurden bereits Sticker aus der Sammlung mit `getStickersByCollection()` abgefragt, ist die Anzahl der Sticker im Attribut `$_stickersCount` abgelegt. Andernfalls ermitteln Sie die Anzahl, indem Sie die Methode `base_stickers::getStickersCount()` aufrufen.

3.  Erweitern Sie die Klasse `base_stickers` um die Methode `base_stickers::getStickersCount()`: **base_stickers um Methode getStickersCount() erweitern**
    ~~~~ {.php}
    function getStickersCount($collectionId) {
      $sql = "SELECT COUNT(*) AS count
                FROM %s
               WHERE collection_id = %d
             ";
      $params = array($this->tableSticker, $collectionId);
      if ($res = $this->databaseQueryFmt($sql, $params)) {
        return $res->fetchField();
      }
      return FALSE;
    }
    ~~~~

    Die Methode fragt die Anzahl der Datensätze in der Tabelle `papaya_stickers` ab, deren `collection_id` der angegebenen `$collectionId` entspricht. Die Abfrage liefert nur einen Datensatz und nur ein Feld. Sie können daher die Auswertung der Abfrage vereinfachen, indem Sie statt `base_db::fetchRow()` und der Rückgabe des Feldwertes mit `$row['count']` die Methode `base_db::fetchField()` verwenden.

4.  Passen Sie in der Konnektorklasse connector_stickers die Methode getRandomSticker() an. Erweitern Sie die Methoden dazu um den optionalen Parameter `$limit`: **Konnektormethode getRandomSticker() anpassen**
    ~~~~ {.php}
    function getRandomSticker($collectionId, $limit = 1) {
      return $this->stickersObj->getRandomSticker($collectionId, $limit);
    }
    ~~~~

5.  Erweitern Sie die Methode `base_stickers::getRandomSticker()` um den Parameter `$limit` und passen Sie die Abfrage an: **Methode getRandomSticker() in base_stickers anpassen**
    ~~~~ {.php}
    function getRandomSticker($collectionId, $limit = 1) {
      $result = FALSE;

      $randomOrder = $this->databaseGetSQLSource('RANDOM');

      $sql = "SELECT s.sticker_id, s.collection_id, s.sticker_text,
                     s.sticker_image, s.sticker_date, s.sticker_author,
                     s.sticker_source, s.sticker_source_url, s.sticker_explanation
                FROM %s AS s
               WHERE s.collection_id = %d
               ORDER BY $randomOrder
             ";

      $params = array($this->tableSticker, $collectionId);
      if ($res = $this->databaseQueryFmt($sql, $params, (int)$limit)) {
        return $res->fetchRow(DB_FETCHMODE_ASSOC);
      }
      return $result;
    }
    ~~~~

    Die Änderungen beschränken sich auf den Parameter `$limit` mit dem Defaultwert 1. Damit wird die Funktionsweise der Methode zur vorhergehenden Version nicht verändert. Zusätzlich wird die Variable `$limit` in der Datenbankfunktion `base_db::databaseQueryFmt()` als dritter Parameter eingesetzt. Da die Auswahl zufällig ist, ist die Angabe des Offsets als vierter Parameter nicht sinnvoll anwendbar.

6.  Speichern Sie Ihre Änderungen ab.

[export_de/Kategorie.md:Eigene Anwendungen schreiben](export_de/Kategorie.md:Eigene_Anwendungen_schreiben )