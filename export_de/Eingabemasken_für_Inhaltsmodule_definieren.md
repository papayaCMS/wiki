---
title: Eingabemasken für Inhaltsmodule definieren
permalink: /Eingabemasken_für_Inhaltsmodule_definieren/
---

Eingabemasken für Box- und Seitenmodule werden über ein assoziatives Array definiert. Die notwendige Funktionalität dazu erben die beiden Basisklassen `base_actionbox` und `base_content` von der gemeinsamen Elternklasse `base_plugin`. Um eigene Eingabefelder zu definieren, müssen Sie das Klassenattribut `$editFields` überladen. Die Ausgabe der Eingabemaske erledigt das System anschließend von selbst.

Im folgenden Listing wird die Struktur des Arrays `$editFields` aufgeführt:

**Ausfüllschema für \$editFields-Array**

    <nowiki>editFields [
      feldname => array[
        Feldtitel,
        Testfunktion,
        Pflichtfeld? (TRUE|FALSE),
        Feldtyp,
        Feldparameter (hängt vom Typ ab),
        Hilfstext,
        Standardwert
      ],
      Zwischenüberschrift
    ]</nowiki>

Die folgende Tabelle beschreibt die einzelnen Felder:

|Feld|Bedeutung|
|----|---------|
|Feldtitel|Beschriftung des Feldes. Die Beschriftung wird in englischer Sprache eingegeben und durch das Phrasensystem von papaya CMS übersetzt.|
|Testfunktion|Der Plausibilitätscheck, mit dem die Nutzereingabe überprüft wird.|
|Pflichtfeld|"TRUE", wenn dieses Feld ausgefüllt werden muss, andernfalls "FALSE".|
|Feldtyp|Typ des Eingabefeldes, beispielsweise einzeilige oder mehrzeilige Textfelder, siehe Tabelle "Feldtypen und ihre möglichen Parameter" in [Eingabemasken für Inhaltsmodule definieren](/Eingabemasken_für_Inhaltsmodule_definieren.md).|
|Feldparameter|Je nach ausgewähltem Feldtyp können entsprechende Parameter eingefügt werden, siehe Tabelle "Feldtypen und ihre möglichen Parameter" in [Eingabemasken für Inhaltsmodule definieren](/Eingabemasken_für_Inhaltsmodule_definieren.md).|
|Hilfstext|Dieser Hilfstext wird als title-Attribut im Glühbirnen-Icon dargestellt, das hinter die Feldbeschriftung gesetzt wird.|
|Standardwert|Standardwert für dieses Feld. Die Standardwerte werden über die Methode `$this->setDefaultData()` in das Array `$this->data` geladen und stehen im Content-Modul zur Verfügung.|

Ein konkretes Beispiel für ein ausgefülltes `$editField` -Array sehen Sie im folgenden Listing:

**Beispiel für \$editFields**

~~~~ {.php}
$editFields = array(
  'max' => array('Count', 'isNum', TRUE, 'input', 5, '', 100),
  'columns' => array('Column count', 'isNum', TRUE, 'input', 1, '', 2),
  'sort' => array('Sort', 'isNum', TRUE, 'translatedcombo',
    array(0 => 'Ascending', 1 => 'Descending', 2 => 'Random'), '', 0),
  'Image',
  'image' => array('Image', 'isSomeText', FALSE, 'image', 400, '', ''),
  'imgalign' => array('Image align', 'isAlpha', TRUE, 'combo',
    array('left' => 'left', 'right' => 'right'), '', 'left'),
  'breakstyle' => array('Text float', 'isAlpha', TRUE, 'combo',
    array('none' => 'None', 'side' => 'Side', 'center' => 'Center'), '', 'none'),
);
~~~~

Jedes einzelne Feld im `$editFields` -Array besteht aus einem Schlüssel, das auf ein eingebettetes Array zeigt. Das eingebettete Array enthält die eigentliche Felddefinition. Neben diesen Schlüssel-Array-Paaren kann das Array auch einfache Strings enthalten. Die Strings werden in der Eingabemaske als Zwischenüberschriften dargestellt. Im obigen Listing stellt `Image` eine solche Zwischenüberschrift dar. Das Array aus dem obigen Listing würde ein Eingabeformular erzeugen, das folgendermaßen aussieht:

<<<<<<< HEAD
[miniatur|zentriert|1000px|Eingabemaske des Seitenmoduls „Kategorie with image“](/images/File:editFieldsExample.png)
=======
![File:EditFieldsExample.png](images/EditFieldsExample.png)
>>>>>>> a2efb5b3261d70ebc0ed214a6131387e209c4f80

Feldtypen
---------

In der folgenden Tabelle sind alle Feldtypen und ihre Parameter aufgelistet:

|Feldtyp|Beschreibung|Parameter|
|-------|------------|---------|
|captcha|Dynamisches Bild für Captcha auswählen.|ID des dynamischen Bildes.|
|checkbox|Checkbox ausgeben|Beschriftung der Checkbox.|
|checkgroup|Eine Gruppe von Checkboxen für die Mehrfachauswahl.|Assoziatives Array, das den Titel sowie den Parameternamen der Checkboxen enthält, die als Gruppe zusammengefasst werden.|
|combo|Drop-Down-Liste mit verschiedenen Einträgen.|Assoziatives Array mit den einzelnen Auswahloptionen. Die Auswahloptionen können gruppiert werden, indem das assoziative Array weitere eingebettete Arrays enthält.|
|color|Eingabefeld mit optionalem Farbauswahldialog.|Maximale Zeichenlänge der Eingabe. Der Parameterwert wird sowohl für das Attribut `maxlength` als auch für das Attribut `size` benutzt.|
|date|Eingabefeld mit optionalem Datumsauswahldialog.|Maximale Zeichenlänge der Eingabe. Der Parameterwert wird sowohl für das Attribut `maxlength` als auch für das Attribut `size` benutzt.|
|dircombo|Drop-Down-Liste mit Verzeichnissen aus dem Dateisystem des Servers.|Ein Array bestehend aus drei Elementen:

