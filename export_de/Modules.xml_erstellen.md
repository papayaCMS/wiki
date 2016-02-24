---
title: Modules.xml erstellen
permalink: /Modules.xml_erstellen/
---

Damit die Module aus Ihrem Paket in papaya CMS verwendet werden können, müssen Sie diese in die Datei `modules.xml` eintragen. Anschließend können Sie die Module Ihres Pakets papaya CMS registrieren, Benutzen Sie dazu die Modulscan-Funktion in der Modulverwaltung.

Um eine `modules.xml` zu schreiben, gehen Sie bitte wie folgt vor:

1.  Legen Sie im Basisverzeichnis Ihres Pakets eine Datei mit dem Namen `modules.xml` an.
2.  Öffnen Sie die Datei mit einem Texteditor.
3.  Fügen Sie eine XML-Deklaration sowie das Dokumentenelement `<modulegroup>` hinzu: **XML-Deklaration und Dokumentenelement eingeben**
    ~~~~ {.xml}
    <?xml version="1.0"?>
    <modulegroup>
    </modulegroup>
    ~~~~

4.  Fügen Sie ein `<name>` -Element mit dem Namen des Pakets und ein `<description>` -Element mit einer kurzen Beschreibung des Paketinhalts in die `modules.xml` ein: **Paketname und -beschreibung eingeben**
    ~~~~ {.xml}
    <?xml version="1.0"?>
    <modulegroup>
      <name>Beispielpaket</name>
      <description>Dieses Paket macht dies und das.</description>
    </modulegroup>
    ~~~~

5.  Fügen Sie das Element `<modules>` ein. Dieses Element enthält für jedes Modul aus Ihrem Paket ein `<module>` -Element: **Modulbeschreibung eingeben**
    ~~~~ {.xml}
    <?xml version="1.0"?>
    <modulegroup>
      <name>Beispielpaket</name>
      <description>Dieses Paket macht dies und das.</description>
      <modules>
        <module type="" guid="" name="" class="" file="" outputfilter=""></module>
        <module type="" guid="" name="" class="" file="" outputfilter="" glyph=""></module>
        <!-- Weitere Module können folgen -->
      </modules>
    </modulegroup>
    ~~~~

6.  Tragen Sie für jedes Attribut die notwendigen Inhalte ein, um die Eigenschaften des Moduls zu spezifizieren: **Moduleigenschaften spezifizieren**
    ~~~~ {.xml}
    <?xml version="1.0"?>
    <modulegroup>
      <name>Beispielpaket</name>
      <description>Dieses Paket macht dies und das.</description>
      <modules>
        <module type="box"
                guid="6a5e040c2277317d3e378f6e91c6a3dc"
                name="Example box"
                class="example"
                file="actbox_example.php"
                outputfilter="yes"></module>
        <module type="admin"
                guid="3a0db2ac6d10ce827666f568057e538c"
                name="Example"
                class="admin_example"
                file="admin_example.php"
                outputfilter="no"
                glyph="example.png"></module>
        <!-- Weitere Module können folgen -->
      </modules>
    </modulegroup>
    ~~~~

    Näheres zu den Attributen des `<module>` -Elements erfahren Sie in Tabelle "Moduleigenschaften" in [modules.xml erstellen](/modules.xml_erstellen "wikilink").

7.  Geben Sie für jedes Modul einen Beschreibungstext innerhalb des `<module>` -Elements ein: **Modulbeschreibung eintragen**
    ~~~~ {.xml}
    <?xml version="1.0"?>
    <modulegroup>
      <name>Beispielpaket</name>
      <description>Dieses Paket macht dies und das.</description>
      <modules>
        <module type="box"
                guid="6a5e040c2277317d3e378f6e91c6a3dc"
                name="Example box"
                class="example"
                file="actbox_example.php"
                outputfilter="yes">Dieses Boxmodul stellt zufällige Beispielinhalte aus der
                                   Beispielanwendung dar.</module>
        <module type="admin"
                guid="3a0db2ac6d10ce827666f568057e538c"
                name="Example"
                class="admin_example"
                file="admin_example.php"
                outputfilter="no"
                glyph="example.png">Die Example-Anwendung dient dazu, Beispielinhalte zu pflegen,
                                    die in der Beispielbox ausgegeben werden können</module>
        <!-- Weitere Module können folgen -->
      </modules>
    </modulegroup>
    ~~~~

8.  Speichern Sie die Datei ab. Das Paket kann nun im Backend von papaya CMS in der Modulverwaltung registriert werden. Führen Sie dazu den Modulscan aus.

Im Folgenden wird das Content-Modell der `modules.xml` beschrieben.

