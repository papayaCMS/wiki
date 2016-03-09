---
title: Eigene Icons aus dem Paket referenzieren
permalink: /Eigene_Icons_aus_dem_Paket_referenzieren/
---

Für eigene Anwendungen ist es manchmal wünschenswert, spezielle Icons zu verwenden, die nicht im Basissystem enthalten sind. Um solche Icons zu verwenden muss lediglich die Quellenangabe des Icons etwas anders aussehen als für Icons im Basissystem (siehe [Vorhandene Icons aus dem Basissystem referenzieren](/Vorhandene_Icons_aus_dem_Basissystem_referenzieren ) ).

Die Angabe für das Bild setzt sich aus dem Schlüsselwort `module:`, der GUID des Moduls sowie der relativen Pfadangabe zur Icon-Datei ( `./pics/icon.png` ) zusammen:

**Beispielausgabe für die Iconreferenz**

~~~~ {.xml}
<glyph src="module:012345679abcdef012345679abcdef/pics/my_icon.png" />
<listitem image="module:012345679abcdef012345679abcdef/pics/my_other_icon.gif" />
~~~~

Da es nicht besonders einfach ist, den Pfad zusammenzustellen, steht in der Klasse `base_module` die Methode `getIconURI()` zur Verfügung. Sie brauchen dieser Klasse lediglich den relativen Pfad zur Bilddatei zu übergeben. Die vollständige URL wird durch die Methode erzeugt.

Die Stickers-Klasse `admin_stickers` erhält beispielsweise bei ihrer Instanziierung eine Referenz auf die `edmodule_stickers` -Instanz. Die Klasse `edmodule_stickers` wiederum erweitert die Klasse `base_module`, in der die Methode `getIconURI()` implementiert ist. Im folgenden Beispiel wird dargestellt, wie mit dieser Methode das Icon eines Pakets eingebunden wird:

**Icon-URL mit base_module::getIconURI() erzeugen**

~~~~ {.php}
..
$result .= sprintf('<glyph src="%s" />'.LF,
  $this->parentObj->getIconURI('pics/icon.png'));
...
~~~~

Die Icons selbst werden im Unterverzeichnis `./pics/16x16/`, `./pics/22x22/` sowie `./pics/48x48/` abgelegt. papaya CMS findet das Bild automatisch im Ordner mit der entsprechenden Größenangabe.

[export_de/Kategorie:Icons für Bearbeitungsmenü oder Adminanwendung benutzen](export_de/Kategorie:Icons_für_Bearbeitungsmenü_oder_Adminanwendung_benutzen )