
Die Klasse `base_options` enthält die von papaya CMS verwendeten Konstanten und initalisiert sie. Dazu gehören auch die Konstanten für die Basistabellen. Diese Konstanten werden verwendet, um Konfigurationsoptionen im ganzen System einheitlich auslesen zu können und um die Namen der Basistabellen zentral zu definieren.

Die Klasse base_options schrittweise erklärt

Die Klasse `base_options` erweitert `base_db`. Da viele Optionen im Backend von papaya CMS bequem über eine Benutzeroberfläche bearbeitet werden können, wird eine Datenbankkonnektivität benötigt:

![File: UML-Diagramm der Klasse base_options](images/BaseOptionsClassDiagram.png)

Zu Beginn der Klasse werden einige Klassenattribute gesetzt. So wird in `$tableOptions` der Name der Optionstabelle gespeichert. Dieser wird in der `conf.inc.php` in der Konstanten PAPAYA_DB_TBL_OPTIONS definiert und in dieser Klasse lediglich als Attribut zugewiesen. Die folgenden Attribute `$options`, `$optionGroups` sowie `$optLinks` werden in der von `base_options` abgeleiteten Klasse `papaya_options` verwendet.

Systemoptionen in der Klasse base_options

Die wichtigsten Attribute dieser Klasse sind `$optFields` und `$tables`. Diese enthalten Struktur und Standardwerte der Konfigurationsoptionen von papaya CMS. Eine Liste aller Konfigurationsoptionen ist im Administratorhandbuch zu finden. Dort werden sie auch ausführlich erklärt. Das folgende Beispiel stellt vor, wie Konfigurationsoptionen im Array `$optFields` definiert sind:

**Beispiel für eine Konfigurationsoption in \$optFields**

~~~~ {.php}
var $optFields = array(
  ...
  'PAPAYA_DBG_DATABASE_ERROR' => array(3, 'isNum', 'combo', array(1 => 'on', 0 => 'off'), 1)
);
~~~~

Der Array-Schlüssel ist der spätere Konstantenname. Der Wert des Arrays ist wieder ein Array. Es funktioniert ähnlich wie das `$editFields` -Array für Module, enthält statt des Titels jedoch eine Gruppen-ID, über die die Option einer Optionsgruppe zugeordnet wird.

|Attributschlüssel|Standardwert|
|0|Gruppen-ID|
|1|Funktion für Validitätsprüfungen (checkIt)|
|2|Eingabetyp|
|3|Konfigurationsoption für den Eingabetyp|
|4|Standardwert|

Die Gruppen sind in der Methode `base_statictables::getTableOptGroups()` festgelegt. In der folgenden Tabelle sind alle Optionsgruppen definiert, die Sie auch in der Systemkonfiguration im papaya-Backend finden können:

|Gruppen-ID|Gruppenname|
|0|Unbekannt|
|1|Tabellen (ungenutzt)|
|2|Dateien und Verzeichnisse|
|3|Fehlersuche|
|4|Language (ungenutzt)|
|5|Projektoptionen|
|6|Internes (ungenutzt)|
|7|System|
|8|Layout|
|9|Standardseiten|
|10|Zeichensätze|
|11|Übersichtsseite|
|12|Anmeldung|
|13|Support|
|14|MediaDB|
|15|Cache|
|16|Protokoll|

Tabellendefinitionen in der Klasse base_options

Die Datenbanktabellen des Basissystems werden alle im Array `$tables` der Klasse `base_options` gespeichert. Im folgenden Code-Beispiel ist dargestellt, wie eine Datenbanktabelle aufgeführt wird:

**Beispiel für eine Tabellendefinition im Array \$tables**

~~~~ {.php}
  var $tables = array(
    'PAPAYA_DB_TBL_LNG' => 'lng',
 .md);
~~~~

Als Schlüssel wird wieder der Konstantenname verwendet. Der Wert ist der Tabellenname ohne das Standardpräfix „ `papaya_` “. In der Methode `papaya_options::defineDatabaseTables()` erhalten die Konstantennamen für die Tabellen den vollständigen Tabellennamen.

Die folgende Tabelle listet alle Datenbanktabellen auf, die in der Klasse base_options definiert werden:

