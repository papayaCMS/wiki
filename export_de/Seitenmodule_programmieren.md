
Ein Seitenmodul zu entwickeln ist fast so einfach wie das Entwickeln eines Boxmoduls. Da die Standardfunktionen bereits in den Basisklassen implementiert sind, müssen Sie lediglich die Methoden `getParsedTeaser()` und `getParsedData()` sowie das Attribut `$editFields` überladen. Folgende Schritte sind notwendig, um ein Seitenmodul zu erstellen:

1.  Erstellen Sie eine PHP-Datei und importieren Sie die Klasse `base_content` aus dem Basissystem.
2.  Überladen Sie das Klassenattribut `$editFields`, um ein Eingabeformular für das Backend zu erstellen.
3.  Überladen Sie die Methode `getParsedData()`, um die Inhalte für die Seite auszugeben.
4.  Überladen Sie die Methode `getParsedTeaser()`, um die Inhalte für den Teaser auszugeben.
5.  Registrieren Sie die Klasse in der `modules.xml`, siehe [modules.xml erstellen](/modules.xml_erstellen.md).

In den folgenden Abschnitten werden die einzelnen Schritte im Detail erläutert.

Einbinden der Klasse base_content in das eigene Seitenmodul

Laden Sie die Datei der notwendigen Elternklasse `base_content` über die PHP-Funktion `require_once()`. Den Pfad zum Basissystem `papaya-lib/` können Sie aus der Konstanten PAPAYA_INCLUDE_PATH auslesen, die in der `conf.inc.php` definiert ist:

**Die Klasse base_content einbinden**

~~~~ {.php}
<?php

require_once(PAPAYA_INCLUDE_PATH.'system/base_content.php');

class content_example extends base_content {
}
?>
~~~~

Das Attribut `$editFields` ist ein Array, in dem die Felder für die Eingabemaske „Inhalt bearbeiten“ definiert werden. Diese Eingabemaske kommt zur Anwendung, wenn Sie im Backend von papaya CMS eine Seite mit diesem Modul anlegen. Das Array `$editFields` ist ein zweidimensionales Array, dessen erste Ebene die Schlüssel für Eingabemaske und Daten enthält.

**Das Attribut \$editFields überladen**

~~~~ {.php}
<?php

require_once(PAPAYA_INCLUDE_PATH.'system/base_content.php');

class content_example extends base_content {

  $editFields = array(
    'title'  => array('Title', 'isSomeText', TRUE, 'input', 200, '', ''),
    'teaser' => array('Teaser', isSomeText, FALSE, 'simplerichtext', 5, '', ''),
    'text'   => array('Text', 'isSomeText', FALSE, 'richtext', 15, '', '')
 .md);
}
?>
~~~~

Die Felder im inneren Array bestimmen die Art des jeweiligen Eingabefeldes. Näheres zum Array `$editFields` erfahren Sie in [Eingabemasken für Inhaltsmodule definieren](/Eingabemasken_für_Inhaltsmodule_definieren.md).

Die Schlüssel `title` und `text` werden intern dazu benutzt, auf die entsprechenden Felder zuzugreifen. Wenn der Nutzer über die Eingabemaske Daten eingibt, stehen diese Daten im Array `$this->data` zur Verfügung. In diesem Fall können die Schlüssel der jeweiligen Felder dazu verwendet werden, die eingegebenen Daten aus dem Array `$this->data` auszulesen.

Die Methode getParsedData() überladen

Die Methode `getParsedData()` wird aufgerufen, um den Inhalt des Moduls auszugeben. Durch Überladen der Methode bestimmen Sie den auszugebenden Inhalt:

**Methode getParsedData() überladen**

~~~~ {.php}
<?php

require_once(PAPAYA_INCLUDE_PATH.'system/base_content.php');

class content_example extends base_content {

  $editFields = array(
    'title'  => array('Title', 'isSomeText', TRUE, 'input', 200, '', ''),
    'teaser' => array('Teaser', isSomeText, FALSE, 'simplerichtext', 5, '', ''),
    'text'   => array('Text', 'isSomeText', FALSE, 'richtext', 15, '', '')
 .md);

