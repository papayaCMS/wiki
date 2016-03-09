---
title: Druckfreundliches CSS schreiben
permalink: /Druckfreundliches_CSS_schreiben/
---

Für die Print-Templates müssen Sie ein anderes CSS einbinden, das die Inhalte druckfreundlich formatiert. Für druckfreundliche CSS-Formatangaben sollten Sie folgende Punkte beachten:

1.  Schwarze Schrift auf weißem Hintergrund ergibt ein optimales Kontrastverhältnis. Monochrome Laserdrucker rastern zudem farbige Schrift, was besonders bei niedriger Auflösung zu einem schlechteren Schriftbild führt.
2.  Die Schriftgröße wird in der Einheit *em* oder auch in *pt* angegeben, keinesfalls jedoch in *px*. Pixelangaben können je nach Plattform und Konfiguration anders berechnet werden, sodass Sie nie genau wissen, in welcher Größe die Schrift dargestellt wird.
3.  Wichtige Illustrationen sollten als Graustufenbild eingefügt werden. Bei farbigen Illustrationen sollten Sie sicherstellen, dass die Kontraste zwischen den Farben groß genug sind und bei einem Schwarz-Weiß-Druck nicht verloren gehen.
4.  Layoutgrafiken ausblenden. Sowohl Layoutgrafiken als auch Hintergrundbilder sollten Sie ausblenden.

In den Demotemplates von papaya CMS werden Layoutgrafiken durch das CSS eingebunden. Sie sollten in ähnlichen Fällen auf solche Layoutgrafiken verzichten und als einziges graphisches Element höchstens das Logo Ihrer Website zulassen.

Im folgenden Beispiel wird eine druckfreundliche Version des Demo-Themes dargestellt:

**Druckfreundliche CSS-Formatangaben**

~~~~ {.css}
body{
  font: 10pt/1.5 Verdana, Arial, Helvetica, sans-serif;
  background: #fff;
  margin: 0 5%;
  color:#000;
}

@media print {
  * {
  background-color: white;
  background-image: none;
  }
}
...
~~~~

Links sollten zudem auch in Schwarz formatiert werden. Damit sie aber noch als Links erkennbar sind, sollte man sie unterstreichen:

**Linkformate definieren**

~~~~ {.css}
...
a {
  text-decoration: underline;
}

a:link, a:visited, a:hover, a:active {
  color: #000;
}
...
~~~~

Bestimmte Elemente der Seite wie die LInks für die Umschaltung der Content-Sprache, die Brotkrümelnavigation oder die Links zum Umschalten der Ausgabeformate können Sie zudem einfach über CSS ausblenden:

**Nicht benötigte LInks ausblenden**

~~~~ {.css}
...
#footerNavigation, .pageTranslations, .accessibilityElement,
#pageNavigation, #pageAdditional, #ariadne {
  display: none;
}
...
~~~~

Ferner können Sie für die zentralen Elemente der Seitenstruktur Hintergrundbilder sowie Hintergrundfarben mit `background:
      none;` deaktivieren. Die Elemente sollen dadurch die Hintergrundeigenschaften des `<body>` -Elements erben. die Breite wird im zentralen Content-Element zudem auf 100 % gesetzt, um die volle Breite des Bildschirmes zu nutzen:

**Seitenstruktur formatieren**

~~~~ {.css}
...
/* =Seitenstruktur
----------------------------------------------- */
#container, #page, #wrapper, #main-boxes .box, #main {
  background: none;
  border: none;
  float: none;
  width: 100%;
}

#subnav, #nav, #left {display:none;}
#header {
  background: none;
  border: none;
  height: auto;
}
...
~~~~

Block-Level-Elemente im Content-Bereich erhalten entsprechende Formatierungen für Schriftgröße, Abstände und Rahmen:

**Formatangaben für Elemente im Content-Bereich**

~~~~ {.css}
...
/* =Elements
----------------------------------------------- */
h1,h2,h3,h4,h5,h6, #logo h2, #logo h4{
  color:#000;
  margin:0;
  padding:0;
  background-color:inherit;
}
h2 {
  font-size:1.4em;
  font-weight:normal;
  line-height:1.4em;
}
h3,h4,h5{
  font-size:1.2em;
  line-height:1.2em;
}
p {
  margin:0;
  padding: 0 0 1em 0;
}

h1 .subTitle {
  font-size: 80%;
}

img {
  border:none;
}
...
~~~~

Zuletzt erhält noch der Footer eine entsprechende Formatierung:

**Formatangaben für den Footer**

~~~~ {.css}
...
/* =Footer
----------------------------------------------- */
#footer{
  color: #999;
  font-size: 95%;
  background: none;
}
#footer p{
  padding: 60px 15px 30px;
}
~~~~

Im folgenden Abschnitt können Sie erfahren, wie Sie die CSS-Datei in die druckfreundliche Seitenausgabe einbinden können.

[Kategorie:Print-Templates schreiben](export_de/Kategorie:Print-Templates_schreiben )