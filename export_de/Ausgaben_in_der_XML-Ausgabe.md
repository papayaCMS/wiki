---
title: Ausgaben in der XML-Ausgabe
permalink: /Ausgaben_in_der_XML-Ausgabe/
---

Wenn Sie für Ihre Seiten Templates für verschiedene Ausgabeformate erstellt haben, können Nutzer diese Ausgaben über eine entsprechende Dateiendung abrufen. Anstelle einer URL wie bspw. <http://www.domain.tld/index.11.de.html> können Sie <http://www.domain.tld/index.11.de.pdf> in die Adresszeile Ihres Browsers eingeben. Auf diese Weise rufen Sie die PDF-Ausgabe der Seite auf.

Eine solche Vorgehensweise ist jedoch nicht besonders benutzerfreundlich. Nutzer mit geringeren technischen Kenntnissen können überfordert sein. Zudem kann der Nutzer nicht wissen, welche Ausgabeformate zur Verfügung stehen. Da Sie die Dateiendungen im Ausgabefilter frei konfigurieren können, sind die entsprechenden Dateiendungen auch nicht immer vorhersagbar.

Sie können dieses Problem jedoch auf eine sehr einfache Weise lösen. Jede Seite speichert die vorhandenen Ansichten als Eigenschaft ab. Das von den Seitenmodulen gelieferte XML enthält dabei für alle verfügbaren Ansichten die passenden Links im Element `<views>`:

**Ansichten in der XML-Ausgabe**

~~~~ {.xml}
<page>
  <content>
    ...
  </content>
  <meta>
    ...
  </meta>
  <views>
    <viewmode ext="html" href="startseite.2.de.html.preview"/>
    <viewmode ext="pdf" href="startseite.2.de.pdf.preview"/>
  </views>
  <translations>
    ...
  </translations>
  <boxes>
    ...
  </boxes>
</page>
~~~~

Sie müssen die Links der `<viewmode>` -Elemente also lediglich in die Webseitenausgabe durchschleifen, damit sie dem Nutzer zur Verfügung stehen. Auf diese Weise werden auch nur diejenigen Links angezeigt, die für die aktuelle Seite zur Verfügung stehen. Links, die ins Leere führen, können auf diese Weise nicht entstehen.

[export_de/Kategorie:Ausgaben verlinken](export_de/Kategorie:Ausgaben_verlinken )