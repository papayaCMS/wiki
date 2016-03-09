---
title: Basismodul für Datenbankzugriffe schreiben
permalink: /Basismodul_für_Datenbankzugriffe_schreiben/
---

Bei komplexeren Anwendungen gibt es häufig mehrere Ausgabemodule (Seite/Box), die auf Daten in einer Datenbanktabelle zugreifen müssen. Das Administrationsmodul benötigt lesenden und schreibenden Zugriff auf die Datenbank. Es ist daher günstig, zumindest den Datenbankzugriff in einer separaten Klasse zu abstrahieren, die von den einzelnen Modulen gemeinsam genutzt werden kann. Schreibzugriffe können gegebenenfalls auch im Administrationsmodul implementiert werden. Bei papaya CMS wird die Klasse für den Datenbankzugriff zumeist als Basisklasse bezeichnet. Der Konvention nach beginnen Datei- und Klassenname mit `base_`.

Die Basisklasse soll Sticker-Datensätze und Sammlungsdatensätze erstellen, laden, aktualisieren, und löschen können. Für die Ausgabemodule wird eine Methode bereitgestellt, die alle Sticker einer Sammlung ganz oder abschnittsweise zurückgibt sowie eine Methode, die einen zufälligen Sticker zurückgibt. Jede Sammlung soll einen Titel und eine Beschreibung haben. Jeder Sticker soll Text oder Bild und optional eine Autorenangabe enthalten.

Für die Basisklasse ergeben sich also folgende Anwendungsfälle:

1.  Sticker hinzufügen
2.  Sticker laden
3.  Sticker bearbeiten
4.  Sticker löschen
5.  Sammlung hinzufügen
6.  Sammlung laden
7.  Sammlung bearbeiten
8.  Sammlung löschen
9.  Alle Sticker einer Sammlung laden
10. Eine Auswahl an Sticker laden (mit Start-ID und Offset).
11. Zufälligen Sticker (einer Sammlung) laden

Struktur der Datenbanktabellen
------------------------------

Die Daten für die Anwendung werden auf zwei Tabellen verteilt. Die erste Datenbanktabelle mit dem Namen `papaya_sticker` enthält alle Sticker-Daten:

|Feld|Datentyp|Beschreibung|
|----|--------|------------|
|sticker_id|integer|ID, Primärschlüssel|
|collection_id|integer|ID, externer Schlüssel der Sammlung|
|sticker_text|string|Text des Stickers|
|sticker_image|string|Bild-ID|
|sticker_author|string|Autor des Textes|

Die zweite Datenbanktabelle enthält alle Daten, die zu einer Sammlung gehören:

|Feld|Datentyp|Beschreibung|
|----|--------|------------|
|collection_id|integer|ID, Primärschlüssel|
|collection_title|string|Titel der Sammlung|
|collection_description|string|Beschreibungstext der Sammlung|

Die beiden Tabellen stehen in einer 1-n-Beziehung zueinander. Genau eine Sammlung kann beliebig viele Sticker-Elemente enthalten, während ein Sticker-Element immer nur einer Sammlung angehören kann. Diese Relation wird dadurch festgehalten, dass zu jedem Sticker-Eintrag die ID der zugehörigen Sammlung über das Feld `collection_id` festgehalten wird.

Klassen mit Datenbankzugriff werden grundsätzlich von der Systemklasse `base_db` abgeleitet. Das folgende Listing stellt das Grundgerüst der Klasse `base_stickers` dar, das die oben erwähnten Anwendungsfälle in Form von Methodenrümpfen enthält:

**Grundgerüst der Basisklasse base_stickers**

~~~~ {.php}
require_once(PAPAYA_INCLUDE_PATH.'system/sys_base_db.php');

class base_stickers extends base_db {
  function __construct() {}
  function addSticker($collectionId, &$data) {}
  function addCollection($title, $description) {}
  function updateSticker($collectionId, $stickerId, &$data) {}
  function updateCollection($collectionId, $title, $description) {}
  function deleteSticker($stickerId) {}
  function deleteCollection($collectionId) {}
  function getCollections() {}
  function getStickersByCollection($collectionId, $limit, $offset) {}
  function getSticker($stickerId) {}
  function getRandomSticker($collectionId) {}
}
~~~~

