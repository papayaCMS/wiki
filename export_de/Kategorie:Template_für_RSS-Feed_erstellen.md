---
title: Kategorie:Template für RSS-Feed erstellen
permalink: Kategorie:Template_für_RSS-Feed_erstellen/
---

Templates für RSS-Feeds schreiben Sie ebenso wie die Templates der Webseitenvorlage in XSLT. Ein wesentlicher Unterschied zur Webseitenvorlage ist, dass Sie in der `output` -Anweisung die Ausgabe auf XML umstellen müssen. Wenn Sie einen Ausgabefilter für RSS anlegen, müssen Sie daran denken, den Content-Type auf „text/xml“ zu setzen.

XSLT-Templates für RSS sind in der Regel sehr einfach aufgebaut. Das minimale RSS-Template, das in den folgenden Abschnitten als Beispiel vorgestellt wird, besteht lediglich aus drei `template` -Regeln:

1.  Eine `template` -Regel mit dem `match` auf das Wurzelelement „/“, siehe [Starttemplate erzeugen](/Starttemplate_erzeugen ).
2.  Eine `template` -Regel, in der die Basisstruktur mit dem Wurzelelement des RSS-Feeds aufgebaut wird, siehe [XSLT-Template für die Basisstruktur](/XSLT-Template_für_die_Basisstruktur ).
3.  Eine `template` -Regel, die alle `<item>` -Elemente inklusive Titel, Teaser und URL erzeugt, siehe [XSLT-Template für die item-Elemente](/XSLT-Template_für_die_item-Elemente ).

[Kategorie:RSS-Feeds erstellen](Kategorie:RSS-Feeds_erstellen )