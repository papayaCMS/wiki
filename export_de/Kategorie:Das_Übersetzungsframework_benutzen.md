---
title: Kategorie:Das Übersetzungsframework benutzen
permalink: Kategorie:Das_Übersetzungsframework_benutzen/
---

In diesem Kapitel erfahren Sie, wie Sie das Übersetzungsframework aus dem Default-Template von papaya CMS nutzen können, um Phrasen zu übersetzen. Das Übersetzungsframework macht es einfach, für die Web- oder PDF-Ausgabe Formatvorlagen zu erstellen, die mehrere Content-Sprachen unterstützen.

Während der Content von Seiten und Boxen aus der Datenbank kommt und mehrsprachige Inhalte im Backend von papaya CMS gepflegt werden, sind bestimmte Standardinhalte fest in der Webseitenvorlage verdrahtet. Die Beschriftungen von Absendebuttons oder Links stehen in der Regel im Template. Bei mehrsprachigen Webprojekten müssen die Phrasen entsprechend übersetzt werden. Das Demo-Template von papaya CMS enthält zu diesem Zweck ein Framework, mit dem die Phrasen und ihre Übersetzungen sehr einfach und flexibel verwaltet werden können.

Die Idee ist, dass die Phrasen in externe XML-Dateien ausgelagert werden. Diese Dateien werden durch die XSLT-Templates eingelesen. Dadurch kann man die Vorlagenlogik in den Templates von der Datenhaltung in Form der Übersetzungsdateien trennen.

Das Übersetzungsframework ist darüber hinaus auf eine Modularisierung der Übersetzungsdateien ausgelegt. Die Übersetzungen stehen nicht alle in einer einzigen Datei, sondern sind auf verschiedene Dateien verteilt. Das Basissystem selbst bringt einige Dateien mit Standardphrasen in den unterstützten Basissprachen Deutsch und Englisch mit. Zudem gibt es für jedes Modul separate Übersetzungsdateien, die dazugeladen werden.

Aufbau des Phrasen-Systems

Das Übersetzungsframework besteht aus drei Komponenten:

1.  XML-Dateien mit den Übersetzungen, siehe [:Kategorie:Mit Übersetzungsdateien arbeiten](/:Kategorie:Mit_Übersetzungsdateien_arbeiten ).
2.  Parameter, über die abhängig vom jeweiligen Kontext die XML-Dateien eingebunden werden, siehe [Übersetzungsdateien in XSLT-Parameter laden](/Übersetzungsdateien_in_XSLT-Parameter_laden ).
3.  Basisstylesheet mit dem Übersetzungstemplate, siehe [:Kategorie:Übersetzungstemplate benutzen](/:Kategorie:Übersetzungstemplate_benutzen ).

[Kategorie:Templates und Themes entwickeln](Kategorie:Templates_und_Themes_entwickeln )