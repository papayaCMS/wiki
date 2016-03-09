---
title: Moduleinbindung vorbereiten
permalink: /Moduleinbindung_vorbereiten/
---

Damit Sie während der Entwicklung Ihr Modul testen können, müssen Sie es in papaya CMS registrieren. Dazu gehen Sie wie folgt vor:

1.  Legen Sie unterhalb von `./papaya-lib/modules/` ein Verzeichnis an, in das die PHP-Quellcodedateien gespeichert werden.
2.  Erstellen Sie in diesem Verzeichnis eine Datei mit dem Namen `modules.xml`. Kopieren Sie dazu einfach folgenden Inhalt in die Datei: **Minimale modules.xml anlegen**
    ~~~~ {.xml}
    <?xml version="1.0"?>
    <modulegroup>
      <name>Example</name>
      <description>Example box and page module</description>
      <modules>
        <module type="box"
                guid="9e0a3e7173945253494aa71831c3fbdd"
                name="My Box"
                class="actbox_example"
                file="actbox_example.php"
                outputfilter="yes"></module>
        <module type="page"
                guid="ba4b97036af3b67fe2649ac679cb9fe0"
                name="My Page"
                class="content_example"
                file="content_example.php"
                outputfilter="yes"></module>
      </modules>
    </modulegroup>
    ~~~~

    Passen Sie den Namen von Dateien und Klassen an Ihre eigenen Module an. Ändern Sie die GUIDs, indem Sie andere md5-Summen verwenden. Stellen Sie sicher, dass die von ihnen gewählten Namen für die Klassen und Dateien mit denen in der `modules.xml` übereinstimmen.

3.  Nachdem Sie die entsprechenden Quellcodedateien angelegt haben, können Sie im Backend von papaya CMS den Modulscan ausführen. Dabei wird die `modules.xml` in die Datenbank eingelesen und die Module werden registriert.

[export_de/Kategorie.md:Box- und Seitenmodule programmieren](export_de/Kategorie.md:Box-_und_Seitenmodule_programmieren )