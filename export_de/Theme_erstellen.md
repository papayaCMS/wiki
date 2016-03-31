
Die in papaya CMS enthaltene Webseitenvorlage kann sehr weitgehend über die CSS-Angaben angepasst werden. Sowohl die Position der einzelnen div-Elemente als auch die Größe wird durch die entsprechenden CSS-Formate bestimmt.

Bei einer Standardinstallation mit dem Demoprojekt finden Sie das Standard-Theme im Ordner `/papaya-themes/papaya-demo/`. Um dieses Themepaket als Ausgangsbasis für Ihre Anpassungen zu nehmen, können Sie es kopieren und in einem weiteren Ordner in `/papaya-themes/` anlegen. Sie können nun mit der Kopie arbeiten und diese verändern.

Um ein eigenes Theme zu erstellen, gehen Sie wie folgt vor:

1.  Erstellen Sie einen neuen Ordner im Verzeichnis `/papaya-themes/`. Wenn Sie das Demo-Theme als Ausgangspunkt für Ihr Projekt nehmen möchten, können Sie auch einfach eine Kopie des Demo-Theme-Verzeichnisses `/papaya-demo` erzeugen.
2.  Legen Sie einen Unterordner für Layout-Grafiken an.
3.  Erzeugen Sie eine CSS-Datei, in der Sie zentrale Formate der Seite bestimmen und Hintergrundgrafiken eingebunden werden.
4.  Erzeugen Sie eine CSS-Datei, in der Sie die zentralen Formate von Boxen bestimmen.
5.  Erzeugen Sie für jedes Seitenmodul eine separate CSS-Datei, die durch das passende XSLT-Seitentemplate eingebunden wird. Näheres zum Einbinden der CSS-Dateien erfahren Sie in [XSLT-Haupttemplate erstellen](XSLT-Haupttemplate_erstellen.md).

Organisation des Demo-Themes

Das Temo-Theme besteht aus mehreren CSS-Dateien und Layoutgrafiken, die im Template referenziert werden. Die wichtigsten CSS-Dateien sind in der folgenden Auflistung beschrieben:

basic.css
Enthält CSS-Formatangaben für Inline-Elemente.

colors.css
Enthält die Farbangaben für die zentralen HTML-Elemente.

favicon.ico
Die Icon-Datei für die Favoriten.

ie6win.css
Diese CSS-Datei enthält CSS-Formatangaben, die an den Internet Explorer 6 unter Windows angepasst worden sind. Das Stylesheet wird über eine entsprechende Browserweiche eingebunden.

ie7.css
Diese CSS-Datei enthält CSS-Formatangaben, die an den Internet Explorer 7 angepasst worden sind. Das Stylesheet wird über eine entsprechende Browserweiche eingebunden.

main.css
Enthält CSS-Formatangaben für alle zentralen Block-Level-Elemente, mit denen das Seitenlayout definiert wird.

print.css
Enthält spezielle CSS-Formatangaben, mit denen die Seite für den Ausdruck optimiert wird. Zur Druckoptimierung gehört, dass Hintergrundgrafiken weggelassen werden und Hintergrundfarben auf weiß gesetzt werden. Als Textfarbe wird generell schwarz eingestellt.

Darüber hinaus enthält das Theme noch viele CSS-Dateien, die das Namenspräfix `box_` oder `page_` enthalten. Diese CSS-Dateien enthalten Formatangaben für modulspezifische HTML-Elemente.

Layoutgrafiken, die durch die CSS-Dateien eingebunden werden, sind im Unterverzeichnis `pics/` enthalten.

Zentrale Formate

Anhand der zentralen CSS-Datei aus dem Demo-Theme, `main.css`, soll aufgezeigt werden, wie die einzelnen `<div>` -Elemente des HTML-Baumes im Demotemplate formatiert werden. Es werden nur die Formatangaben für die Elemente dargestellt, welche die zentralen Bereiche wie Titelleiste, Navigationsleiste, Content-Bereich, rechte Spalte und Fußleiste formatieren:

**Zentrale CSS-Datei main.css**

