---
title: Installation im Unterverzeichnis: .htaccess anpassen
permalink: /Installation_im_Unterverzeichnis:_.htaccess_anpassen/
---

**Wenn Sie das papaya CMS in einem Unterverzeichnis des Webservers bzw. des Document Roots installieren möchten, so muss die mitgelieferte `.htaccess` Datei entsprechend angepasst werden.**

Hierzu können Sie die im papaya Release mitgelieferte .htaccess verwenden, die in 2 Varianten enthalten ist:

1.  [htaccess.tpl](http://websvn.papaya-cms.com/wsvn/papayaCMS/trunk/papayaCMS/readme/htaccess.tpl) - eine Mustervorlage, bei der alle anzupassenden Pfadangaben mittels `%webpath_pages%` gekennzeichnet sind
2.  [.htaccess](http://websvn.papaya-cms.com/wsvn/papayaCMS/trunk/papayaCMS/.htaccess) - die im Hauptverzeichnis des entpackten papaya CMS befindliche Datei (diese ist standardmäßig so konfiguriert wie es bei einer Installation im Hauptverzeichnis/Document Root des Webservers notwendig wäre).

Alternativ - was auch der empfohlene Weg ist - können Sie den "**papaya .htaccess Generator**" verwenden. Hier muss nur der gewünschte Pfadname eingegeben werden, der Server erzeugt dann für Sie die passende `.htaccess` Datei.

Nach erfolgreicher Installation müssen Sie im Backend von papaya CMS überpüfen, ob die Verzeichnisse der Mediendatenbank existieren und schreibbar sind. Dazu gehen Sie wie folgt vor:

1.  Klicken Sie in der Menügruppe „Administration“ auf Einstellungen . Die Systemkonfiguration wird dargestellt: Systemkonfiguration
2.  Klicken Sie im Bearbeitungsmenü auf Pfade prüfen . Wenn die Pfade existieren und schreibbar sind, wird eine entsprechende Infomeldung dargestellt.

Falls die Pfade nicht existieren sollten, wird papaya CMS sie anlegen. Eine Fehlermeldung wird jedoch in folgenden Fällen dargestellt:

1.  Die Pfade existieren, sind jedoch nicht schreibbar.
2.  papaya CMS versucht, die fehlenden Verzeichnisse anzulegen. Dieser Versuch schlägt jedoch fehl, da die notwendigen Dateirechte fehlen.

Wenn Sie die Pfade erfolgreich überprüft haben, ist die Installation abgeschlossen.

[export_de/Kategorie.md:papaya CMS installieren und konfigurieren](export_de/Kategorie.md:papaya_CMS_installieren_und_konfigurieren )