Das Element <modulegroup>
-------------------------

Das Element `<modulegroup>` umfasst die gesamte Paketdefinition und besteht aus folgenden Elementen:

|Element|Funktion|
|-------|--------|
|`name`|Obligatorisch: Dieses Element enthält den Namen des Pakets. Der Name wird in der Modulverwaltung von papaya CMS dargestellt.|
|`description`|Obligatorisch. Dieses Element enthält eine kurze Beschreibung des Pakets.|
|`modules`|Obligatorisch: Enthält für jedes Modul des Pakets ein `<module>` -Element.|
|`tables`|Optional, enthält für jede Datenbanktabelle ein `<table>` -Element.|

Das Element <modules>
---------------------

Das Element `<modules>` enthält `<module>` -Elemente. Jedes `<module>` -Element beschreibt ein Modul, wobei die Eigenschaften des Moduls über Attribute erfasst werden. `<module>` -Elemente sollten den optionalen Beschreibungstext enthalten, der die Funktion des Moduls beschreibt. Der Nutzer erfährt darüber hinaus, wie man das Modul einsetzen kann. Der Beschreibungstext wird in der Modulverwaltung angezeigt, wenn Sie das Modul in der Modulverwaltung von papaya CMS anklicken. Die Attribute des `<module>` -Elements sind in der folgenden Tabelle aufgeschlüsselt:

|Attribut|Bedeutung|Notwendig|
|--------|---------|---------|
|`type`|Modultyp, siehe Tabelle "Modultypen" in [Modultypen in papaya CMS](/Modultypen_in_papaya_CMS "wikilink").|ja|
|`guid`|Eindeutiger 32-stelliger hexadezimaler Schlüssel.|ja|
|`name`|Der kurze und aussagekräftige Name des Moduls.|ja|
|`class`|Name der PHP-Klasse des Moduls.|ja|
|`file`|Name der PHP-Datei, die die Modulklasse enthält. Der Dateiname ist grundsätzlich gleich dem Klassennamen plus der Endung `.php`.|ja|
|`outputfilter`|„yes“, wenn ein Ausgabefilter notwendig ist, andernfalls „no“. Das Attribut kann auch ganz wegfallen, wobei es so interpretiert wird, als hätte es den Wert „yes“. Dieses Attribut ist nur für Content-Module vom Typ *page* und *box* relevant.|nein|
|`glyph`|Icon für Adminmodule|nein|

Die GUID identifiziert das Modul eindeutig. Dadurch ist es möglich, Dateinamen und Klassennamen von Modulen zu ändern und sogar Module in andere Pakete zu verschieben, ohne etwas in der Installation ändern zu müssen. Lediglich der Modulscan muss erneut durchgeführt werden. Sie finden einen md5-Zufallsgenerator z.B. unter <http://md5.ch> .

Sie können das Attribut „glyph“ bei Administrationsmodulen angeben, um der Anwendung ein Icon zuzuweisen. Näheres zu Icons für Anwendungen erfahren Sie in [Icons für das Admin-Modul definieren](/Icons_für_das_Admin-Modul_definieren "wikilink").

Das Element <tables>
--------------------

Das optionale `<tables>` -Element enthält `<table>` -Elemente, die alle für das Paket notwendigen Datenbanktabellen aufführen. Ein `<table>` -Element beschreibt dabei eine Tabelle, wobei der Name der Tabelle ohne Tabellen-Präfix im Attribut `name` aufgeführt wird. Standardmäßig lautet der Tabellenpräfix „papaya“. Da es jedoch erlaubt ist, abweichende Tabellenpräfixe zu benutzen, wird das Tabellenpräfix aus Gründen der Flexibilität nicht im Namen aufgeführt.

Die Struktur der Tabelle muss in der Datei `./DATA/table_tabellenname.xml` definiert sein. Der Name dieser Datei wird dabei aus dem Namen abgeleitet, der im name-Attribut des `<table>` -Elements angegeben ist. Näheres dazu, wie die Tabellen in die `modules.xml` eingetragen werden, erfahren Sie in [Tabellen in modules.xml referenzieren](/Tabellen_in_modules.xml_referenzieren "wikilink").

In [Tabellenstrukturdatei mit papaya CMS erstellen](/Tabellenstrukturdatei_mit_papaya_CMS_erstellen "wikilink") erfahren Sie, wie Sie die XML-Dateien mit der Strukturbeschreibung der Datenbanktabellen erstellen und in die `modules.xml` einfügen können.

[Kategorie:Verzeichnisse und Metadaten für Pakete erstellen](/Kategorie:Verzeichnisse_und_Metadaten_für_Pakete_erstellen "wikilink")