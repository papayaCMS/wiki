---
title: Schriftfamilien und -schnitte auswählen
permalink: /Schriftfamilien_und_-schnitte_auswählen/
---

Der PDF-Ausgabefilter von papaya CMS wird mit einer Reihe von Standardfonts ausgeliefert. Die Fonts liegen zusammen mit ihren Fontmetrikdateien im folgenden Verzeichnis:

`/papaya-lib/modules/free/pdf/external/fpdf/font`

Wenn Sie eigene Schriften verwenden möchten, müssen Sie folgende Punkte beachten:

1.  Sie müssen True-Type-Fonts benutzen. Die Schriftdateien müssen Sie in ein Format transformieren, das durch die FPDF-API lesbar ist. Näheres dazu erfahren Sie auf der Website von FPDF unter <http://www.fpdf.org> .
2.  Sie müssen entsprechende Fontmetrikdateien erzeugen.
3.  Die Fontdateien können Sie zusammen mit den Fontmetrikdateien im Template-Verzeichnis unter `/fonts` einfügen. Näheres zur Verzeichnisstruktur der PDF-Vorlage erfahren Sie in [PDF-Vorlagenkonzept](/PDF-Vorlagenkonzept ).
4.  Die Schriften werden über `<font>` -Elemente in die Stylesheetdatei eingebunden.

Der PDF-Ausgabefilter benutzt automatisch die Standardfonts, sofern die von Ihnen angegebenen Fonts nicht gefunden werden können.

Einbinden der Schriften in die Stylesheetdatei
----------------------------------------------

Die Schriftfamilien und die entsprechenden fetten, kursiven und fettkursiven Schriftschnitte werden im `<fonts>` -Element definiert. Das `<fonts>` -Element enthält dazu eine Reihe von `<font>` -Elementen. Ein `<font>` -Element definiert genau eine Schriftfamilie. Mit `<font>` wählen Sie über das Attribut `name` eine Schriftfamilie aus. Über die Attribute `default`, `bold`, `italic` und `bolditalic` werden dabei die Fontmetrikdateien für die jeweiligen Schriftschnitte der Familie ausgewählt. Der folgende Ausschnitt zeigt ein Beispiel für die Fontdefinition aus der Stylesheetdatei des Demotemplates:

**Schriften definieren**

~~~~ {.xml}
...
<fonts>
  <font name="ninjapenguin"
        default="fonts/NINJP.php"
        bold="fonts/NINJP.php"
        italic="fonts/NINJP.php"
        bolditalic="fonts/NINJP.php" />
  <font name="libertine"
        default="fonts/LinLibertine_R-2.2.0.php"
        bold="fonts/LinLibertine_Bd-2.2.0rc10.php"
        italic="fonts/LinLibertine_It-2.2.0rc1.php"
        bolditalic="fonts/LinLibertine_BdIt-2.1.6v2.php" />
  <font name="gentium"
        default="fonts/GenR102.php"
        bold="fonts/GenAR102.php"
        italic="fonts/GenI102.php"
        bolditalic="fonts/GenAI102.php" />
  <font name="charis"
        default="fonts/CharisSILR.php"
        bold="fonts/CharisSILB.php"
        italic="fonts/CharisSILI.php"
        bolditalic="fonts/CharisSILBI.php"/>
</fonts>
...
~~~~

Ausgewählten Tags eine Schrift zuordnen
---------------------------------------

Sie können bestimmten Elementen wie `<p>` oder `<h1>` eine eigene Schrift zuordnen. Sie können dabei die Schriftfamilie, Größe, Form und das Schriftgewicht bestimmen. Das folgende Beispiel stellt die Schriftdefinition mittels der <tag>-Elemente vor:

**Ausgewählten Tags eine Schrift zuordnen**

~~~~ {.xml}
...
<tags>
  <tag name="h1" font="sans-serif 18 bold" />
  <tag name="h2" font="sans-serif 16 bold" />
  <tag name="h3" font="sans-serif 14 bold" />
</tags>
...
~~~~

Details zum Beispiel
--------------------

1.  Mit dem Attribut `name` wählen Sie das Element aus, das formatiert werden soll.
2.  Mit dem Attribut `font` bestimmen Sie den Schriftschnitt für das Element. Das font-Attribut erwartet die drei Angaben für Schriftfamilie, Schriftgröße in Punkt und Schriftgewicht bzw. Schriftform.

[Kategorie:PDF-Template schreiben](export_de/Kategorie:PDF-Template_schreiben )