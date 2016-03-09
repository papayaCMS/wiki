---
title: Struktur der XML-Seitenausgabe
permalink: /Struktur_der_XML-Seitenausgabe/
---

Die folgende Illustration stellt den grundlegenden Aufbau der XML-Seitenstruktur dar. Abgebildet sind die ersten zwei Ebenen des XML-Baumes:

![File:XMLOutputXMLRoot.png](images/XMLOutputXMLRoot.png)

Das Root-Element der Seite ist `<page>`. Dieses Wurzelelement enthält genau fünf unmittelbare Kindelemente:

1.  `<content>`: In diesem Tag ist der Inhalt der Seite enthalten.
2.  `<meta>`: Dieses Tag enthält Metainformationen der Seite. Die Metainformationen werden in Form von Metatags in die HTML-Ausgabe eingebunden.
3.  `<views>`: Dieses Tag enthält eine Liste der Ansichten, die für diese Seite definiert sind. Standardausgabeformat ist HTML, möglich ist zusätzlich PDF.
4.  `<translations>`: Dieses Tag enthält Informationen zu den vorhandenen Übersetzungen dieser Seite.
5.  `<boxes>`: Dieses Tag enthält eine Liste der Boxen, die mit dieser Seite verknüpft sind.

<content>-Tag mit <topic>-Element
---------------------------------

Das `<content>` -Tag enthält genau ein unmittelbares Kindelement: `<topic>`. In diesem Tag sind die seitenmodulspezifischen Inhalte in Form einer XML-Struktur enthalten. Das `<topic>` -Tag besitzt ferner eine Reihe von wichtigen Informationen in Form von Attributen:

|Attribut|Bedeutung|
|--------|---------|
|`no`|Die Seiten-ID|
|`href`|Der Dokumentenname der Seite in der aktuellen Sprachversion|
|`author`|Der Name des Benutzers, der die Seite erstellt hat.|
|`created`|Das Datum der Erstellung der Seite.|
|`published`|Das Veröffentlichungsdatum der Seite.|
|`audited`|Das Datum, an dem die Seite vom Redakteur bestätigt worden ist (nur bei veröffentlichten Seiten).|
|`module`|Der Name des Moduls, das die XML-Ausgabe liefert.|
|`guid`|Der eindeutige Schlüssel, der dieses Modul identifiziert.|

<meta>-Tag mit <metatag>-Elementen
----------------------------------

<meta> enthält einige spezielle <metatag>-Tags, die Metainformationen über die Seite enthalten. Diese Metaangaben sollten bei der HTML-Ausgabe in entsprechende Metatags im `<head>` -Bereich ausgegeben werden.

<views>-Tag mit <viewmode>-Elementen
------------------------------------

Der `<views>` -Tag enthält beliebig viele `<viewmode>` -Tags, die Angaben zu den jeweiligen definierten Ausgabeformaten enthalten. So ist neben dem Standardausgabeformat HTML auch PDF denkbar. Folgende Attribute sind im `<viewmode>` -Tag enthalten:

|Attribut|Bedeutung|
|--------|---------|
|ext|Die Dateiendung, die mit dem Ausgabemodus korrespondiert. Für HTML lautet die Dateiendung standardmäßig „html“.|
|href|Der Dokumentenname mit der Endung, die zum Ausgabemodus passt.|

<translations>-Tag mit <translation>-Elementen
----------------------------------------------

Der `<translations>` -Tag enthält für jede vorhanden Sprachversion der Seite einen `<translation>` -Tag. Dieses Tag enthält folgende Attribute:

|Attribut|Bedeutung|
|--------|---------|
|lng_short|Das Sprachkürzel im Format de_DE oder en_EN.|
|lng_title|Der Name der Sprache in der entsprechenden Landessprache, z.B. „Deutsch“, „English“, „Italiano“, „Français“.|
|href|Der Dateiname des HTML-Dokuments, das den Inhalt in der entsprechenden Übersetzung enthält. Die deutsche Version der Seite mit der ID „2“ hat bspw. folgende Bezeichnung: `index.2.de.html`.|
|selected|Der `<translation>` -Tag mit den Angaben zur aktuell angezeigten Sprachversion der Seite enthält dieses Attribut mit dem gleichnamigen Wert „selected“. Bei nicht angezeigten Sprachen enthält das jeweilige Tag kein `selected` -Attribut.|

