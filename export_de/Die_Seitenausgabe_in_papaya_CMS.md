---
title: Die Seitenausgabe in papaya CMS
permalink: /Die_Seitenausgabe_in_papaya_CMS/
---

Die Daten für die Seitenausgabe werden in papaya CMS durch Module aus der Datenbank ausgelesen und als XML ausgegeben. Ein Seitenmodul fungiert in diesem Fall als Schnittstelle für die Ausgabe. Die folgende Illustration stellt diesen sehr vereinfachten Vorgang vor:

<<<<<<< HEAD
[miniatur|zentriert|1000px|Ein papaya-Modul gibt XML aus](/images/File:php-modul-zu-xml-ausgabe.png )
=======
![File:Php-modul-zu-xml-ausgabe.png](images/Php-modul-zu-xml-ausgabe.png)
>>>>>>> a2efb5b3261d70ebc0ed214a6131387e209c4f80

In [:Kategorie:Wie sieht es unter der Haube aus?](/:Kategorie:Wie_sieht_es_unter_der_Haube_aus? ) wird schrittweise und in einer weitaus höheren Detailstufe erklärt, wie eine Seite in papaya CMS aufgerufen und ausgegeben wird. Zunächst genügt es zu wissen, dass das Script vornehmlich Daten aus der Datenbank ausliest, verarbeitet und als XML ausgibt. Das folgende Listing stellt wiederum die auf das Wesentliche gekürzte XML-Ausgabe eines Seitenmoduls vor:

**Auszug aus der XML-Ausgabe eines Seitenmoduls**

~~~~ {.xml}
<page>
  <content>
    <topic>
      <title>Meine schönsten Bilder</title>
      <text>Hier können Sie eine Auswahl meiner schönsten Bilder bewundern.</text>
      <image>
        <img src="galerie.media.0123456789abcdef0123456789abcdef.jpg"
             alt="Foto einer Galerie"/>
      </image>
    </topic>
  </content>
</page>
~~~~

XML-Ausgabe mit Ausgabefilter formatieren
-----------------------------------------

Die XML-Seitenausgabe ist nicht konzipiert worden, um sie direkt den Besuchern einer Website zu präsentieren. Es gibt Ausgabeformen, die dafür weitaus besser geeignet sind. Sie müssen das XML also in ein geeignetes Ausgabeformat umformen (transformieren). Meistens ist das eine Umformung nach (X)HTML, XML für einen RSS-Feed oder eine PDF-Ausgabe. Eine ausführliche Dokumentation zum Erstellen von Templates und Themes für die Ausgabe von Webseiten, RSS-Feeds und PDFs finden Sie im Handbuch „papaya CMS 5: Templates und Themes erstellen“.

Die Technik der Wahl ist XSLT . Sie ermöglicht es, XML-Dokumente in andere Textformate umzuwandeln. Diese Aufgabe führt der XSLT-Prozessor durch. Damit der XSLT-Prozessor weiß, welche Vorlage (Template) er für eine bestimmte XML-Ausgabe verwenden soll, werden in papaya CMS Module (siehe Abbildung "Ein papaya-Modul gibt XML aus" in [Die Seitenausgabe in papaya CMS](/Die_Seitenausgabe_in_papaya_CMS ) ) mit XSLT-Templates verknüpft. Diese Verknüpfung wird in papaya CMS in Form von Ansichten hergestellt.

Ansichten sind Darstellungsinstanzen von Modulen. Sie verknüpfen das Modul selbst mit einem Ausgabemodus, beispielsweise HTML , RSS oder PDF . Ein Ausgabemodus entspricht dabei einem Ausgabefilter , der für ein bestimmtes Ausgabeformat wie HTML konfiguriert ist. Für den verknüpften Ausgabemodus müssen Sie anschließend eine Templatedatei auswählen, die kompatibel mit der XML-Ausgabe des Moduls ist. Der ganze Zusammenhang wird in der folgenden Grafik deutlich:

![File:Ansichten.png](images/Ansichten.png)

Im obigen Beispiel in Abbildung "Zusammenhang zwischen Ansichten, Ausgabemodi und Templates" in [Die Seitenausgabe in papaya CMS](/Die_Seitenausgabe_in_papaya_CMS ) ist die Ansicht „Standardseite“ mit den Ausgabemodi *html* und *rss* verknüpft. Für den Ausgabemodus *html* ist die Templatedatei `page_general.xsl`, für den Ausgabemodus *rss* die Datei `page_rss.xsl` ausgewählt.