Im folgenden wird nun die Implementation des Konstruktors sowie der einzelnen Methoden vorgestellt.

Konstruktor implementieren
--------------------------

Damit der möglicherweise gesetzte Tabellen-Präfix genutzt wird, müssen Sie Tabellennamen abhängig von der Konstanten PAPAYA_DB_TABLEPREFIX definieren. Eine passende Stelle dafür ist der Konstruktor. Rufen Sie zunächst den Elternkonstruktor auf, damit dort vorgenommene Initialisierungen trotz Überladen durchgeführt werden:

**Konstruktor der Basisklasse**

~~~~ {.php}
function __construct() {
  parent::__construct();
  $this->tableSticker     = PAPAYA_DB_TABLEPREFIX.'_sticker';
  $this->tableCollections = PAPAYA_DB_TABLEPREFIX.'_sticker_collections';
}
~~~~

Soll Ihre Anwendung mit PHP4 kompatibel sein, fügen Sie den PHP4-Konstruktor (entspricht dem Klassennamen) ein. Der Einfachheit halber ruft der PHP4-Konstruktor den PHP5-Konstruktor `__construct()` auf:

**PHP4-Konstruktor der Basisklasse**

~~~~ {.php}
function base_stickers() {
  base_sticker::__construct();
}
~~~~

Der PHP4-Konstruktor delegiert also die Aufgabe an den PHP5-Konstruktor.

Daten zur Datenbank hinzufügen
------------------------------

Verwenden Sie `base_db::databaseInsertRecord()` um einen Datensatz der Datenbanktabelle hinzuzufügen. Der erste Parameter ist der Tabellenname, den Sie im Attribut `$tableSticker` abgelegt haben. Der zweite Parameter ist der Name des Autoincrement-Feldes, dessen Wert Sie zurückbekommen wollen um die Datensatz-Id zu erhalten. Benötigen Sie den Wert nicht, so geben Sie `NULL` an: Sie unterdrücken damit die Abfrage nach der soeben eingefügten ID. Der dritte Parameter enthält die einzufügenden Daten.

Das Daten-Array enthält den Namen der Felder und die dazugehörigen Daten in einer Liste von Schlüssel-Wert-Paaren. Diese Daten werden in die angegebene Tabelle geschrieben.

Außer der Collection-ID werden alle anderen Daten aus dem Parameter `$data` ausgelesen. Sie können also die übermittelten Formularparameter direkt an diese Methode weitergeben. Vorher sollten Sie ggf. überprüfen, ob die Daten gesetzt sind. Verwenden Sie Typecasting um der Datenbankabstraktion einen Wert mit dem richtigen Datentyp zu übergeben.

**Implementation der Methode addSticker()**

~~~~ {.php}
function addSticker($collectionId, &$data) {
  $data = array(
    'collection_id' => (int)$collectionId,
    'sticker_text'  => (string)$data['sticker_text'],
    'sticker_image' => (string)$data['sticker_image'],
    'sticker_authr' => (string)$data['sticker_author'],
 .md);
  $stickerId = $this->databaseInsertRecord(
    $this->tableSticker,
    'sticker_id',
    $data
 .md);
  if ($stickerId) {
    return $stickerId;
  }
  return FALSE;
}
~~~~

Entsprechend wird die Methode `addCollection()` implementiert:

**Implementation der Methode addCollection()**

~~~~ {.php}
function addCollection($title, $description) {
  $data = array(
    'collection_title'       => (string)$title,
    'collection_description' => (string)$description,
 .md);
  $collectionId = $this->databaseInsertRecord(
    $this->tableCollections,
    'collection_id',
    $data
 .md);
  if ($collectionId) {
    return $collectionId;
  }
  return FALSE;
}
~~~~

Daten in der Datenbank ändern
-----------------------------

