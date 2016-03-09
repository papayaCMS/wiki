---
title: Ausgabe von Webseiten
permalink: /Ausgabe_von_Webseiten/
---

Der grundliegende Aufbau der Webseite wird im Demotemplate von einem zentralen XSLT-Dokument bestimmt. Dieses Dokument besteht aus dem XSLT-Template für das HTML-Grundgerüst. Dieses Haupttemplate bestimmt also den grundlegenden Aufbau der Seite und enthält entsprechend Bereiche, in die Boxen nach bestimmten Boxgruppen geordnet in das HTML-Grundgerüst eingefügt werden. Da die Boxen bereits als generiertes HTML in CDATA-Abschnitte der XML-Seitenausgabe eingefügt werden, können Sie einfach durch das Webseitentemplate in die Seitenausgabe eingefügt werden, ohne dass explizit ein spezielles Template aufgerufen werden müsste.

Im zentralen Content-Bereich der Seite wird der eigentliche Seiteninhalt ausgegeben. Da sich jedoch die XML-Ausgabe der Seiten je nach Seitenmodul unterscheidet, wird im Haupttemplate ein entsprechendes Template für das Seitenmodul aufgerufen. Dieses Template wandelt das XML für den Content-Bereich der Seite in das Zielformat um.

Für jedes Seitenmodul kann also ein eigenes XSLT-Template in einer separaten XSLT-Datei erstellt werden. Diese XSLT-Datei importiert das zentrale XSLT-Dokument mit dem Template für das HTML-Grundgerüst und überlagert das XSLT-Template für den Content-Bereich der Seite ( `content_area` ). Auf diese Weise muss man nicht für jedes Seitenmodul das komplette HTML-Grundgerüst nachbauen, da man sich lediglich auf den Content-Bereich konzentrieren kann.

<<<<<<< HEAD
Die folgende Illustration stellt den Ausgabeprozess von Seiten in papaya CMS dar:

[miniatur|zentriert|1000px|Seitenausgabe in papaya CMS](/images/File:Ausgabekonzept.png )

=======
>>>>>>> a2efb5b3261d70ebc0ed214a6131387e209c4f80
Eine Seite kann in der Regel mit beliebig vielen Boxen verknüpft sein. Bei Seiten und Boxen handelt es sich um Module, die Ihre Inhalte als XML ausgeben. Wenn eine HTML-Ausgabe der Seite erzeugt werden soll, muss dieses XML in das Zielformat HTML umgewandelt werden. Dazu ruft die Template-Engine von papaya CMS für jedes Modul das passende XSLT-Stylesheet mit den Transformationsregeln auf.

Das XML der Boxen wird durch den Ausgabefilter stets separat in das Zielformat HTML transformiert. Da die HTML-Ausgabe der Boxen stets in CDATA-Abschnitte des Seiten-XML eingebettet wird, müssen die verlinkten Boxen also vorher durch den Ausgabefilter gehen. Wenn das Seiten-XML ausgegeben wird, stößt dies automatisch die Umwandlung des Boxen-XMLs in das Zielformat an. Anschließend kann das XSLT-Template für die Seite durch den Ausgabefilter aufgerufen werden. Dabei werden die Boxen direkt in den HTML-Seitenbaum eingefügt, während das XML des Seitenmoduls in das HTML-Format umgewandelt wird.

Das Template, das das HTML-Grundgerüst erstellt, bindet auch die CSS-Ressourcen aus dem Theme ein. Durch die Verknüpfung mit dem Theme wird das HTML formatiert, farblich gestaltet und gegebenenfalls mit Layoutgrafiken ausgestattet, die per CSS eingebunden werden.

Ansichten verbinden Module mit Ausgabefiltern und XSLT-Templates
----------------------------------------------------------------

Damit die Template-Engine von papaya CMS für jedes Modul sowohl das richtige XSLT-Template als auch den passenden Ausgabefilter benutzen kann, müssen alle Module mit Ansichten verknüpft werden. Ansichten sind abstrakte Objekte, die jedes Modul mit einem Ausgabefilter und einem XSLT-Template verknüpfen. Das Besondere an dieser Verknüpfung ist, dass eine Ansicht mit beliebig vielen Ausgabefiltern verknüpft werden kann.

Die Auswahl des Ausgabefilters erfolgt dabei nach der Dateiendung, mit der die Seite im Webbrowser angefordert wird. Wenn Sie beispielsweise einen Ausgabefilter für die Endung html definiert haben, können Sie diesen Ausgabefilter mit einer Ansicht verknüpfen. Anschließend wählen Sie für den verknüpften Ausgabefilter ein XSLT-Template aus, das mit der Ansicht verknüpft werden soll. Wenn Sie also eine Seite mit dieser Ansicht anlegen, wird bei der Ausgabe der Seite im HTML-Format der verknüpfte Ausgabefilter mit der ausgewählten XSLT-Datei aufgerufen. Der Ausgabefilter wandelt dabei das Seiten-XML in das Zielformat HTML um.

Die folgende Illustration stellt Ihnen die Frontend- und Backendschnittstelle für Module in papaya CMS vor:

<<<<<<< HEAD
[miniatur|zentriert|1000px|Frontend- und Backend-Schnittstelle in papaya CMS](/images/File:papayaSystem.png )

=======
>>>>>>> a2efb5b3261d70ebc0ed214a6131387e209c4f80
Wichtig ist das Teilsystem, das als Frontend-Schnittstelle bezeichnet ist. In diesem Bereich wird die Ausgabe der Module – egal ob es sich um Boxen oder Seiten handelt – an den XSLT-Ausgabefilter übergeben und umgewandelt. Dabei kann sowohl die XML-Ausgabe der Module als auch die Ausgabe des Ausgabefilters in einem Cache-Speicher vorgehalten werden, um die Performanz bei der Seitenauslieferung zu erhöhen.

Beim Aufruf des Ausgabefilters werden im XSLT-Prozessor zudem alle notwendigen Parameter mit Angaben für das Theme-Verzeichnis und für weitere Ressourcen gesetzt. Wenn die Seite mit Boxen verknüpft ist, wird vorher das XML von allen verknüpften Boxen durch den Ausgabefilter in das Zielformat umgewandelt und die HTML-Ausgabe der Boxen in CDATA-Abschnitte des Seiten-XML eingebunden. Erst dann kann das Template der Seite aufgerufen werden.

[Kategorie:Das Vorlagenkonzept in papaya CMS](export_de/Kategorie:Das_Vorlagenkonzept_in_papaya_CMS )
