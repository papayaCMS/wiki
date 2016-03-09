---
title: Strings für Datenbankabfrage maskieren
permalink: /Strings_für_Datenbankabfrage_maskieren/
---

Strings werden automatisch durch Methoden der Datenbankabstraktion maskiert, bevor eine SQL-Abfrage stattfindet. In manchen Fällen sind Sie jedoch selber dafür verantwortlich, Nutzereingaben zu maskieren.

Wenn Sie `base_db::databaseQueryFmt()` verwenden, werden Parameter automatisch maskiert, die über die `vprintf()` -ähnliche Funktionsweise übergeben werden und nicht direkt im `$sql` stehen. Sie müssen weiterhin auf korrekte SQL-Syntax achten und prüfen, ob Werte für Bedingungen leer sind. Sie müssen aber Anführungszeichen und dergleichen nicht maskieren.

**Codebeispiel für eine einfache Datenbankabfrage**

~~~~ {.sql}
$sql = "SELECT my_id, my_title, my_description
          FROM %s
         WHERE my_title = '%s'
       ";
$params = array($this->tableMyItems, $myTitle);
if ($res = $this->databaseQueryFmt($sql, $params)) {
  $result = $res->fetchRow(DB_FETCHMODE_ASSOC);
}
~~~~

Wenn Sie komplexere Bedingungen wie `IN ()` verwenden möchten, verwenden Sie die Methode `base_db::databaseGetSQLCondition('field_name',
    $values)`. Mit dieser Methode erhalten Sie eine gültige, maskierte SQL-Bedingung. Eine vollständige Erklärung der Funktionsweise von `base_db::databaseGetSQLCondition()` finden Sie in der Quellcodedokumentation.

Das folgenden Listing stellt eine beispielhafte Anwendung der Methode databaseGetSQLCondition() vor:

**Codebeispiel für base_db::databaseGetSQLCondition()**

~~~~ {.php}
$titleList = array('one item', "joe's item", 'best item');

$myCondition = $this->databaseGetSQLCondition('my_title', $titleList);

$sql = "SELECT my_id, my_title, my_description
          FROM %s
         WHERE $myCondition
       ";
$params = array($this->tableMyItems);
// ...
~~~~

Die SQL Abfrage sieht dann wie folgt aus:

**Beispielausgabe einer SQL-Abfrage mit maskierten Werten**

~~~~ {.sql}
SELECT my_id, my_title, my_description
  FROM papaya_my_items
 WHERE my_title IN ('one item', 'joe\'s item', 'best item');
~~~~

Datensätze in die Datenbank einfügen
------------------------------------

Wenn Sie Datensätze mit `base_db::databaseInsertRecords()` einfügen oder mit `base_db::databaseUpdateRecord()` ändern, brauchen Sie sich um das Maskieren ebenfalls keine Gedanken zu machen.

**Automatisches Maskieren bei databaseInsertRecord()**

~~~~ {.php}
$data = array(
  'my_title' => 'item 4',
  'my_description' => "The 'best' item ever",
);
$this->databaseInsertRecord($this->tableMyItems, NULL, $data);
~~~~

Wenn Sie komplexe SQL-Abfragen selbst zusammensetzen möchten und es mit den Bordmitteln nicht möglich ist die Abfrage zu erstellen, müssen Sie Zeichenketten gegebenenfalls selber maskieren. Verwenden Sie in solch einem Fall die Methode `base_db::escapeStr()` um DBMS-abhängig die korrekte Maskierung zu erhalten.

[Kategorie:Content ausgeben und Nutzereingaben maskieren](/Kategorie:Content_ausgeben_und_Nutzereingaben_maskieren )