|Feldbeschriftung|Bedeutung|
|PAPAYA_DB_TBL_AUTHGROUPS|auth_groups|
|PAPAYA_DB_TBL_AUTHIP|auth_ip|
|PAPAYA_DB_TBL_AUTHLINK|auth_link|
|PAPAYA_DB_TBL_AUTHMODPERMLINKS|auth_modperm_link|
|PAPAYA_DB_TBL_AUTHMODPERMS|auth_modperm|
|PAPAYA_DB_TBL_AUTHOPTIONS|auth_useropt|
|PAPAYA_DB_TBL_AUTHPERM|auth_perm|
|PAPAYA_DB_TBL_AUTHTRY|auth_try|
|PAPAYA_DB_TBL_AUTHUSER|auth_user|
|PAPAYA_DB_TBL_BOX|box|
|PAPAYA_DB_TBL_BOXGROUP|boxgroups|
|PAPAYA_DB_TBL_BOXLINKS|boxlinks|
|PAPAYA_DB_TBL_BOX_PUBLIC|box_public|
|PAPAYA_DB_TBL_BOX_PUBLIC_TRANS|box_public_trans|
|PAPAYA_DB_TBL_BOX_TRANS|box_trans|
|PAPAYA_DB_TBL_BOX_VERSIONS|box_versions|
|PAPAYA_DB_TBL_BOX_VERSIONS_TRANS|box_versions_trans|
|PAPAYA_DB_TBL_CRONJOBS|cronjobs|
|PAPAYA_DB_TBL_DATAFILTER|datafilter|
|PAPAYA_DB_TBL_DATAFILTER_LINKS|datafilter_links|
|PAPAYA_DB_TBL_DOMAINS|domains|
|PAPAYA_DB_TBL_IMAGES|images|
|PAPAYA_DB_TBL_IMPORTFILTER|importfilter|
|PAPAYA_DB_TBL_IMPORTFILTER_LINKS|importfilter_links|
|PAPAYA_DB_TBL_LINKTYPES|linktypes|
|PAPAYA_DB_TBL_LNG|lng|
|PAPAYA_DB_TBL_LOCKING|locking|
|PAPAYA_DB_TBL_LOG|log|
|PAPAYA_DB_TBL_LOG_QUERIES|log_queries|
|PAPAYA_DB_TBL_MEDIADB_FILES|mediadb_files|
|PAPAYA_DB_TBL_MEDIADB_FILES_DERIVATIONS|mediadb_files_derivations|
|PAPAYA_DB_TBL_MEDIADB_FILES_TRANS|mediadb_files_trans|
|PAPAYA_DB_TBL_MEDIADB_FILES_VERSIONS|mediadb_files_versions|
|PAPAYA_DB_TBL_MEDIADB_FOLDERS|mediadb_folders|
|PAPAYA_DB_TBL_MEDIADB_FOLDERS_PERMISSIONS|mediadb_folders_permissions|
|PAPAYA_DB_TBL_MEDIADB_FOLDERS_TRANS|mediadb_folders_trans|
|PAPAYA_DB_TBL_MEDIADB_MIMEGROUPS|mediadb_mimegroups|
|PAPAYA_DB_TBL_MEDIADB_MIMEGROUPS_TRANS|mediadb_mimegroups_trans|
|PAPAYA_DB_TBL_MEDIADB_MIMETYPES|mediadb_mimetypes|
|PAPAYA_DB_TBL_MEDIADB_MIMETYPES_EXTENSIONS|mediadb_mimetypes_extensions|
|PAPAYA_DB_TBL_MEDIA_FILES|media_files|
|PAPAYA_DB_TBL_MEDIA_FILES_TRANS|media_files_trans|
|PAPAYA_DB_TBL_MEDIA_FOLDERS|media_folders|
|PAPAYA_DB_TBL_MEDIA_FOLDERS_TRANS|media_folders_trans|
|PAPAYA_DB_TBL_MEDIA_LINKS|media_links|
|PAPAYA_DB_TBL_MESSAGES|messages|
|PAPAYA_DB_TBL_MIMETYPES|mimetypes|
|PAPAYA_DB_TBL_MODULEGROUPS|modulegroups|
|PAPAYA_DB_TBL_MODULEOPTIONS|moduleoptions|
|PAPAYA_DB_TBL_MODULES|modules|
|PAPAYA_DB_TBL_PHRASE|phrase|
|PAPAYA_DB_TBL_PHRASE_LOG|phrase_log|
|PAPAYA_DB_TBL_PHRASE_MODULE|phrase_module|
|PAPAYA_DB_TBL_PHRASE_MODULE_REL|phrase_relmod|
|PAPAYA_DB_TBL_PHRASE_TRANS|phrase_trans|
|PAPAYA_DB_TBL_SPAM_CATEGORIES|spamcategories|
|PAPAYA_DB_TBL_SPAM_IGNORE|spamignore|
|PAPAYA_DB_TBL_SPAM_LOG|spamlog|
|PAPAYA_DB_TBL_SPAM_REFERENCES|spamreferences|
|PAPAYA_DB_TBL_SPAM_STOP|spamstop|
|PAPAYA_DB_TBL_SPAM_WORDS|spamwords|
|PAPAYA_DB_TBL_SURFER|surfer|
|PAPAYA_DB_TBL_SURFERACTIVITY|surferactivity|
|PAPAYA_DB_TBL_SURFERBLACKLIST|surferblacklist|
|PAPAYA_DB_TBL_SURFERCHANGEREQUESTS|surferchangerequests|
|PAPAYA_DB_TBL_SURFERCONTACTCACHE|surfercontactcache|
|PAPAYA_DB_TBL_SURFERCONTACTDATA|surfercontactdata|
|PAPAYA_DB_TBL_SURFERCONTACTPUBLIC|surfercontactpublic|
|PAPAYA_DB_TBL_SURFERCONTACTS|surfercontacts|
|PAPAYA_DB_TBL_SURFERDATA|surferdata|
|PAPAYA_DB_TBL_SURFERDATACLASSES|surferdataclasses|
|PAPAYA_DB_TBL_SURFERDATACLASSTITLES|surferdataclasstitles|
|PAPAYA_DB_TBL_SURFERDATATITLES|surferdatatitles|
|PAPAYA_DB_TBL_SURFERFAVORITES|surferfavorites|
|PAPAYA_DB_TBL_SURFERGROUPS|surfergroups|
|PAPAYA_DB_TBL_SURFERLISTS|surferlists|
|PAPAYA_DB_TBL_SURFERPERM|surferperm|
|PAPAYA_DB_TBL_SURFERPERMLINK|surferlinks|
|PAPAYA_DB_TBL_TAG|tag|
|PAPAYA_DB_TBL_TAG_CATEGORY|tag_category|
|PAPAYA_DB_TBL_TAG_CATEGORY_PERMISSIONS|tag_category_permissions|
|PAPAYA_DB_TBL_TAG_CATEGORY_TRANS|tag_category_trans|
|PAPAYA_DB_TBL_TAG_LINKS|tag_links|
|PAPAYA_DB_TBL_TAG_TRANS|tag_trans|
|PAPAYA_DB_TBL_TODOS|todos|
|PAPAYA_DB_TBL_TOPICS|topic|
|PAPAYA_DB_TBL_TOPICS_PUBLIC|topic_public|
|PAPAYA_DB_TBL_TOPICS_PUBLIC_TRANS|topic_public_trans|
|PAPAYA_DB_TBL_TOPICS_TRANS|topic_trans|
|PAPAYA_DB_TBL_TOPICS_VERSIONS|topic_versions|
|PAPAYA_DB_TBL_TOPICS_VERSIONS_TRANS|topic_versions_trans|
|PAPAYA_DB_TBL_URLS|urls|
|PAPAYA_DB_TBL_VIEWLINKS|viewlinks|
|PAPAYA_DB_TBL_VIEWMODES|viewmodes|
|PAPAYA_DB_TBL_VIEWS|views|

