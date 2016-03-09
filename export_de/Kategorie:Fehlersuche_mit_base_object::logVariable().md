---
title: Kategorie:Fehlersuche mit base object::logVariable()
permalink: Kategorie:Fehlersuche_mit_base_object::logVariable()/
---

Dieser Abschnitt erklärt, wie Sie mit `base_object::logVariable()` die Inhalte einer Variablen in das Systemprotokoll schreiben können. In bestimmten Fällen lassen sich dadurch wertvolle Debug-Informationen auf einer live-Webseite sammeln. Die Funktion `base_object::logVariable()` sollte aus folgenden Gründen benutzt werden:

1.  Wenn Sie sporadisch auftretende Fehler suchen und die Inhalte einer Variable während des laufenden Betriebs überwachen wollen.
2.  Wenn Sie auf dem System die Debugausgabe nicht benutzen wollen und deshalb nicht die Methode `$this->debug()` verwenden können.
3.  Wenn Sie den Nutzern die Debugausgabe nicht zeigen wollen.

Da `base_object::logVariable()` in `base_object` implementiert worden ist, steht sie in allen Klassen im papaya-Basissystem zur Verfügung.

[Kategorie:Fehlersuche in papaya CMS](export_de/Kategorie:Fehlersuche_in_papaya_CMS )