~~~~ {.xml}
/* Page basics
html {
  text-align: center;
  font: .625em/1.5 Verdana, Arial, Helvetica, sans-serif;
}
body {
  background: url(pics/hg.png) repeat top left;
  margin: 17px auto;
  max-width: 99%;
  min-width: 50em;
  padding: 0;
  text-align: left;
  width: 79.3em;
}
...
~~~~

Grundlegende Eigenschaften wie die Standard-Absatzausrichtung sowie die Schriftfamilie und -größe werden dem Element `<html>` über den gleichnamigen Selektor `html` zugewiesen. Damit erben alle in `<html>` eingebetteten Elemente diese Eigenschaften.

Über den Selektor `body` werden dem `<body>` -Element sowohl Formatangaben als auch eine Hintergrundgrafik zugewiesen. Formate wie die Angabe der Hintergrundfarbe werden an alle in `<body>` enthaltenen Elemente vererbt. Über CSS wird diesem Element auch eine Hintergrundgrafik zugewiesen.

Titelleiste (\#header) und Fußleiste (\#footer)

**CSS-Definition der Titelleiste**

~~~~ {.xml}
...
/* Header
#header .headerBackground {
  background: #FFF url(pics/header_bg.gif) repeat-y top right;
  margin: 0 8px;
  padding: 10px 10px 0px 10px;
}
#header .headerContent {
  height: 100px;
}
#header ul.pageTranslations {
  padding: 2px 7px;
  margin: 0;
}
#header ul.pageTranslations li {
  display: inline;
  padding-right: 4px;
}
#header ul.pageTranslations li a {
  font-weight: bold;
  text-decoration: none;
}
...
~~~~

Das

<div>
-Element mit dem ID-Selektor `#header` umschließt die Kopfzeile. Es erhält eine Hintergrundgrafik sowie eine Hintergrundfarbe ( `background`.md) und entsprechende Abstände ( `padding` und `margin`.md). Auch hier wird die Hintergrundgrafik per CSS definiert und nicht in das Seiten-HTML im Template geschrieben. Dies ist im Sinne der Trennung von Form und Inhalt gewünscht. Layout-Grafiken sollen also möglichst im Theme, also per CSS, definiert werden.

Der Content-Bereich der Kopfzeile wird über einen speziellen Klassenselektor `.headerContent` formatiert. Der Bereich erhält jedoch lediglich eine fixe Höhe von 100 Pixeln.

Der Header enthält zudem Links, mit denen die vorhandenen Content-Sprachen umgeschaltet werden können. Diese Elemente werden ebenfalls über separate Klassenselektoren `ul.pageTranslations` formatiert.

**CSS-Definition der Fußzeile**

~~~~ {.xml}
...
/* Footer
#footer {
  clear: both;
  font-size: 90%;
  position: relative;
}
#footer .footerContent {
  margin: -3px 8px;
}
#footer #copyright {
  border: none;
  padding-right: 0;
}
...
~~~~

Auf ähnliche Weise wird das `<div>` -Element formatiert, dass die CSS-Klasse `#footer` enthält. Dieses Element wird relativ positioniert. Zudem wird die Schriftgröße auf 90 % der Grundschrift gesetzt. Über den Selektor `#footer
      .footerContent` erhält der Content-Bereich lediglich eine margin-Definition. Das im Footer enthaltene copyright-Element wird über den ID-Selektor `#footer #copyright` noch gesondert formatiert, indem der Rahmen ( `border`.md) sowie der Innenabstand ( `padding`.md) auf "0" gesetzt werden.

Footer-Navigation für die Umschaltung des Ausgabeformats

Der Footer enhält zudem noch einige Links, über die Nutzer die aktuelle Seite in einem anderen Ausgabeformat wie PDF, RSS oder Print öffnen können. Dabei werden diese Links nur dann ausgegeben, wenn die aktuelle Seite auch tatsächlich über diese alternativen Ausgaben verfügt. Die Formatangaben für diese Links sind ebenfalls in der `main.css` enthalten:

**Formatierung der Footer-Navigation in der main.css**

~~~~ {.css}
...
/* Footer Navigation
#footerNavigation {
  float: right;
}
#footerNavigation ul {
  display: inline;
  list-style-type: none;
  margin: 0;
  padding: 0;
}
#footerNavigation li {
  display: inline;
  margin: 0;
  padding: 0;
}
#footerNavigation a {
  border-right-width: 1px;
  border-right-style: solid;
  padding-right: 4px;
  padding-left: 4px;
  text-decoration: none;
}
#footerNavigation li.last a {
  border-right: none;
}
#footerNavigation li li.last a {
  border-right-width: 1px;
  border-right-style: solid;
}
...
~~~~

Da die Footer-Links im Default-Template als Listenelemente ausgegeben werden, enthalten die CSS-Definitionen entsprechende Formatangaben für `<ul>` - und `<li>` -Elemente. Diese Elemente, die standardmäßig als Blockelemente formatiert werden, erhalten das `display` -Attribut `inline`. Damit werden diese Elemente inzeilig dargestellt.

CSS-Definitionen für den Content-Bereich

Das Standard-Template in papaya CMS kann dynamisch auf ein einspaltiges, zweispaltiges oder dreispaltiges Layout umgeschaltet werden. Die Linke Spalte wird dabei für die Navigation genutzt, die mittlere Spalte für den Seiteninhalt und die rechte Spalte für Kontextinformationen.

Der Inhalt für die linke und die rechte Spalte wird durch Boxen geliefert. Wenn diese Boxen nicht in die Seite integriert werden, schaltet das Templatesystem automatisch auf ein zweispaltiges oder einspaltiges Layout um. Damit die Spalten auch korrekt formatiert werden, enthält die main.css entsprechende zentrale Formatklassen:

**Zentrale Formatklassen für Spalten (main.css)**

~~~~ {.css}
...
/* Page
#page .threeColumnLayout, #page .twoColumnLayoutLeft {
  background: url(pics/bg_content.gif) repeat-y 20.95% 0;
  padding-right: 10px;
}
#page .twoColumnLayoutRight {
  padding-right: 10px;
}
#page .threeColumnLayout .pageBackground, #page .twoColumnLayoutRight .pageBackground {
  background: url(pics/bg_content_gr.gif) repeat-y 78.815% 0;
  display: block; /* be nice to Safari */
  padding-bottom: 2em;
}
#page .contentBackground:after {
  content: ".";
  display: block;
  height: 0;
  clear: both;
  visibility: hidden;
}
#page #pageContent {
  float: left;
  padding-left: 20px;
  margin-left: 1px;
  font-size: 110%;
}
#page .threeColumnLayout #pageContent {
  width: 52.866%;
}
#page .twoColumnLayoutLeft #pageContent {
  width: 74.322%;
}
#page .twoColumnLayoutRight #pageContent {
  width: 74.322%;
}
#page .singleColumnLayout #pageContent {
  width: 95.796%;
}
...
~~~~