Die Methode `papaya_options::loadAndDefine()` ist die einzige von außen aufgerufene Methode dieser Klasse. Der Aufruf erfolgt im Konstruktor von `papaya_page`. Die Methode `loadAndDefine()` führt folgende Schritte aus:

1.  Aus der Optionstabelle in der Datenbank werden alle installationsspezifischen Werte der Konfigurationsoptionen ausgelesen und als Konstanten definiert. Dazu wird die Klassenmethode `base_options::define()` benutzt.
2.  `base_options::define()` definiert nur diejenigen Optionen, die noch nicht per `define()` definiert worden sind. Damit erhalten alle Systemoptionen Vorrang, die beispielsweise in der `conf.inc.php` definiert worden sind.
3.  Anschließend werden die Standardwerte aus dem Array `$optFields` gelesen. Wenn für eine Systemoption aus diesem Array kein Wert aus der Datenbank gefunden worden ist und sie folglich noch nicht definiert werden konnte, werden die Konstanten mit dem Standardwert aus diesem Array initialisiert.
4.  Im nächsten Schritt werden für das Protokoll-Objekt und das Übersetzungsobjekt Namen im globalen Namensraum vergeben: **Protokoll- und Übersetzungsobjekt in den globalen Namensraum setzen**

\$this-\>define('PAPAYA_GLOBAL_PHRASEOBJECT', 'PAPAYA_PHRASE_TRANSLATOR');