Verwenden Sie die Methode `base_db::databaseUpdateRecord()`, um einen bestehenden Datensatz zu aktualisieren. Übergeben Sie Tabellennamen, Updatedaten sowie eine SQL-Bedingung als Parameter. Nach einer Plausibilitätsprüfung bauen Sie das Daten-Array wie bei der Methode `addSticker()` auf. Übergeben Sie als Bedingung ein assoziatives Array mit dem Feldnamen `sticker_id` als Schlüssel und der Sticker-ID als Wert. Daraus wird die SQL-Bedingung konstruiert, die bestimmt, welche Datensätze zu ändern sind.

**Implementation der Methode updateSticker()**

~~~~ {.php}
function updateSticker($collectionId, $stickerId, &$data) {
  if ($collectionId > 0 && $stickerId > 0) {
    $data = array(
      'collection_id'  => $collectionId,
      'sticker_text'   => $data['sticker_text'],
      'sticker_image'  => $data['sticker_image'],
      'sticker_author' => $data['sticker_author'],
   .md);
    $condition = array(
      'sticker_id' => $stickerId,
   .md);
    return (FALSE !== $this->databaseUpdateRecord($this->tableSticker, $data, $condition));
  }
}
~~~~

Entsprechend arbeitet die Methode `updateCollection()`:

**Implementation der Methode updateCollection()**

~~~~ {.php}
function updateCollection($collectionId, $title, $description) {
  if ($collectionId > 0 && $title != '') {
    $data = array(
      'collection_title'       => (string)$title,
      'collection_description' => (string)$description,
   .md);
    $condition = array(
      'collection_id' => $this->params['col_id'],
   .md);
    return (FALSE !== $this->databaseUpdateRecord(
      $this->tableCollections, $data, $condition)
   .md);
  }
}
~~~~

Daten aus der Datenbank entfernen
---------------------------------

Um einen Datensatz zu löschen, können Sie die Methode `base_db::databaseDeleteRecord()` benutzen. Diese benötigt als ersten Parameter den Namen der Tabelle, auf die sich die Löschaktion bezieht, als zweiten Parameter die Löschbedingung in Form eines assoziativen Arrays. Der Array-Schlüssel bezeichnet den Feldnamen, im Normalfall der Primärschlüssel, der Array-Wert den Wert dieses Feldes beim zu löschenden Datensatz. Im folgenden Beispiel wird die Methode zum Löschen von Sticker-Einträgen vorgestellt. Die Löschbedingung besteht aus dem Feldnamen `sticker_id` und der ID des zu löschenden Feldes:

**Implementation der Methode deleteSticker()**

~~~~ {.php}
function deleteSticker($stickerId) {
  if ($stickerId > 0) {
    $condition = array('sticker_id' => $stickerId);
    return (FALSE !== $this->databaseDeleteRecord(
      $this->tableSticker, $condition));
  }
}
~~~~

Die Methode zum Löschen einer Sammlung wird ähnlich implementiert. Sie sollten jedoch zunächst alle Sticker der Sammlung löschen, damit keine verwaisten Datensätze zurückbleiben.

**Implementation der Methode deleteCollection()**

~~~~ {.php}
function deleteCollection($collectionId) {
  if ($collectionId > 0) {
    $condition = array('collection_id' => $collectionId);
    if (FALSE !== $this->databaseDeleteRecord($this->tableSticker, $condition)) {
      return (FALSE !== $this->databaseDeleteRecord(
        $this->tableCollections, $condition)
     .md);
    }
  }
}
~~~~

Daten aus der Datenbank auslesen
--------------------------------

Im Folgenden geht es um die Implementation der Ausgabemethoden. Um bestehende Daten verwalten zu können, benötigen Sie in der Stickers-Anwendung eine Liste aller Sammlungen, alle Sticker einer Sammlung und schließlich einzelne Sammlungen oder Sticker zur Bearbeitung.

