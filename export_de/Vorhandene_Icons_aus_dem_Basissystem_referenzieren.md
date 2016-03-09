---
title: Vorhandene Icons aus dem Basissystem referenzieren
permalink: /Vorhandene_Icons_aus_dem_Basissystem_referenzieren/
---

Übersicht der Standard-Icons
----------------------------

papaya CMS bietet im Backend eine Übersichtsseite mit den Icons des Basissystems. Sie finden die Übersichtsseite im Bereich „Einstellungen“, wenn Sie auf den Menüpunkt Icons ansehen klicken:

![Files:papaya-iconoverview-places.png](images/papaya-iconoverview-places.png)

Die Tabellenüberschrift bezeichnet die Gruppe. In den Tabellenzellen wird jeweils ein Icon angezeigt und sein Name sowie die verfügbaren Größen angegeben.

Icons in Anwendungen benutzen
-----------------------------

Sie binden Icons in die Anwendung ein, indem Sie einen Bezeichner aus dem Gruppennamen und dem Iconnamen zusammenstellen. Den Bezeichner setzen Sie als Index für das Array `$this->images` ein. Das Array `$this->images` enthält alle Icons des Systems als absolute URLs und ist standardmäßig in allen Modulen enthalten, die Backendausgaben besitzen.

Der zu verwendende Bezeichner setzt sich aus dem Gruppennamen und dem Iconnamen zusammen. Die Namen werden mit Bindestrichen miteinander verbunden werden. Aus „places“ und „network-server“ wird also „ `places-network-server` “.

Icons lassen sich dabei wie folgt beispielsweise für die XML-Ausgabe von Listitems referenzieren:

**Icons in die Backendausgabe einfügen**

~~~~ {.php}
...
$result .= sprintf(
  '<listitem image="%s" title="%s">'.LF,
  $this->images['places-network-server'],
  $title);
...
~~~~

Icons in base_btnbuilder benutzen
----------------------------------

Wenn Sie eine Toolbar mit einer Instanz der Klasse `base_btnbuilder` erstellen, müssen Sie der Klasse eine Referenz auf das `$images` -Array übergeben:

**Referenz auf \$images-Array übergeben**

~~~~ {.php}
$this->menubar = &new base_btnbuilder;
$this->menubar->images = &$this->images;
~~~~

Anschließend können Sie über die Methode `addButton()` der `base_btnbuilder` -Klasse die Toolbar mit Buttons bestücken. Der dritte Parameter bezeichnet das Icon, das verwendet werden soll (falls gewünscht). Es ist hier nicht nötig auf `$this->images` zurückzugreifen. Das wird von `base_btnbuilder` selbständig erledigt.

[Kategorie:Icons für Bearbeitungsmenü oder Adminanwendung benutzen](/Kategorie:Icons_für_Bearbeitungsmenü_oder_Adminanwendung_benutzen )