Ein Modul kann beliebig viele benannte Darstellungsinstanzen in Form von Ansichten haben. Das bedeutet, dass Sie beliebig viele Ansichten mit dem selben Modul erzeugen können. Dies bietet sich vor allem dann an, wenn Sie die Ausgabe eines Moduls beispielsweise für Ihre Startseite ein wenig anders gestalten möchten. So ist im obigen Beispiel in Abbildung "Zusammenhang zwischen Ansichten, Ausgabemodi und Templates" in [Die Seitenausgabe in papaya CMS](/Die_Seitenausgabe_in_papaya_CMS ) eine zweite Ansicht des Moduls „Topic with image“ mit dem Namen „Spezial“ angelegt, die ebenso wie die Ansicht „Standardausgabe“ mit dem Ausgabemodus *html* verknüpft ist. In diesem Fall jedoch wurde das Template `page_special.xsl` ausgewählt.

Um eine Ansicht einzusetzen, verknüpfen Sie diese mit einer Seite. Anschließend konfigurieren Sie die Seite. Das Seitenmodul stellt dazu eine entsprechende Schnittstelle zur Verfügung, über die Sie je nach Modul auch Inhalte eingeben können. Anschließend können Sie sich die Seite in der Seitenvorschau anschauen und die Seite schließlich veröffentlichen. Bei der Seitenausgabe werden die verknüpften Ausgabefilter und Templates anhand des zugewiesenen Modus ausgewählt. Wird beispielsweise die Seite als `index.7.html` aufgerufen, erfolgt die Ausgabe der Seite in folgenden Schritten:

1.  Anhand der in der URL enthaltenen Seiten-ID (in diesem Fall die 7) wird die Seite ermittelt. papaya CMS erkennt, dass die Seite mit der Ansicht „Standardseite“ verknüpft ist.
2.  Anhand der Endung *html* wird der angeforderte Ausgabemodus der Seite ermittelt. In der aktuellen Ansicht ist für den Modus *html* die Templatedatei `page_general.xsl` verknüpft.
3.  Die XML-Ausgabe des Moduls wird erzeugt und an den Ausgabefilter übergeben.
4.  Der Ausgabefilter transformiert das XML anhand der Umwandlungsregeln in der Templatedatei `page_general.xsl`. Das Ergebnis der Umwandlung wird als HTML-Dokument ausgegeben.

Wird stattdessen `index.7.rss` aufgerufen, kommt `page_rss.xsl` zum Einsatz. In diesem Modus wird einfach ein RSS-Feed ausgegeben.

Die Ausgabemodi (pdf, html, rss, print und weitere) müssen zuvor erstellt werden. Näheres dazu finden Sie im Handbuch „papaya CMS 5: Handbuch für Administratoren“, Kapitel 18.

HTML-Ausgabe mit CSS-Definitionen und Schmuckgrafiken formatieren
-----------------------------------------------------------------

Nun ist schlichtes HTML nicht sonderlich ansprechend. Da Sie sicherlich darauf bedacht sind, Inhalt und Darstellung voneinander zu trennen, und es guter moderner Entwicklungsstil ist, werden CSS-Definitionen nicht direkt in die HTML-Ausgabe geschrieben, sondern CSS-Dateien (Stylesheets) verwendet. Im Zusammenhang mit papaya CMS wird eine Sammlung von CSS-Dateien und Layout-Bildern, die einer Webseite Stil geben, meistens als Theme bezeichnet. Im Unterschied zu diesen Theme-Bildern, die der Dekoration dienen, werden Inhaltsbilder vom System verwaltet, näheres dazu finden Sie im Handbuch „papaya CMS 5: Handbuch für Administratoren“, Kapitel 5.

![File:Html-css-ausgabe.png](images/Html-css-ausgabe.png)

Sie haben gesehen: Seitenmodule von papaya CMS geben XML aus. Ein Seitenmodul ist zudem über eine Ansicht mit einem Ausgabefilter und einem XSLT-Template verknüpft. Der Ausgabefilter wandelt die XML-Ausgabe des Moduls in ein Zielformat wie HTML um und nutzt dabei die Transformationsregeln aus dem XSLT-Template. Eine Ansicht kann dabei beliebig viele Zielformate über Ausgabemodi zugewiesen bekommen.

[Kategorie:Wie funktioniert eigentlich papaya CMS?](Kategorie:Wie_funktioniert_eigentlich_papaya_CMS? )
