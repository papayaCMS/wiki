---
title: Text bei der Ausgabe maskieren
permalink: /Text_bei_der_Ausgabe_maskieren/
---

In der Klasse `base_object` ist die Methode `getXHTMLString()` implementiert, mit der Sie ein gültiges XHTML-Dokument ausgeben können. Diese Methode sorgt dafür, dass auf jeden Fall ein gültiges XML-Dokument ausgegeben wird, indem bestimmte Sonderzeichen wie `&` oder `ä` in entsprechende HTML-Entitys umgewandelt werden. Tags bleiben jedoch erhalten, ebenso wie bereits definierte Entities.

Wird der Funktion als zweiten Parameter `TRUE` übergeben, werden alle im String enthaltenen Zeilenumbrüche in `<br
    />` -Elemente umgewandelt. Das folgende Beispiel zeigt, wie die Methode aufgerufen werden kann:

**Codebeispiel für base_object::getXHTMLString()**

~~~~ {.php}
...
function getParsedData() {
  ...
  $result .= sprintf(
    '<teaser>%s</teaser>'.LF,
    $this->getXHTMLString($this->data['teaser'], !(bool)$this->data['nl2br'])
  );
...
}
...
~~~~

Im obigen Beispiel wird `getXHTMLString()` in der Funktion `getParsedData()` benutzt, um einen String zu maskieren, der in ein XML-Element eingefügt werden soll. Die Methode `getXHTMLString()` garantiert, dass der einzufügende String auf jeden Fall gültiges XHTML ist. Zu diesem Zweck führt `getXHTMLString()` intern eine Überprüfung durch, indem der String mit der Klasse `simple_xmltree` getestet wird. Falls dieser Test fehlschlägt, wird der komplette String maskiert, indem die Methode `escapeHTMLChars()` aus der Klasse `papaya_strings` auf den String angewendet wird.

Wann Sie getXHTMLString() und wann escapeHTMLChars() benutzen sollten
---------------------------------------------------------------------

Wenn eine Benutzereingabe HTML-Zeichen enthalten darf, müssen Sie die Methode `base_object::getXHTMLString()` anwenden. Diese Methode erstellt gültiges XML und erhält gleichzeitig die enthaltenen HTML-Tags.

Ist für die Benutzereingabe HTML nicht vorgesehen, müssen Sie `papaya_strings::escapeHTMLChars()` verwenden. Dadurch stellen Sie sicher, dass alle HTML-Sonderzeichen maskiert werden.

Welche Ausgabemethode für die Maskierung benutzen, hängt auch von den eingesetzten Plausibilitätschecks ab. Wenn Sie Eingabeformulare über `$editFields` oder mit `base_dialog` erzeugen, wählen Sie für jedes Feld auch einen Plausibilitätscheck aus. Diese Überprüfungsmethoden aus der Klasse `checkit ` bestimmen mehr oder weniger restriktiv, welche Werte in Felder eingegeben werden dürfen. Ist als Prüffunktion „isSomeText“ angegeben, `sollten Sie
    base_object::getXHTMLString()` für die Ausgabemaskierung benutzen. Ist als Prüffunktion „isNoHTML“ oder restriktiver (isNum, isAlpha und dergleichen) angegeben, reicht `papaya_strings::escapeHTMLChars()`. Letzteres ist performanter, da das XML nicht validiert werden muss.

[Kategorie:Content ausgeben und Nutzereingaben maskieren](export_de/Kategorie:Content_ausgeben_und_Nutzereingaben_maskieren )