Für den Content-Bereich der Seite werden entsprechende Hintergrundbilder eingebunden. Über den ID-Selektor `#page
      #pageContent` werden für den Seiteninhalt entsprechende Abstände definiert. Die entsprechenden Spaltenklassen erhalten bestimmte Breitenangaben, je nachdem, ob es sich um eine einspaltige, zweispaltige oder dreispaltige Klasse handelt. Eine Besonderheit ist dabei, dass es für das zweispaltige Layout zwei Klassen gibt: `.twoColumnLayoutRight` und `.twoColumnLayoutLeft`:

1.  Die erste Klasse `.twoColumnLayoutLeft` wird benutzt, wenn Boxen für die linke Spalte benutzt werden, aber keine Boxen für die rechte Spalte.
2.  Die zweite Klasse `.twoColumnLayoutRight` wird benutzt, wenn Boxen für die rechte Spalte benutzt werden, aber keine Boxen für die linke Spalte.

Die HTML-Elemente innerhalb der einzelnen Spalten werden durch zusätzliche CSS-Definitionen weiter spezifiziert.

CSS-Definitionen für die linke Spalte

Die linke Spalte mit der ID `#pageNavigation` enthält zusätzliche CSS-Definitionen für zentrale `<div>` -Elemente, die die Boxen sowie die Boxentitel umfassen:

**CSS-Formatangaben für die Navigationsspalte (main.css)**

