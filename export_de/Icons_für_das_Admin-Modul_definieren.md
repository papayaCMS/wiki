---
title: Icons für das Admin-Modul definieren
permalink: /Icons_für_das_Admin-Modul_definieren/
---

Jedes Paket kann eigene Icons für die Anwendung mitliefern, die im Unterverzeichnis `./DATA/pics` des Pakets gespeichert werden. Sie können die Icons in der Anwendung selbst für Buttons oder Icons zu benutzen oder als Icon für die Anwendung selbst. Das Icon für die Anwendung sollte in allen drei Größen (16x16, 22x22, 48x48) vorhanden sein.

Icon in der modules.xml im glyph-Attribut benutzen
--------------------------------------------------

Um das Icon für die Anwendung zu benutzen, gehen Sie wie folgt vor:

1.  Speichern Sie das Icon in den Größen 16x16, 22x22 und 48x48 Pixel in den jeweiligen Unterverzeichnissen des Verzeichnisses `./DATA/pics/` ab. Im folgenden Beispiel soll die Datei `example.png` benutzt werden: **Icon-Dateien in Unterverzeichnisse ./DATA/pics/ speichern**
        <nowiki>./DATA/pics/16x16/example.png
        ./DATA/pics/22x22/example.png
        ./DATA/pics/48x48/example.png</nowiki>

2.  Geben Sie den Dateinamen in der `modules.xml` im Attribut `glyph` an: **Icon im Attribut glyph angeben**
    ~~~~ {.xml}
    ...
    <module
      type="admin"
      guid="3a0db2ac6d10ce827666f568057e538c"
      name="Example"
      class="admin_example"
      file="admin_example.php"
      outputfilter="no"
      glyph="example.png">Description</module>
    ...
    ~~~~

3.  Speichern Sie die `modules.xml` ab und führen Sie in der Modulverwaltung den Modulscan durch, damit die geänderten Daten aus der `modules.xml` eingelesen werden.

Icons für das Bearbeitungsmenü benutzen
---------------------------------------

Näheres dazu, wie Sie Icons für das Bearbeitungsmenü benutzen können, erfahren Sie in [Eigene Icons aus dem Paket referenzieren](/Eigene_Icons_aus_dem_Paket_referenzieren ).

[Kategorie:Verzeichnisse und Metadaten für Pakete erstellen](Kategorie:Verzeichnisse_und_Metadaten_für_Pakete_erstellen )