Für einfache SQL-Abfragen steht Ihnen die Methode `base_db::databaseQueryFmt()` zur Verfügung. Sie Formulieren die SQL-Abfrage dabei in einem String, wobei Sie für den Tabellennamen den `sprintf()` -Platzhalter `%s` einsetzen. Übrigens können Sie beliebige Positionen im SQL mit Platzhaltern besetzen, die durch Parameter ersetzt werden, die der Methode `databaseQueryFmt()` als Array übergeben werden. Als ersten Parameter erwartet die Methode `databaseQueryFmt()` den SQL-String, als zweiten Parameter ein Array mit den Parametern. `databaseQueryFmt()` arbeitet wie die PHP-Funktion `vprintf()` und ersetzt alle Platzhalter im SQL-String durch die Parameter im Parameter-Array.

Die folgende Methode liest alle Sammlungen aus der Tabelle aus. Anstelle des Tabellennamens steht im SQL-String ein Platzhalter, der durch den Tabellennamen im Parameter-Array `$params` ersetzt wird:

**Implementation der Methode getCollections()**

~~~~ {.php}
function getCollections() {
  $result = FALSE;
  $sql = "SELECT collection_id, collection_title, collection_description
            FROM %s
         ";
  $params = array($this->tableCollections);
  if ($res = $this->databaseQueryFmt($sql, $params)) {
    $result = array();
    while ($row = $res->fetchRow(DB_FETCHMODE_ASSOC)) {
      $result[$row['collection_id']] = $row;
    }
  }
  return $result;
}
~~~~

Die Rückgabe von `base_db::databaseQueryFmt()` ist eine Instanz von `dbresult_base` (in `system/db/base.php`.md), die ein einfaches Arbeiten mit der Ergebnismenge erlaubt. Verwenden Sie `dbresult_base::fetchRow()` um den nächsten Datensatz aus dem Ergebnis zu holen. Der Parameter DB_FETCHMODE_ASSOC bewirkt, dass der Feldname als Schlüssel des Ergebnisarrays verwendet wird. Der Standardwert der Parameters, DB_FETCHMODE_DEFAULT , bewirkt, dass die Schlüssel durchnummeriert werden, in der Reihenfolge der Feldnamen im SQL-Befehl.

Beispiel für eine Rückgabe der Methode `getCollections()`:

**Beispielausgabe für die Methode getCollections()**

~~~~ {.php}
array(
  1 => array(
    'collection_id'          => 1,
    'collection_title'       => 'meine Sammlung',
    'collection_description' => 'einige tolle Zitate',
 .md),
  2 => array(
    'collection_id'          => 2,
    'collection_title'       => 'lustige Bilder',
    'collection_description' => 'witzige Bilder, die ich im Internet gefunden habe',
 .md),
);
~~~~

Um alle Sticker einer Sammlung zu erhalten, implementieren Sie z.B. folgende Methode:

**Implementation der Methode getStickersByCollection()**

~~~~ {.php}
function getStickersByCollection($collectionId, $limit, $offset) {
  $result = FALSE;
  if ($collectionId > 0) {
    $sql = "SELECT sticker_id, collection_id, sticker_text, sticker_image,
                   sticker_author
              FROM %s
             WHERE collection_id = %d
             ORDER BY sticker_id ASC
          ";
    $params = array($this->tableSticker, $collectionId);
    if ($res = $this->databaseQueryFmt($sql, $params, $limit, $offset)) {
      $result = array();
      while ($row = $res->fetchRow(DB_FETCHMODE_ASSOC)) {
        $result[$row['sticker_id']] = $row;
      }
      $this->absCount = $res->absCount();
    }
  }
  return $result;
}
~~~~

Wenn Sie eine Bedingung in die SQL-Abfrage einbauen, verwenden Sie als Platzhalter bei Strings `%s` mit einfachen Anführungszeichen, bei Zahlen `%d`. Im obigen Beispiel wird eine numerische SQL-Bedingung benutzt, für die ein entsprechender Platzhalter `%d` gesetzt ist. Für diese SQL-Bedingung wird an das `$params` -Array der Wert `$collectionId` angehängt. Die Reihenfolge der Platzhalter in der Abfrage muss der Reihenfolge der Parameter entsprechen.

