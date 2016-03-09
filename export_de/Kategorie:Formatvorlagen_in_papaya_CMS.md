---
title: export_de/Kategorie.md:Formatvorlagen in papaya CMS
permalink: export_de/Kategorie.md:Formatvorlagen_in_papaya_CMS/
---

papaya CMS verfügt über ein sehr flexibles Vorlagensystem, dass durch die Trennung von drei zentralen Ebenen gekennzeichnet ist:

1.  Trennung der Vorlagenlogik von der Programmlogik
2.  Trennung der Vorlagenlogik von den Layoutdefinitionen
3.  Trennung der Inhalte von Vorlagenlogik und Layoutdefinition

Trennung der Vorlagenlogik von der Programmlogik
------------------------------------------------

Unter Programmlogik versteht man die Art und Weise, wie ein Programm funktioniert und auf bestimmte Ereignisse oder Zustände von Variablen reagiert. Ein Programm steuert die Ein- und Ausgabe sowie die Manipulation von Daten. Die Vorlagenlogik ist in den XSLT-Templates enthalten. Sie steuert die Art und Weise, mit der die XML-Ausgabe der Module in das Zieldokument transformiert werden kann. Diese Logik ist dabei strikt von der Programmlogik getrennt.

Die Inhalte in papaya CMS werden in einem medienneutralen Format gespeichert. Sie liegen entweder als ASCII oder als XML-Dokument vor und werden in diesem Format auch in der Datenbank vorgehalten. Die Seiten- und Boxmodule liefern diese Inhalte ebenso medienneutral als XML-Dokument an den Ausgabefilter aus, sodass Sie sich als Programmierer der Templates darum kümmern müssen, wie das entsprechende Zielformat auszusehen hat.

Trennung der Vorlagenlogik von den Layoutdefinitionen
-----------------------------------------------------

Bei den Webseiten-Vorlagen gibt es eine Trennung zwischen der Vorlagenlogik und der Layoutdefinition. Die Vorlagenlogik ist in den XSLT-Templates enthalten, die Layoutdefinition im Theme bestehend aus CSS und Layoutgrafiken. In einer ähnlichen Weise finden Sie diese Trennung auch bei der PDF-Vorlage. In diesem Fall enthält die PDF-Musterdatei Layoutgrafiken, das Layout jedoch definieren Sie in der Vorlagenlogik.

Wenn Sie das XSLT-Template für die Webseitenausgabe schreiben, müssen Sie sich nicht über die Positionierung von Layoutgrafiken oder CSS-Definitionen kümmern. Das XSLT-Template soll lediglich dafür sorgen, dass das von den Modulen gelieferte XML als HTML-Baum ausgegeben wird. Die HTML-Ausgabe enthält dabei eine Verknüpfung mit den CSS-Dateien. Sowohl die Einbindung der Layoutgrafiken als auch die Definitionen für Farben und Schriftformatierung sind dabei in der jeweiligen CSS-Datei enthalten.

Auf diesem Wege wird nicht nur die Webseitenvorlage von der Programmlogik getrennt, sondern auch die Vorlagenlogik im XSLT-Stylesheet von den Layoutdefinitionen in der CSS-Datei.

Trennung der Inhalte von Vorlagenlogik und Layoutdefinition
-----------------------------------------------------------

Inhalte werden medienneutral vorgehalten und im ASCII- oder im XML-Format in der Datenbank gespeichert. Multimediadateien wie Flash, MPEG-Filme, MP3-Dateien oder einfache Bilder liegen zwar im Verzeichnissystem, werden jedoch in der Mediendatenbank registriert und durch Metadaten ergänzt. Wenn Sie die Inhalte in papaya CMS eingeben, müssen Sie jedoch keine layoutspezifischen Informationen speichern. Es sind also keine HTML-Kenntnisse erforderlich.

[export_de/Kategorie.md:Einleitung](export_de/Kategorie.md:Einleitung )