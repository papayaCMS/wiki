---
title: Installation in a subfolder: Changing the .htaccess file
permalink: /Installation_in_a_subfolder:_Changing_the_.htaccess_file/
---

**Wenn Sie das papaya CMS in einem Unterverzeichnis des Webservers bzw. des Document Roots installieren möchten, so muss die mitgelieferte `.htaccess` Datei entsprechend angepasst werden.**

Hierzu können Sie die im papaya Release mitgelieferte .htaccess verwenden, die in 2 Varianten enthalten ist:

1.  [htaccess.tpl](http://websvn.papaya-cms.com/wsvn/papayaCMS/trunk/papayaCMS/readme/htaccess.tpl) - eine Mustervorlage, bei der alle anzupassenden Pfadangaben mittels `%webpath_pages%` gekennzeichnet sind
2.  [.htaccess](http://websvn.papaya-cms.com/wsvn/papayaCMS/trunk/papayaCMS/.htaccess) - die im Hauptverzeichnis des entpackten papaya CMS befindliche Datei (diese ist standardmäßig so konfiguriert wie es bei einer Installation im Hauptverzeichnis/Document Root des Webservers notwendig wäre).

Alternativ - was auch der empfohlene Weg ist - können Sie den "**[papaya .htaccess Generator](http://www.papaya-cms.com/htaccess-generator)**" verwenden. Hier muss nur der gewünschte Pfadname eingegeben werden, der Server erzeugt dann für Sie die passende `.htaccess` Datei.

Das Tool ist unter <http://www.papaya-cms.com/htaccess-generator> verfügbar.

[Category:papaya CMS installieren und konfigurieren](export_en/Category:papaya_CMS_installieren_und_konfigurieren "wikilink") [en:Installation in a subfolder: Changing the .htaccess file](/en:Installation_in_a_subfolder:_Changing_the_.htaccess_file "wikilink")

papaya CMS contains an installation program that you can use via your browser. The installation program is located in the /papaya subdirectory and is called `install.php`. Enter *<http://www.your-domain.tld/papaya/install.php>* in your browser's address bar. Replace "your-domain" with your domain name and "tld" with the correct top level domain.

The installation script's start page displays some general information and links to pages with further information. To start the installation, follow these steps:

1.  In the *Installation/Update* section, click Next. A page containing the GPL license conditions is displayed.
2.  Click Next to accept the terms of the license. The next page displays the system check.
3.  Click next if your system matches all technical conditions for the succesful usage of papaya CMS. The Next link is only displayed if all tests have been successfully executed.
4.  Now enter the account data for the default administrator. You need to enter login name, given name, surname, email address, and password.
5.  Confirm the account data by clicking Save. After the administrator account has been successfully created, the Next link is activated.
6.  Click Next. A question dialog is displayed. Confirm the question dialog by clicking on Create to create the option table for the papaya CMS base configuration. After this table has been successfully created, the form page to initialize the database is displayed.

In the *Database Setup* section, the different steps are displayed as a numbered list of links. Click each of these links one after the other to complete the necessary steps to initialize the database. The last link will take you directly to the papaya CMS administration interface.

In the next step, you need to check the directories for the media database and for the website cache in the papaya CMS backend.

[Category:Installing and configuring papaya CMS](export_en/Category:Installing_and_configuring_papaya_CMS "wikilink") [de:Installation im Unterverzeichnis: .htaccess anpassen](/de:Installation_im_Unterverzeichnis:_.htaccess_anpassen "wikilink")