Die Parameter `$limit` und `$offset` werden als dritten und vierten Parameter der Methode `databaseQueryFmt()` übergeben. `$limit` beinhaltet die maximale Anzahl auszulesender Datensätze, während `$offset` angibt, um wie viele Datensätze versetzt die Abfrage Datensätze zurückliefern soll. Die Methode `databaseQueryFmt()` kümmert sich dabei um die DBMS-spezifische Verwendung der Abfrageeinschränkungen.

Beispiel für eine Rückgabe der Methode `getStickersByCollection()`:

**Beispielausgabe für die Methode getStickersByCollection()**

~~~~ {.php}
array(
  1 => array(
    'sticker_id'     => 1,
    'collection_id'  => 2,
    'sticker_text'   => 'Exploits of a Mom',
    'sticker_image'  => '263d7b3d1e0fbac620d0c5c634bcb2ed',
    'sticker_author' => 'xkcd',
 .md),
  2 => array(
    'sticker_id'     => 2,
    'collection_id'  => 1,
    'sticker_text'   => 'It is by the fortune of God that, in this country,
we have three benefits: freedom of speech, freedom of thought, and the wisdom never to use either.',
    'sticker_image'  => '',
    'sticker_author' => 'Mark Twain',
 .md),
);
~~~~

Das folgende Listing stellt die Implementation der `getSticker()` -Methode vor. Diese Methode liest genau einen Stickereintrag anhand der übergebenen Sticker-ID aus der Datenbank aus:

**Implementation der Methode getSticker()**

~~~~ {.php}
function getSticker($stickerId) {
  $result = FALSE;
  if ($stickerId > 0) {
    $sql = "SELECT sticker_id, collection_id, sticker_text, sticker_image,
                   sticker_author
              FROM %s
             WHERE sticker_id = %d
             ORDER BY sticker_id ASC
          ";
    $params = array($this->tableSticker, $stickerId);
    if ($res = $this->databaseQueryFmt($sql, $params)) {
      return $res->fetchRow(DB_FETCHMODE_ASSOC);
    }
  }
  return $result;
}
~~~~

Im Unterschied zu `getStickersByCollection()` bezieht sich die `WHERE` -Bedingung hier auf die `sticker_id`, und nicht auf die `collection_id`. Da wir nur einen Datensatz zurückbekommen, können wir die Rückgabe von `base_db::fetchRow()` mit `return` direkt weitergeben.

Für die spätere Boxausgabe muss ein zufälliger Sticker aus der Datenbank geladen werden. Das folgende Listing stellt die Implementation einer entsprechenden Datenbankabfrage vor:

**Implementation der Methode getRandomSticker()**

~~~~ {.php}
function getRandomSticker($collectionId) {
  $result = FALSE;
  $randomOrder = $this->databaseGetSQLSource('RANDOM');

  $sql = "SELECT sticker_id, collection_id, sticker_text,
                 sticker_image, sticker_date, sticker_author,
                 sticker_source, sticker_source_url, sticker_explanation
            FROM %s
           WHERE collection_id = %d
           ORDER BY $randomOrder
         ";
  $params = array($this->tableSticker, $collectionId);
  if ($res = $this->databaseQueryFmt($sql, $params, 1)) {
    return $res->fetchRow(DB_FETCHMODE_ASSOC);
  }
  return $result;
}
~~~~

Der einzige Parameter `$collectionId` der Methode ist die ID der Sammlung, zu der der Sticker gehört. Mit der Methode `base_db::databaseGetSQLSource()` wird die DBMS-spezifische Variante der Zufallsfunktion ausgelesen. Bei MySQL ist der Rückgabewert `RAND()`. Übergeben Sie der Methode `base_db::databaseQueryFmt()` als dritten Parameter (maximale Anzahl der angeforderten Datensätze) die Zahl 1. Dadurch wird die Rückgabe auf einen Datensatz ( `LIMIT 1`.md) beschränkt.

Damit ist die Klasse `base_stickers` vollständig, sodass dem Einsatz im Administrationsmodul nichts mehr im Wege steht.

[Kategorie:Eigene Anwendungen schreiben](export_de/Kategorie:Eigene_Anwendungen_schreiben.md)