1.  Pfad zu einem Zielverzeichnis, ausgehend vom Basisverzeichnis.
2.  Basisverzeichnis ( `page`, `admin` oder `upload` ; oder ein absolutes Verzeichnis auf dem Dateisystem des Servers)
3.  Regulärer Ausdruck, der die Liste der anzuzeigenden Dateien beschränkt.|
|file|Dateiupload-Feld, identisch mit `imagefile`.|Maximale Zeichenlänge der Eingabe. Der Parameterwert wird sowohl für das Attribut `maxlength` als auch für das Attribut `size` benutzt.|
|filecombo|Drop-Down-Liste mit Dateiauswahl.|Array mit vier Feldern für folgende Daten:

1.  Pfad: Relativer Pfad ausgehend vom Basisverzeichnis im Dateisystem des Servers.
2.  Regulärer Ausdruck: Nur Dateien auflisten, auf die der reguläre Ausdruck trifft.
3.  TRUE, wenn in der Drop-Down-Liste auch ein leerer Eintrag für die Nullauswahl vorhanden sein soll, andernfalls FALSE (Standard)
4.  Basisverzeichnis (optional): Eines von folgenden Kürzeln `theme` (absoluter Pfad zum papaya-Theme), `page` (absoluter Pfad zum Installationsverzeichnis innerhalb von DocumentRoot von papaya CMS), `admin` (wie `page`, nur innerhalb des Admin-Verzeichnisses von papaya CMS), `upload` (absoluter Pfad zum Uploadverzeichnis der MediaDB); standardmäßig wird eine Callback-Methode erwartet.|
|function|Das Feld wird durch eine Callback-Funktion bestimmt.|Name einer Callback-Funktion, die beispielsweise die Auswahloptionen für Drop-Down-Listen zurückgibt.|
|geopos|Eingabefeld mit optionalem Dialog zur Auswahl von Geo-Positionsdaten über den Google Maps Api-Server.|Wert für das `size` -Attribut des Eingabefeldes.|
|image|Eingabefeld mit optionalem Dialog zur Formatierung und Auswahl von Bilddateien aus der MediaDB.|Wert für das `size` -Attribut des Eingabefeldes.|
|imagefile|Dateiupload-Feld, identisch mit `file`.|Maximale Zeichenlänge der Eingabe. Der Parameterwert wird sowohl für das Attribut `maxlength` als auch für das Attribut `size` benutzt.|
|imagefixed|Eingabefeld mit optionalem Dialog, um Bilddateien direkt aus der MediaDB auszuwählen.|Wert für das `size` -Attribut des Eingabefeldes.|
|info|Einfaches Textlabel zur Information ohne Bearbeitungsmöglichkeit.|n/a|
|input|Einzeiliges Texteingabefeld.|Maximale Zeichenlänge der Eingabe. Der Parameterwert wird sowohl für das Attribut `maxlength` als auch für das Attribut `size` benutzt.|
|mediafile|Eingabefeld mit optionalem Dialog, um eine Datei aus der MediaDB auswählen.|Wert für das `size` -Attribut des Eingabefeldes.|
|mediafolder|Drop-Down-Liste mit allen Ordnern der MediaDB.|n/a|
|mediaimage|Eingabefeld mit optionalem Dialog, über das ein Bild aus der MediaDB ausgewählt werden kann.|Assoziatives Array mit den Schlüsseln `width` und `height`, wodurch Breite und Höhe des angezeigten Bildes bestimmt werden.|
|pageid|Eingabefeld mit optionalem Dialog, um eine Seiten-ID auszuwählen.|Maximale Zeichenlänge der Eingabe. Der Parameterwert wird sowohl für das Attribut `maxlength` als auch für das Attribut `size` benutzt.|
|password|Passwortfeld. Eingaben werden maskiert.|Maximale Zeichenlänge der Eingabe. Der Parameterwert wird sowohl für das Attribut `maxlength` als auch für das Attribut `size` benutzt.|
|radio|Radiobutton|Assoziatives Array mit dem Schlüssel als Buttonwert (value-Attribut) und dem Array-Wert als Button-Beschriftung.|
|richtext|Mehrzeiliges Textfeld mit Richtext-Editor.|Maximale Anzahl der Zeilen.|
|simplerichtext|Mehrzeiliges Textfeld mit einfachem Richtext-Editor|Maximale Anzahl der Zeilen.|
|textarea|Mehrzeiliges Textfeld.|Maximale Anzahl der Zeilen.|
|translatedcombo|Drop-Down-Liste, dessen Einträge durch die Phrasenkomponente übersetzt werden.|Assoziatives Array mit Schlüssel-Wert-Paaren. Der Schlüssel wird als Wert für das `value` -Attribut des `<option>` -Elements benutzt, der Wert als Beschriftungstext. Der Beschriftungstext wird durch das Phrasensystem von papaya CMS übersetzt.|
|yesno|Zwei Radiobuttons für Ja-Nein-Auswahl.|n/a|

[Kategorie:Box- und Seitenmodule programmieren](export_de/Kategorie:Box-_und_Seitenmodule_programmieren.md)