~~~~ {.css}
...
/* Page Navigation Column
#page #pageNavigation {
  float: left;
  width: 21.456%;
}
#page #pageNavigation div.box {
  margin-right: 1px;
}
#page #pageNavigation div.box div.boxTitle {
  padding: 0.2em 0 0.2em 8px;
  font-weight: bold;
  font-size: 1.1em;
}
...
~~~~

CSS-Definitionen für die rechte Spalte

Die rechte Spalte mit der ID `#pageAdditional` enthält ebenso zusätzliche CSS-Definitionen für zentrale `<div>` -Elemente, die die Boxen sowie die jeweiligen Boxentitel umfassen:

**CSS-Formatangaben für die rechte Spalte (main.css)**

~~~~ {.css}
...
/* Page Additional Content Column
#page #pageAdditional {
  float: right;
  width: 21.456%;
}
#pageAdditional .box .boxTitle {
  font-size: 100%;
  padding-left: 10px;
}
#pageAdditional .first .boxTitle {
  background: url(pics/right_corner.gif) no-repeat top right;
}
#pageAdditional .boxData {
  padding: 10px;
}
...
~~~~

CSS-Definitionen für den zentralen Seiteninhalt (mittlere Spalte)

Für die zentrale Spalte im Content-Bereich werden einige wenige CSS-Angaben gemacht:

**CSS-Definition des Content-Bereiches**

~~~~ {.xml}
...
/* Page Content
#content {
  margin-bottom: 1em;
}
#content .subTitle {
  font-size: 0.8em;
  display: block;
}

#content ol, #content ul {
  margin: 1em 0 2em 5px;
}
#content li {
  list-style-type: none;
  background: transparent url(pics/listitem.gif) no-repeat 0 .4em;
  padding: 0 0 3px 8px;
}
...
~~~~

So erhält das äußere `<div>` -Element über den ID-Selektor `#content` noch einen zusätzlichen Außenabstand nach unten um `1 em`. Zudem wird noch die CSS-Klasse `.subTitle` definiert, mit denen Untertitel von Artikel formatiert werden können. Dabei erhält das Element die Darstellung eines Block-Level-Elements ( `display: block;`.md) und eine Schriftgröße von `0.8 em`.

Zudem werden noch zusätzliche Abstände für geordnete und ungeordnete Listen gesetzt. Aufzählungszeichen für Gliederungslisten werden über den Selektor `#content li` deaktiviert, indem der Wert des Attributs `list-style-type` auf `none` gesetzt wird. Über die eingebundene Hintergrundgrafik wird stattdessen eine Grafik als Aufzählungszeichen benutzt, die aus dem Theme kommt.

Die restlichen CSS-Definitionen in der `main.css` dienen dazu, Abstände sowie `float` -Eigenschaften von Elementen innerhalb von `#content` zu bestimmen.

Theme des Demotemplates an eigene Wünsche anpassen

Wenn Sie das Theme an Ihre eigenen Bedürfnisse anpassen möchten, können SIe wie folgt vorgehen:

1.  Kopieren Sie sich das Theme-Verzeichnis und benennen Sie es um.
2.  Wählen Sie das neue Theme-Verzeichnis in der Systemsteuerung aus, siehe [Installation der Webseitenvorlage](Installation_der_Webseitenvorlage.md).
3.  Erstellen Sie ggf. neue Layoutgrafiken und speichern Sie diese in das `pics` -Unterverzeichnis Ihres Themes..
4.  Passen Sie die CSS-Dateien Ihrem Layout an:
    1.  Ändern Sie die Farbangaben in der Datei `color.css`.
    2.  Fügen Sie andere Hintergrundgrafiken, Layoutgrafiken sowie ein anderes Logo ein. Passen Sie dazu die entsprechenden Bildreferenzen in der Datei `main.css` und `basic.css` an.

5.  Optional können Sie noch die zentralen Schriftangaben sowie Größe und Abstände der zentralen Layout-Elemente anpassen: Passen Sie ggf. die Schriftangaben in der Datei `main.css` an. Ändern Sie ggf. die Größe sowie die Abstände von Kopfzeile, Fußzeile, Content-Bereich, Navigationsspalte sowie Kontextspalte. Auch diese Werte können Sie in der `main.css` einstelllen.

Im folgenden Abschnitt können Sie erfahren, wie Sie ein HTML-Template erstellen..