  function getParsedData() {
    $this->setDefaultData();
    $result = '<example>'.LF;
    $result .= sprintf('<title>%s</title>'.LF,
      papaya_strings::escapeHTMLChars($this->data['title']));
    $result .= sprintf('<text>%s</text>'.LF,
      $this->getXHTMLString($this->data['text']));
    $result .= '</example>'.LF;
    return $result;
  }
}
?>
~~~~

Die Inhaltsdaten für eine Seite werden vom Benutzer im Backend unter "Inhalt bearbeiten" eingegeben. Das Eingabeformular ist über die `$editFields` definiert. papaya CMS liest diese Daten automatisch aus der Datenbank aus und lädt diese in das assoziative Array `$this->data`. Sie können auf die Daten zugreifen, indem Sie mit den Schlüsseln der Felder im Array `$editFields` das Array `$this->data` auslesen.

Wenn Sie die Inhalte der Felder ausgeben, müssen diese maskiert werden. Dafür wird im Quellcode-Beispiel die Methode `papaya_strings::escapeHTMLChars()` verwendet. Für den Haupttext wird die Methode `base_object::getXHTMLString()` verwendet. Näheres dazu erfahren Sie in [:Kategorie:Content ausgeben und Nutzereingaben maskieren](/:export_de/Kategorie:Content_ausgeben_und_Nutzereingaben_maskieren.md).

Die Methode `getParsedTeaser()` ist ähnlich aufgebaut wie `getParsedData()`. Der Unterschied besteht darin, dass `getParsedTeaser()` von speziellen Übersichtsseiten und bestimmten Boxen aufgerufen wird, um die Seite anzuteasern. Daher gibt diese Funktion immer einen Kurztext aus. Darüber hinaus ist diese Methode optional. Sie muss anders als `getParsedData()` nicht durch Seitenmodule implementiert werden, damit das Seitenmodul Inhalte ausgeben kann.

**Methode getParsedTeaser() überladen**

~~~~ {.php}
<?php

require_once(PAPAYA_INCLUDE_PATH.'system/base_content.php');

class content_example extends base_content {

  $editFields = array(
    'title'  => array('Title', 'isSomeText', TRUE, 'input', 200, '', ''),
    'teaser' => array('Teaser', isSomeText, FALSE, 'simplerichtext', 5, '', ''),
    'text'   => array('Text', 'isSomeText', FALSE, 'richtext', 15, '', '')
 .md);

  function getParsedData() {
    $this->setDefaultData();
    $result = '<example>'.LF;
    $result .= sprintf('<title>%s</title>'.LF,
      papaya_strings::escapeHTMLChars($this->data['title']));
    $result .= sprintf('<text>%s</text>'.LF,
      $this->getXHTMLString($this->data['text']));
    $result .= '</example>'.LF;
    return $result;
  }

  function getParsedTeaser() {
    $this->setDefaultData();
    $result = '';
    $result .= sprintf('<title>%s</title>'.LF,
       papaya_strings::escapeHTMLChars($this->data['title']));
    $result .= sprintf('<teaser>%s</teaser>'.LF,
      $this->getXHTMLString($this->data['text']));
    return $result;
  }
}
?>
~~~~

Beim Seitenaufruf gibt die Methode `getParsedTeaser()` selbst keine Daten aus. Die Funktion wird immer dann aufgerufen, wenn die Seite von Übersichtsseiten oder -boxen angeteasert wird. Eine Übersichtsseite ist beispielsweise eine Seite mit dem Modul „Kategorie with Image“. Dieses Modul ruft die Methode `getParsedTeaser()` aller unmittelbaren Unterseiten auf und stellt sie in einer verlinkten Übersicht dar.

Als letzten Schritt tragen Sie das Seitenmodul wie in [modules.xml erstellen](/modules.xml_erstellen.md) beschrieben in die `modules.xml` ein. Nachdem Sie in der Modulverwaltung den Modulscan durchgeführt haben, wird das neue Modul erkannt und kann verwendet werden.

[Kategorie:Box- und Seitenmodule programmieren](export_de/Kategorie:Box-_und_Seitenmodule_programmieren.md)