<boxes>-Tag mit <box>-Elementen
-------------------------------

Der `<boxes>` -Tag enthält für jede mit der Seite verbundene Box ein entsprechendes `<box>` -Element. Die Position der Box im HTML-Baum der Ausgabe wird dabei durch den Wert im group-Attribut bestimmt. Das Haupttemplate filtert dabei die Boxen mit den passenden `group` -Attribut an den jeweiligen Stellen im Template aus. Falls also im Template ein bestimmter `group` -Wert nicht vorhanden ist, wird die Box mit diesem `group` -Attribut nicht in die HTML-Ausgabe eingebunden.

Das `<box>` -Element enthält folgende Attribute:

|Attribut|Bedeutung|
|--------|---------|
|group|Die Boxgruppe, zu der diese Box gehört. Der Name der Boxgruppe regelt die Position der Box im HTML-Ausgabebaum.|
|guid|Der eindeutige Schlüssel, der dieses Modul identifiziert.|
|module|Name des Boxmoduls, das die XML-Ausgabe liefert.|

XML-Modus der Template-Engine
-----------------------------

Boxen werden immer separat geparst. Das von der Template-Engine intern verwendete XML-Dokument enthält anstelle der `<box>` -Elemente CDATA-Abschnitte mit der vorgeparsten HTML-Ausgabe des jeweiligen Boxmoduls.

**Format der internen XML-Ausgabe**

~~~~ {.xml}
...
<boxes>
<box title="" group="left" guid="6c7eca0f6e323f9d8f14f7070eccbfcb"
 module="actionbox_sitemap">
<![CDATA[
  <div class="navigation">
    <ul>
      <li class="active">
        <a href="startseite.2.html" class="active">Startseite</a>
      </li>
      <li>
        <a href="rezepte.5.html">Rezepte</a>
      </li>
      <li>
        <a href="kontaktformular.4.html">Kontaktformular</a>
      </li>
      <li>
        <a href="sitemap.3.html">Sitemap</a>
      </li>
    </ul>
  </div>
]]></box>
<box title="Rezepte" group="right" guid="41a921a5b6bb4ef582bf0526ec79e07d"
 module="actionbox_categteaser">
<![CDATA[
  <h3 class="teaserBoxTopicTitle">
    <a href="batida-de-papaya.7.html">Batida de Papaya</a>
  </h3>
  <div class="teaserBoxTopicTeaser">Ein Cocktail zum genießen.</div>
  <h3 class="teaserBoxTopicTitle">
    <a href="orangen-papaya-lassie.6.html">Orangen-Papaya-Lassie</a>
  </h3>
  <div class="teaserBoxTopicTeaser">Fruchtiger Longdrink. Genau das
     Richtige für heiße Sommertage.</div>
]]></box>
...
~~~~

Sie können sich die interne Version der XML-Ausgabe anzeigen lassen, indem Sie den Parameter „ `?XML=1` “ an die URL in der Navigationsleiste Ihres Browsers anhängen:

![File:XMLOutputInternesFormatAktivieren.png](images/XMLOutputInternesFormatAktivieren.png)

Beachten Sie jedoch, dass keine entsprechenden HTTP-Header mit Angaben zum MIME-Typ gesendet werden, weshalb das XML-Dokument von den Browsern nicht als XML formatiert wird. Der Grund hierfür ist, dass diese XML-Ausgabe durch entsprechende HTML-formatierte Fehlermeldungen ergänzt wird. Durch diese HTML-Elemente kann die Gültigkeit des ausgegebenen XML-Dokuments nicht immer garantiert werden. Damit es also nicht zu Fehlermeldungen des Browsers kommt, behandelt der Browser das Dokument nicht als XML-Dokument.

Sie müssen also die Funktion Ihres Browsers zur Anzeige des Seitenquellcodes aufrufen, um den XML-Quellcode sehen zu können.

[Kategorie:Formatvorlagen in papaya CMS](export_de/Kategorie:Formatvorlagen_in_papaya_CMS.md)
