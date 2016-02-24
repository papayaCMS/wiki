---
title: Verzeichnisstruktur
permalink: /Verzeichnisstruktur/
---

Verzeichnisstruktur der Templates
---------------------------------

Die Templates für papaya CMS liegen im Verzeichnis `/papaya-data/templates/`. Jedes Template erhält dabei in diesem Verzeichnis ein eigenes Unterverzeichnis. In der Standarddistribution von papaya CMS befindet sich zurzeit ein Unterverzeichnis darin:

1.  `default-xhtml`: Dieses Templateset generiert HTML-Ausgaben mit der Dokumenttypdeklaration für XHTML in der Version 1.0.

Das Standard-Template hat folgende Verzeichnisstruktur:

_functions/
Enthält zahlreiche Standard-Templates zum Formatieren und Verarbeiten von Strings.

_lang/
Enthält XSLT-Templates, die für die Übersetzung von Phrasen in die aktuelle Content-Sprache benutzt werden. Weitere Templates dienen der sprachspezifischen Formatierung von Zahlen, Datumsangaben und Uhrzeiten. Das Verzeichnis enthält darüber hinaus noch einige XML-Dateien mit Standard-Übersetzungen für einige gängige Phrasen. Näheres zum Übersetzen von Phrasen erfahren Sie in [:Kategorie:Das Übersetzungsframework benutzen](/:Kategorie:Das_Übersetzungsframework_benutzen "wikilink").

feeds/
Enthält die Seitentemplates für die Ausgabe von Feeds wie Atom oder RSS, siehe [:Kategorie:RSS-Feeds erstellen](/:Kategorie:RSS-Feeds_erstellen "wikilink").

html/
Enthält die Seiten- und Boxentemplates für die HTML-Ausgabe, die mit den Ansichten verknüpft werden.

html/base/
Enthält Templates, die das HTML-Grundgerüst des Demotemplates erzeugen.

html/modules/
Enthält alle XSLT-Templates für die Boxen- und Seitenmodule aus den papaya-internen Paketen.

html/modules/_base/
Enthält Boxen- und Seitentemplates für Module aus dem Basispaket.

html/modules/commercial/
Enthält Boxen- und Seitentemplates für Module aus kommerziellen Paketen.

html/modules/free/
Enthält Boxen- und Seitentemplates für Module aus freien Paketen wie Forum, FAQ oder Quiz.

pdf/
Enthält die Seitentemplates für die PDF-Ausgabe, die mit den Ansichten verknüpft werden. Näheres zu PDF-Templates erfahren Sie in [:Kategorie:Vorlage für die PDF-Ausgabe erstellen](/:Kategorie:Vorlage_für_die_PDF-Ausgabe_erstellen "wikilink").

pdf/base/
Enthält das Standard-Stylesheet `tags.xsl`, das Inzeilige Elemente wie `<strong>` oder `<th>` in Elemente übersetzt, die durch den PDF-Ausgabefilter unterstützt werden.

pdf/files/
Enthält die PDF-Datei, die als Vorlage für die PDF-Ausgabe genutzt wird.

print/
Enthält die Seitentemplates für die Print-Templates. Die mit diesen Templates erzeugten Seitenausgaben sind für den Ausdruck optimiert. Näheres dazu erfahren Sie in [:Kategorie:Print-Templates erstellen](/:Kategorie:Print-Templates_erstellen "wikilink").

Wenn Sie ein eigenes Template anlegen, müssen Sie folgende Dinge beachten:

1.  Sie müssen für jedes neue Template einen Ordner in `/papaya-data/templates/` anlegen.
2.  Der Templateordner muss das Verzeichnis `/xslt/` enthalten, in dem alle Seiten- und Boxtemplates für die Verknüpfung mit den Ansichten vorhanden sind. Aus Gründen der Übersichtlichkeit erhalten die Dateinamen von Seitentemplates im Demotemplate das Präfix `page_` und die von Boxtemplates das Präfix `box_`.

Der Grund für die Trennung von Template und Theme ist technisch bedingt. CSS-Dateien und Layoutgrafiken werden immer durch den Webserver ausgeliefert. Zu diesem Zweck müssen die Dateien jedoch immer im DocumentRoot liegen. Da man jedoch das Verzeichnis `/papaya-data/` optional auch außerhalb des DocumentRoot ablegen kann, könnte man die Themes nicht mehr erreichen. Aus diesem Grund wurde die Trennung eingeführt.

Verzeichnisstruktur der Themes
------------------------------

Die Themes für papaya CMS liegen im Verzeichnis `papaya-themes/`. Jedes Theme wird dabei in einem eigenen Unterverzeichnis angelegt. So finden Sie in diesem Verzeichnis das Demo-Theme im Ordner `demo-theme/`. Das Demo-Theme hat folgende Verzeichnisstruktur:

./
Das Wurzelverzeichnis enthält alle CSS-Dateien des Themes, wobei für jedes Modul eine separate CSS-Datei definiert worden ist.

./js/
Das optionale Verzeichnis js enthält JavaScript-Dateien, die in Seiten eingebunden werden.

./papaya/
Dieses Standardverzeichnis enthält JavaScript- sowie Flash-Dateien, die bei bestimmten Standardanwendungen von papaya CMS benutzt werden. Die JavaScript-Funktion für Popup-Fenster befindet sich beispielsweise in diesem Verzeichnis.

./papaya/firebug/
Firebug-Dateien, mit denen Debug-Informationen für Firebug ausgegeben werden können. Dieses Verzeichnis kann optional für Themes benutzt werden.

./papaya/swfobject/
Eine JavaScript- und eine Flash-Datei, mit der Flash-Objekte in Webseiten eingebettet werden. swfobject überprüft zudem die Flash-Version auf dem Client des Nutzers und öffnet ggf. einen Download-Dialog, über den der Nutzer sein Flash-Plugin aktualisieren kann.

./pics
Das Bildverzeichnis enthält alle Layoutgrafiken.

Wenn Sie ein eigenes Theme erstellen möchten, erstellen Sie im Verzeichnis `papaya-themes/` ein neues Verzeichnis.

Falls Sie Flash oder Java-Applets verwenden, die Layout-Funktionen erfüllen, können Sie natürlich zusätzliche Verzeichnisse anlegen und bspw. mit `flash/` oder `java/` benennen. In allen anderen Fällen sollten Sie solche multimedialen Inhalte mit der Mediendatenbank verwalten.

Verzeichnis für clientseitige Scripte
-------------------------------------

Clientseitige Scripte für papaya CMS werden im Verzeichnis `papaya-script/` gespeichert. Das Verzeichnis enthält dabei lediglich generische Scripte, die von verschiedenen Themes genutzt werden können. Sie ermöglichen unter anderem, die Vollbildansicht von Bildern in Popup-Fenstern zu öffnen. Spezielle Scripte, die auf bestimmte Template-Theme-Sets angepasst sind, sollten Sie daher im jeweiligen Theme-Verzeichnis unterbringen.

Sie können natürlich für Ihr eigenes Projekt auch eine eigene Verzeichnisstruktur anlegen, in der Sie bspw. Ihre JavaScript-Bibliothek organisieren.

[Kategorie:Das Vorlagenkonzept in papaya CMS](/Kategorie:Das_Vorlagenkonzept_in_papaya_CMS "wikilink")