---
title: Boxmodule programmieren
permalink: /Boxmodule_programmieren/
---

Ein Boxmodul ist das einfachste Content-Modul in papaya CMS. Es besteht aus einer Klasse, die von der Basisklasse `base_actionbox` abgeleitet ist. Folgende Schritte sind notwendig, um eine eigene Box zu erstellen:

1.  Erstellen Sie eine PHP-Datei und importieren Sie die Klasse `base_actionbox` aus dem Basissystem.
2.  Überladen Sie das Attribut `$preview`, um festzulegen, ob die Box eine Vorschaufunktion unterstützen soll oder nicht.
3.  Überladen Sie das Klassenattribut `$editFields`, um Eingabefelder für das Backend zu erstellen.
4.  Überladen Sie die Funktion `getParsedData()`, um die Ausgabe der Boxinhalte zu bestimmen.
5.  Registrieren Sie die Klasse in der `modules.xml`, siehe [modules.xml erstellen](/modules.xml_erstellen ).

In den folgenden Abschnitten werden die einzelnen Schritte im Detail erläutert.

Klasse base_actionbox einbinden
--------------------------------

Laden Sie mit der PHP-Funktion `require_once()` die Datei, die die notwendige Elternklasse `base_actionbox` enthält. Den Pfad zum Basissystem `papaya-lib/` können Sie aus der Konstanten PAPAYA_INCLUDE_PATH auslesen, die in der `conf.inc.php` definiert ist:

**Die Klasse base_actionbox einbinden**

~~~~ {.php}
<?php

require_once(PAPAYA_INCLUDE_PATH.'system/base_actionbox.php');

class actbox_example extends base_actionbox {
}
?>
~~~~

Leiten Sie Ihre Klasse von der die Basisklasse `base_actionbox` ab.

Attribute \$preview und \$editFields überladen
----------------------------------------------

Das Attribut `$preview` nimmt einen booleschen Wert an und hat die Funktion eines Flags. Hat `$preview` den Wert `TRUE`, kann die Box im Preview-Modus angeschaut werden, andernfalls nicht.

**Das Attribut \$preview überladen**

~~~~ {.php}
<?php

require_once(PAPAYA_INCLUDE_PATH.'system/base_actionbox.php');

class actbox_example extends base_actionbox {
  $preview = TRUE;
}
?>
~~~~

Das Attribut `$editFields` ist ein Array, in dem die Felder für die Eingabemaske „Inhalt bearbeiten“ definiert werden. Diese Eingabemaske kommt zur Anwendung, wenn Sie im Backend von papaya CMS eine Box mit diesem Modul anlegen. Das Array `$editFields` ist ein zweidimensionales Array, dessen erste Ebene die Schlüssel für Eingabemaske und Daten enthält.

**Das Attribut \$editFields überladen**

~~~~ {.php}
<?php

require_once(PAPAYA_INCLUDE_PATH.'system/base_actionbox.php');

class actbox_example extends base_actionbox {
  $preview = TRUE;

  $editFields = array(
    'title' => array('Title', 'isSomeText', FALSE, 'input', 200),
    'text' => array('Text', 'isSomeText', FALSE, 'textarea', 15)
  );
}
?>
~~~~

Die Felder im inneren Array bestimmen die Art des jeweiligen Eingabefeldes. Näheres zum Array `$editFields` erfahren Sie in [Eingabemasken für Inhaltsmodule definieren](/Eingabemasken_für_Inhaltsmodule_definieren ).

Die Schlüssel `title` und `text` werden intern dazu benutzt, um auf die entsprechenden Felder zuzugreifen. Wenn der Nutzer über die Eingabemaske Daten eingibt, stehen diese Daten im Array `$this->data` zur Verfügung. In diesem Fall können die Schlüssel der jeweiligen Felder dazu verwendet werden, die eingegebenen Daten aus dem Array `$this->data` auszulesen.

Die Methode getParsedData() überladen
-------------------------------------

Die Methode `getParsedData()` wird aufgerufen, um den Inhalt des Moduls auszugeben. Durch Überladen der Methode bestimmen Sie den auszugebenden Inhalt:

**Methode getParsedData() überladen**

~~~~ {.php}
<?php

require_once(PAPAYA_INCLUDE_PATH.'system/base_actionbox.php');

class actbox_example extends base_actionbox {
  $preview = TRUE;

  $editFields = array(
    'title' => array('Title', 'isSomeText', FALSE, 'input', 200),
    'text' => array('Text', 'isSomeText', FALSE, 'textarea', 15)
  );

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

Die Inhaltsdaten für eine Box werden vom Benutzer im Backend unter "Inhalt bearbeiten" eingegeben. Das Eingabeformular ist über die `$editFields` definiert. papaya CMS liest diese Daten automatisch aus der Datenbank aus und lädt sie in das assoziative Array `$this->data`. Sie können auf die Daten zuzugreifen, indem Sie mit den Schlüsseln der Felder im Array `$editFields` das Array `$this->data` auslesen.

Wenn Sie die Inhalte der Felder ausgeben, müssen diese maskiert werden. Dafür wird im Quellcode-Beispiel die Methode `papaya_strings::escapeHTMLChars()` verwendet. Für den Haupttext wird die Methode `base_object::getXHTMLString()` verwendet. Näheres dazu erfahren Sie in [:Kategorie:Content ausgeben und Nutzereingaben maskieren](/:Kategorie:Content_ausgeben_und_Nutzereingaben_maskieren ).

Als letzten Schritt tragen Sie das Boxmodul wie in [modules.xml erstellen](/modules.xml_erstellen ) beschrieben in die `modules.xml` ein. Nachdem Sie in der Modulverwaltung den Modulscan durchgeführt haben, wird das neue Modul erkannt und kann verwendet werden.

[Kategorie:Box- und Seitenmodule programmieren](Kategorie:Box-_und_Seitenmodule_programmieren )