</source>
1.  Nun werden die ganzen Pfadkonstanten gesetzt: **Pfadkonstanten im globalen Namensraum**

\$this-\>define('PAPAYA_PATH_CACHE', PAPAYA_PATH_DATA.'cache/'); \$this-\>define('PAPAYA_PATH_MEDIAFILES_OLD', PAPAYA_PATH_DATA.'media_db/'); \$this-\>define('PAPAYA_PATH_THUMBFILES_OLD', PAPAYA_PATH_DATA.'media_thumbs/'); \$this-\>define('PAPAYA_PATH_MEDIAFILES', PAPAYA_PATH_DATA.'media/files/'); \$this-\>define('PAPAYA_PATH_THUMBFILES', PAPAYA_PATH_DATA.'media/thumbs/'); \$this-\>define('PAPAYA_PATH_TEMPLATES', PAPAYA_PATH_DATA.'templates/'.

` PAPAYA_LAYOUT_TEMPLATES.'/');`

\$path = str_replace('//', '/', PAPAYA_PATH_WEB.PAPAYA_PATH_ADMIN.'/'); \$this-\>define('PAPAYA_PATHWEB_ADMIN', \$path);

</source>
Die Pfadkonstanten ersparen es Ihnen als Entwickler, alle Pfade manuell zusammensetzen zu müssen. Pfadkonstanten erlauben zudem eine gewisse Flexibilität, wenn Verzeichnisstrukturen geändert werden, da die Basispfade alle zentral verwaltet werden.

1.  Im letzten Schritt wird die Methode `base_options::defineDatabaseTables()` ausgeführt. Diese Methode iteriert alle in `$this->tables` angelegten Konstanten- und Tabellennamen und definiert diese Konstanten. Bei der Definition wird das Tabellenpräfix aus der Konstanten PAPAYA_DB_TABLEPREFIX mit dem Tabellennamen verknüpft: **Konstanten der Tabellennamen definieren**

` if (isset($opt)) {`
`   $this->define($optionName, PAPAYA_DB_TABLEPREFIX.'_'.$opt);`
` }`

}

</source>
Hinweise zu den Pfadkonstanten in papaya CMS

|Konstantenname|Erläuterung|
|PAPAYA_MODULES_PATH|Absoluter Pfad zum Modul-Verzeichnis.|
|PAPAYA_PATH_CACHE|Absoluter Pfad zum Cache-Verzeichnis.|
|PAPAYA_PATH_MEDIAFILES_OLD|Alter Pfad zur MediaDB, wird bei der Konvertierung benötigt.|
|PAPAYA_PATH_THUMBFILES_OLD|Alter Pfad zu den Thumbnails (eigentlich nicht notwendig).|
|PAPAYA_PATH_MEDIAFILES|Absoluter Pfad zu den MediaDB-Dateien.|
|PAPAYA_PATH_THUMBFILES|Absoluter Pfad zu den Thumbnails.|
|PAPAYA_PATH_TEMPLATES|Absoluter Pfad zu den Templates.|
|PAPAYA_PATHWEB_ADMIN|Relativer Pfad zum Backend dieser papaya-Installation.|

[Kategorie:Wie sieht es unter der Haube aus?](export_de/Kategorie:Wie_sieht_es_unter_der_Haube_aus?.md)
