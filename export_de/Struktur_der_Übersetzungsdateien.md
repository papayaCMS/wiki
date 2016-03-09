---
title: Struktur der Übersetzungsdateien
permalink: /Struktur_der_Übersetzungsdateien/
---

Die in papaya CMS benutzten Übersetzungsdateien liegen im XML-Format vor und haben folgende Struktur:

**XMLSchema-Deklaration der Übersetzungsdatei**

~~~~ {.xml}
<?xml version="1.0"?>
<xs:schema xmlns:xs="http://www.w3.org/2001/XMLSchema">
   <xs:element name="texts">
     <xs:complexType>
       <xs:sequence>
         <xs:element name="text" minOccurs="1" maxOccurs="unbounded">
           <xs:complexType>
             <xs:simpleContent>
               <xs:extension base="xs:string">
                 <xs:attribute name="ident" type="xs:NMTOKEN"/>
               </xs:extension>
             </xs:simpleContent>
           </xs:complexType>
         </xs:element>
       </xs:sequence>
     </xs:complexType>
   </xs:element>
</xs:schema>
~~~~

Das Dokumentenelement `<texts>` enthält eine beliebige Menge von `<text>` -Elementen, die die eigentliche Phrasenübersetzung als Textknoten enthalten. Das Phrasensystem greift dabei über das Attribut `ident` (kurz für *identifier* ) auf die Übersetzung zu, wobei im Attribut `ident` die Phrase enthalten ist.

Ein konkretes Beispiel für eine Übersetzungsdatei ist im folgenden Listing dargestellt:

**Auszug aus der Standard-Übersetzungsdatei de-DE.xml**

~~~~ {.xml}
<?xml version="1.0"?>
<texts>
  <text ident="MORE">mehr</text>
  <text ident="SAVE">Speichern</text>
  <text ident="CANCEL">Abbrechen</text>
  <text ident="BACK">Zurück</text>
  <text ident="NEXT">Nächste</text>
  <text ident="PREV">Vorherige</text>
  <text ident="OVERVIEW">Übersicht</text>
  <text ident="DOWNLOAD">Download</text>
</texts>
~~~~

[Kategorie:Mit Übersetzungsdateien arbeiten](export_de/Kategorie:Mit_Übersetzungsdateien_arbeiten )