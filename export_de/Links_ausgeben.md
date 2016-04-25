papaya CMS stellt einige Methoden zur Verfügung, mit denen Sie Links ausgeben können. Die Links berücksichtigen dabei den Kontext der Anwendung und die Konfiguration von papaya CMS. Diese Methoden sind alle in der Klasse `base_object` definiert und können somit in allen von `base_object` abgeleiteten Klassen über `$this` aufgerufen werden:

|Methode|Verwendung|
|-------|----------|
|`getLink()`|Mit dieser Methode erstellen Sie Links im Backend von papaya CMS, siehe [Links ausgeben](Links_ausgeben.md).|
|`getWebLink()`|Mit dieser Methode erstellen Sie Links für die Frontend-Ausgabe von papaya CMS, siehe [Links ausgeben](Links_ausgeben.md).|
|`getAbsoluteURL()`|Mit dieser Methode erhalten Sie eine absolute URL inklusive Protokoll und Domainnamen, siehe [Links ausgeben](Links_ausgeben.md).|
|`getWebMediaLink()`|Mit dieser Methode erzeugen Sie eine absolute URL auf eine Datei aus der MediaDB, siehe [Links ausgeben](Links_ausgeben.md).|

In den folgenden Abschnitten werden die Methoden kurz vorgestellt.

Die Methode base_object::getLink()

Verwenden Sie `base_object::getLink()` um Links für Anwendungen im Backend zu erstellen. Alle Parameter sind optional.

|Parameter|Beschreibung|
|---------|------------|
|\$params|Die zu übergebenden Parameter, z.B. array('key' =\> 'value, 'key2' =\> 'othervalue'). Erstellt abhängig vom \$paramName (der Klasse oder der explizit übergebene) die GET-Parameter.|
|\$paramName|Der zu verwendende Parametername. Nur anzugeben, wenn er vom Standard-Parameternamen im Klassenattribut \$paramName abweicht, das im aktuellen Objekt gesetzt ist.|
|\$fileName|Name des auszuführenden PHP-Scriptes unterhalb von `/papaya`. Für Anwendungen ist das automatisch `modules.php`|
|\$pageId|Eine Seiten-ID, sofern auf eine andere Seite verwiesen werden soll.|

Die Methode base_object::getWebLink()

Verwenden Sie `base_object::getWebLink()` um einen Link für die Ausgabe im Frontend zu erstellen. Alle Parameter sind optional.

|Parameter|Beschreibung|
|\$pageId|Die ID der Seite, die aufgerufen werden soll. Standard ist die aktuelle Seiten-ID.|
|\$lng|Die Sprache, in der die Seite aufgerufen werden soll. Entspricht dem lng_ident, z.B. de, en, ...|
|\$mode|Modus, nicht der Ausgabemodus (html, pdf, ...), sondern ob Vorschau „preview“, XML-Vorschau (xmlpreview) oder normal („page“, default).|
|\$params|Parameter für diesen Link, `array('key' => 'value',
          ...)`|
|\$paramName|Der Parametername, falls abweichend von `$this->paramName`|
|\$text|Der sprechende Name der URL, standardmäßig der normalisierte Seitentitel oder „index“.|
|\$categId|Kategorie-ID. Die numerische ID kann sich auf eine export_de/Kategorie aus dem Katalog beziehen, wenn die Seite der export_de/Kategorie zugeordnet worden ist. Die ID kann sich jedoch auch auf jede andere Ressource beziehen, die mit dem Seitenrequest angefordert wird.|

Die Methode base_object::getAbsoluteURL()

Verwenden Sie `base_object::getAbsoluteURL()` um eine absolute URL zu erhalten. Die URL enthält das Protokoll und den Domainnamen. Die Methode sorgt dafür, dass die Session bei Bedarf nicht berücksichtigt wird. Bestandteile wie `/../` werden aufgelöst und Parameter sowie Targets ( `#top`.md) berücksichtigt.

|Parameter|Beschreibung|
|---------|------------|
|\$url|Relative oder absolute URL oder Seiten-ID.|
|\$text|Der sprechende Name der URL, standardmäßig der normalisierte Seitentitel oder „index“.|
|\$includeSession|„TRUE“, wenn die Session-ID in die URL übernommen werden soll, andernfalls „FALSE“.|

Die Methode base_object::getWebMediaLink()

Verwenden Sie `base_object::getWebMediaLink()` um anhand einer Media-ID einen vollständigen Link auf eine Datei zu erhalten. Die Methode gibt einen Dateinamen zurück, z.B. `dateiname.media.0123456789abcdef0123456789abcdef.png`


|Parameter|Beschreibung|
|---------|------------|
|\$mediaID|32-stellige hexadezimale ID der Datei|
|\$mode|Ausgabemodus: `media`, `download`, `popup` oder `thumb`:
| |		1.  `media`: Für die MediaDB-ID wird ein einfacher Link zur Datei zurückgegeben.
| |		2.  `download`: Dieser Modus wird so umgesetzt,dass entsprechende HTTP-Header gesetzt werden, damit der Browser den Datei-Speichern-Dialog darstellt.
| |		3.  `popup`: Die Datei wird in einem JavaScript-Popup-Fenster geöffnet. Bilder können damit in der Vollbildansicht dargestellt werden.
| |		4.  `thumb`: Die MediaDB-ID wird als Thumbnail-Link inklusive Größenangaben und Parametern interpretiert.|
|\$text|Der sprechende Name der URL, standardmäßig der normalisierte Dateiname oder „index“.|
|\$ext|Endung der Datei, z.B. „png“|

[Kategorie:Content ausgeben und Nutzereingaben maskieren](../export_de/Kategorie:Content_ausgeben_und_Nutzereingaben_maskieren.md)
