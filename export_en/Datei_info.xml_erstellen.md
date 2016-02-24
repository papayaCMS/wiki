---
title: Datei info.xml erstellen
permalink: /images/File_info.xml_erstellen/
---

Die Datei `info.xml` enthält eine Liste aller Boxgruppen, die vom Template unterstützt werden. Damit die Benutzer Ihres Templates also nicht den Quellcode Ihrer Templates lesen müsen, um die Namen der Boxgruppen zu erfahren, sollten Sie Ihrem Template diese Datei beilegen.

Die `info.xml` wird automatisch durch papaya CMS eingelesen. Vorhandene Boxgruppen, deren Namen nicht in der info.xml enthalten sind, werden mit einem entsprechenden Warnicon markiert. Wenn unterstützte Boxgruppen noch nicht angelegt worden sind, werden entsprechende Gruppen mit dem Gruppe-Anlegen-Icon dargestellt. Näheres zur Verwaltung der Boxgruppen erfahren Sie im Handbuch "papaya CMS 5: Handbuch für Administratoren", Abschnitt 4.6.

Die folgende Datei stellt einen Beispiel der `info.xml` dar:

**Beispiel einer info.xml**

~~~~ {.xml}
<?xml version="1.0"?>
<info>
  <author>papaya Software GmbH</author>
  <boxes>
    <groups>
      <group name="copyright" />
      <group name="header" />
      <group name="navigation" />
      <group name="additional" />
      <group name="ariadne" />
      <group name="before-content" />
      <group name="after-content" />
      <group name="footer" />
      <group name="html-head" />
    </groups>
  </boxes>
</info>
~~~~

[Kategorie:Implementierungsphase: Webseitenvorlage erstellen](/Kategorie:Implementierungsphase:_Webseitenvorlage_erstellen "wikilink")