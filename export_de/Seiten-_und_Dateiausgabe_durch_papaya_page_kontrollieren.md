---
title: Seiten- und Dateiausgabe durch papaya page kontrollieren
permalink: /Seiten-_und_Dateiausgabe_durch_papaya_page_kontrollieren/
---

Die Klasse `papaya_page` ist maßgeblich verantwortlich für die Seiten- und Dateienausgabe im Frontend. Außerdem werden in der Klasse `papaya_page` Fehlerkonstanten definiert. Anhand der Fehlernummern der entsprechenden Konstanten lässt sich leichter herausfinden, wodurch ein Problem verursacht wurde. Der Fehlercode erscheint hinter dem HTTP-Fehlercode . Im folgenden Screenshot wird die Ausgabe einer Fehlermeldung (hier die 303) dargestellt:

[miniatur|zentriert|1000px|Beispiel für eine Fehlerausgabe mit papaya-spezifischem Fehlercode](/images/File:Fehlermeldungen-500-303.png )

Im oben dargestellten Fall ist für den Ausgabemodus *html* kein Verzeichnis relativ zum eingestellten PAPAYA_LAYOUT_TEMPLATES angegeben. Dies wird mit dem Fehlercode 303 quittiert.

Funktion execute() ausführen
----------------------------

Nachdem in der `index.php` eine Instanz der Klasse `papaya_page` erzeugt worden ist, wird die Funktion `execute()` der Instanz aufgerufen. Die Funktion `execute()` ruft als erstes `papaya_page::startTimer()` auf. Dadurch wird der Zeitpunkt festgehalten, zu dem die Seitenausgabe gestartet wird.

Anschließend werden einige Konstanten und Klassenattribute gesetzt. Normalerweise enthält `$_SERVER['HTTP_HOST']` den Host-Header des Requests. Wenn der Client keinen Host-Header mitgeschickt hat, ist `$_SERVER['HTTP_HOST']` leer. Als Fallback wird der Wert von `$_SERVER['SERVER_NAME']` verwendet, um die Variable nutzen zu können. `$_SERVER['SERVER_NAME']` enthält den Servernamen oder VirtualHost, in dessen Umgebung papaya CMS läuft.

Dann wird das Domainhandling initialisiert und ausgeführt. Zu diesem Zweck wird die Klasse `base_domains` eingesetzt. Diese Klasse ermöglicht es mit einer papaya-Installation unterschiedliche Domains mit abweichenden Inhalten zu bedienen:

**Domainverwaltung einbinden**

~~~~ {.php}
    include_once(PAPAYA_INCLUDE_PATH.'system/base_domains.php');
    $this->domains = &new base_domains();
    $this->domains->handleDomain();
~~~~

`papaya_page::loadOptions()` instanziiert die Klasse `base_options` und führt `base_options::loadAndDefine()` aus. Sollte dies fehlschlagen, wird der anschließende Abschnitt zur Fehlerbehandlung ausgeführt. Es wird der 503-Header, Service unavailable, gesendet und versucht das Fehlerdokument zu lesen. Ist das Fehlerdokument vorhanden und lesbar, wird es ausgegeben, andernfalls wird die Standardfehlerausgabe von papaya CMS verwendet:

**Systemoptionen aus der Datenbank laden**

~~~~ {.php}
$this->domains = &new base_domains();
$this->domains->handleDomain();
if (!$this->loadOptions()) {
  $this->sendHTTPStatus(503);
  if (defined('PAPAYA_ERRORDOCUMENT_503') &&
      file_exists(PAPAYA_ERRORDOCUMENT_503) &&
      is_file(PAPAYA_ERRORDOCUMENT_503) &&
      is_readable(PAPAYA_ERRORDOCUMENT_503)) {
    $this->sendHeader('Content-type: text/html; charset=utf-8;');
    readfile(PAPAYA_ERRORDOCUMENT_503);
  } else {
    $this->getErrorHTML(503, 'Service Unavailable', PAPAYA_PAGE_ERROR_DATABASE);
  }
  exit;
}
~~~~

`papaya_page::getError()` verwendet einen Redirect um Fehler anzuzeigen. Wenn ein Access denied (403), File not found (404) oder ein nicht näher bezeichneter Internal Server Error (500) auftritt, wird `papaya_page::getError()` aufgerufen:

**Fehlermeldungen ausgeben**

~~~~ {.php}
if (isset($_GET['redirect']) && $_GET['redirect'] &&
    in_array($_GET['redirect'], array(403, 404, 500))) {
  $message = empty($_GET['msg']) ? '' : $_GET['msg'];
  $code = empty($_GET['code']) ? 0 : $_GET['code'];
  $this->getError($_GET['redirect'], $message, $code);
  exit;
}
~~~~

Im folgenden Schritt wird mit `base_object::parseRequestURI()` das Array `$_SERVER['REQUEST_URI']` zerlegt. Die Ergebnisse werden in `$this->requestData` gespeichert. `$_SERVER['REQUEST_URI']` enthält den Pfad auf dem Server ohne den Domainnamen. Dabei werden alle von papaya CMS benötigten Informationen wie Seiten-ID, Sprache usw. aus dem Pfad extrahiert:

**Request-URI parsen und in Bestandteile zerlegen**

~~~~ {.php}
$this->requestData = base_object::parseRequestURI();
~~~~

Diese extrahierten Informationen werden in der Methode `papaya_page::initializeParams()` verarbeitet.

papaya_page::initializeParams()
--------------------------------

Die Instanzmethode `initializeParams()` wird aufgerufen:

**Parameter initialisieren**

~~~~ {.php}
$this->initializeParams();
~~~~

Ziel dieser Methode ist es, die zu verwendende Seiten-ID zu ermitteln. Sie kümmert sich auch um eine eventuell vorhandene Box-ID. Dazu werden die Parameter `$_REQUEST['tt']['page_id']` und `$_REQUEST['p_id']` herangezogen:

**\$this-\>initializeParams: Topic-ID bestimmen**

~~~~ {.php}
function initializeParams() {
  if (isset($_REQUEST['tt']['page_id']) && $_REQUEST['tt']['page_id'] > 0) {
    $this->topicId = (int)$_REQUEST['tt']['page_id'];
  } elseif (isset($_REQUEST['p_id']) && $_REQUEST['p_id'] > 0) {
    $this->topicId = (int)$_REQUEST['p_id'];
  } elseif (isset($this->requestData['page_id']) &&
            $this->requestData['page_id'] > 0) {
    $this->topicId = (int)$this->requestData['page_id'];
    $_REQUEST['p_id'] = (int)$this->requestData['page_id'];
~~~~

Der Wert des Parameter `$_REQUEST['tt']['page_id']` wird dabei in der Instanzvariable `$this->topicId` gespeichert. Falls `$_REQUEST['tt']['page_id']` nicht gesetzt ist, wird stattdessen `$_REQUEST['p_id']` herangezogen.

Mit `papaya_page::checkAlias()` wird überprüft, ob es sich bei der URL um einen Alias handelt, also um eine Kurz-URL, die auf eine papaya-Seite verweist. Wird eine Aliasseite ermittelt, wird sie als aktuelle Seiten-ID gesetzt:

**\$this-\>initializeParams: Aliasseite bestimmen**

    <nowiki>} else {
      $aliasPage = $this->checkAlias();
      if (TRUE === $aliasPage) {
        $this->topicId = -1;
      } elseif (FALSE === $aliasPage) {
        $this->topicId = (int)PAPAYA_PAGEID_DEFAULT;
      } else {
        $this->topicId = (int)$aliasPage;
      }</nowiki>

Um diesen Block besser zu verstehen, wird im folgenden Abschnitt die Funktion checkAlias() erläutert.

Die Methode papaya_page::checkAlias()
--------------------------------------

Die Methode `papaya_page::checkAlias()` überprüft, ob für die angegebene URL ein Alias definiert ist. Die Aliasüberprüfung findet nur statt, wenn der Redirect-Status 404 (File not found) lautet, andernfalls ist die Datei oder der Ordner physisch vorhanden und soll auch verwendet werden. Eine 404-Statusmeldung kann in diesem Zusammenhang in folgenden Fällen auftreten:

1.  CSS-Hack für den IE-Mac
2.  Fehlende Datei `favicon.ico`

Diese Fälle müssen speziell behandelt werden.

Es kommt vor, dass der CSS-Hack für den IE-Mac von Browsern so interpretiert wird, das Requests der Form `http://www.domain.tld/(` oder `http://www.domain.tld/*/` entstehen. Damit diese nicht zu 404-Fehlern führen, werden sie von `papaya_page::checkAlias()` mit dem Code 200 (OK) quittiert und beendet.

Im folgenden Schritt werden 404-Redirects überprüft. Wenn also in der `REQUEST_URI` der Dateiname `favicon.ico` auftaucht, wird versucht, anstelle der fehlenden Datei im DocumentRoot ein `favicon.ico` im Theme-Pfad zu finden. Falls die Datei gefunden werden konnte, wird sie mit `papaya_file_delivery` ausgeliefert. Näheres dazu im Abschnitt `papaya_file_delivery`. Das Script wird anschließend zur Sicherheit beendet; in `papaya_file_delivery` sollte allerdings bereits ein `exit` ausgeführt worden sein. Wurde kein favicon gefunden, wird ein 404-Fehler generiert und das Script beendet:

**checkAlias() - Handling von 404-Fehlern**

~~~~ {.php}
if (isset($_SERVER['REDIRECT_STATUS']) && $_SERVER['REDIRECT_STATUS'] == 404) {
  $requestURI = isset($_SERVER['REQUEST_URI']) ? $_SERVER['REQUEST_URI'] : '';
  if (preg_match('(^/?([\\("]|\\\\"))', $requestURI)) {
    /* mac-ie css hack handling - path starts with ( or " */
    $this->sendHTTPStatus(200);
    exit;
  } elseif (preg_match('(^/?blank.gif)', $requestURI)) {
    //Handling of relative location headers emitted by ivw box
...
  } elseif (preg_match('(favicon\.ico)i', $requestURI)) {
    /* if we get an error for a favicon.ico - let's look in the theme */
    $webFile = 'papaya-themes/'.PAPAYA_LAYOUT_THEME.'/favicon.ico';
    $localFile = $this->getBasePath(TRUE).$webFile;
    if (file_exists($localFile) && is_readable($localFile)) {
      $fileData = array(
        'file_name' => 'favicon.ico',
        'file_size' => filesize($localFile),
        'mimetype' => 'image/x-icon'
      );
      $this->sendHTTPStatus(200);
      include_once(PAPAYA_INCLUDE_PATH.'system/papaya_file_delivery.php');
      papaya_file_delivery::outputFile($localFile, $fileData);
      exit;
    } else {
      $this->getError(
        404,
        'File/path '.$_SERVER['REDIRECT_URL'].' not found',
        PAPAYA_PAGE_ERROR_PATH
      );
    }
    exit;
  }
~~~~

Wenn weder ein CSS-Hack für den Mac-IE abgefangen noch ein `favicon.ico` gesucht werden muss, können echte Aliase behandeln werden. Dazu wird die Klasse `base_urlmounter` instanziiert und die Methode `base_urlmounter::locate()` aufgerufen. Die Methode `base_urlmounter::locate()` versucht aus der REQUEST_URI einen existierenden Alias abzulesen und die entsprechende papaya-Seite zu finden. Näheres dazu im Abschnitt `base_urlmounter`. `$alias` ist ein Array, dessen zweiter Wert den Redirect-Modus enthält (0 = Weiterleitung, 1 = Frameset, 2 = Modul). Je nach Redirect-Modus wird nun unterschiedlich vorgegangen:

**Weiterleitung über Aliasmodul**

~~~~ {.php}
include_once(PAPAYA_INCLUDE_PATH.'system/base_urlmounter.php');
$urlMounter = &new base_urlmounter;
if ($alias = $urlMounter->locate()) {
  switch ($alias[1]) {
  case 2 :
    if ($url = $urlMounter->executeAliasPlugin($alias[2])) {
      if (is_string($url)) {
        if ($alquiasURL = $urlMounter->getAliasURL($url)) {
          $this->sendHTTPStatus(302);
          $this->sendHeader('X-Papaya-Status: alias plugin redirect');
          $this->sendHeader('Location: '.$aliasURL);
          exit();
~~~~

Wenn der Redirect-Modus „2“ ist, wird ein Aliasmodul ausgeführt, das eine URL zurückliefern soll. Wenn die Ressource gefunden wird, die in der URL spezifiziert ist, sendet papaya CMS den Redirect-Header 302 (Moved Permanently) und leitet an die Seite weiter.

Wenn es sich nicht um ein Aliasmodul, sondern um einen einfachen Alias handelt, gibt die Funktion `checkAlias()` die ID der papaya-Seite zurück, auf die der Alias verweist:

**Weiterleitung über Alias-Seite**

~~~~ {.php}
} else {
  define('PAPAYA_ALIAS_PAGE', TRUE);
  //parse url and set request data
  include_once(PAPAYA_INCLUDE_PATH.'system/base_url_analyze.php');
  $requestData = base_url_analyze::parseURL($url);
  $this->requestData = $this->parseRequestURI($requestData['path']);
  $this->mode = $this->requestData['ext'];
  if (!empty($_SERVER['REDIRECT_QUERY_STRING'])) {
    $_GET = $this->queryStringToArray($_SERVER['REDIRECT_QUERY_STRING']);
  }
  if (!empty($requestData['query'])) {
    $_GET = array_merge_recursive(
      $_GET,
      $this->queryStringToArray($requestData['query'])
    );
  }
  return $this->requestData['page_id'];
}
  } elseif (is_array($url)) {
    return $this->setRequestFromAlias($url);
  } elseif ($url === TRUE) {
    //url was handled in plugin
    exit;
  }
}
break;
~~~~

Zurück zur Methode papaya_page::initializeParams()
---------------------------------------------------

Im letzten Schritt wird `$_REQUEST['p_id']` mit der ermittelten ID überschrieben, die im Attribut `$this->topicId` gespeichert worden ist:

**Feld \$_REQUEST['p_id] mit \$this-\>topicId überschreiben**

    <nowiki>  $_REQUEST['p_id'] = $this->topicId;
    }</nowiki>

Die Information in `$_REQUEST['p_id']` wird in `papaya_rpc` benötigt.

Wenn aus der URI eine Sprachangabe (language) ermittelt werden konnte, wird diese in `$_REQUEST['language']` gespeichert. `base_object::getWebLink()` verwendet diese Information.

**\$this-\>initializeParams: Sprachcode ermitteln**

    <nowiki>if (isset($this->requestData['language'])) {
      $_REQUEST['language'] = $this->requestData['language'];
    }</nowiki>

Im Attribut `$this->boxId` wird die ID der Box gespeichert, für die eine Seitenvorschau erzeugt werden soll. Für die Vorschau von Boxen wird immer eine Seite benötigt, die im Attribut `$this->topicId` angegeben ist:

**\$this-\>initializeParams: Box-ID ermitteln**

    <nowiki>  if (isset($_GET['papaya_box_preview']) && $_GET['papaya_box_preview'] > 0) {
        $this->boxId = (int)$_GET['papaya_box_preview'];
      } else {
        $this->boxId = -1;
      }
    }</nowiki>

Damit ist die Methode `initializeParams()` zu Ende. Im folgenden Abschnitt wird die Methode `initPageMode()` besprochen. Diese Methode bestimmt unter anderem den Ausgabemodus der Seite.

papaya_page::initPageMode()
----------------------------

In papaya_page::execute() wird im folgenden Schritt die Instanzmethode `$this->initPageMode()` aufgerufen:

**\$this-\>initPageMode() aufrufen**

~~~~ {.php}
$this->initPageMode();
~~~~

Mit der Methode `papaya_page::initPageMode()` wird die Seitenversion und der Ausgabemodus ermittelt. Außerdem wird festgestellt, ob die Session gestartet werden soll und wenn ja, ob sie schreibbar sein soll.

Zunächst wird die Seitenversion bestimmt. Dazu werden die Parameter `$_GET['p_date']` sowie der Requestparameter `datetime` ausgewertet, der in der URL enthalten sein kann:

**Seitenversion bestimmen**

~~~~ {.php}
function initPageMode() {
  $this->versionDateTime = 0;
  if (isset($_GET['p_date'])) {
    $this->versionDateTime = (int)$_GET['p_date'];
  } elseif (isset($this->requestData['datetime']) &&
            $this->requestData['datetime'] > 0) {
    $this->versionDateTime = (int)$this->requestData['datetime'];
  }
~~~~

Im Vorschaumodus wird der Status der Seite auf „nicht öffentlich“ ( `$this->public = FALSE` ) gesetzt. Andernfalls wird, wenn der Parameter `public` übergeben wird, der Status dann öffentlich, wenn dieser „yes“ enthält. Ist der Parameter nicht gesetzt, wird die Versionszeit auf „0“ gesetzt und der Status auf „öffentlich“ ( `$this->public = TRUE` ):

**Vorschaumodus bestimmen**

~~~~ {.php}
if (isset($this->requestData['preview']) && $this->requestData['preview']) {
  $this->public = FALSE;
} elseif (isset($_GET['public'])) {
  $this->public = ($_GET['public'] == 'yes');
} else {
  $this->versionDateTime = 0;
  $this->public = TRUE;
}
~~~~

**Hinweis:** In `base_object::parseRequestURI()` wird durch den regulären Ausdruck sichergestellt, dass es keine Versionszeit im öffentlichen Modus gibt.

Anschließend muss der Ausgabemodus bestimmt werden. Wenn die in der URL enthaltene Erweiterung `xml` ist, wird `$_GET['XML']` auf 1 gesetzt. Dieser Eintrag wird später durch das Layoutobjekt (eine Instanz der Klasse `papaya_xsl` ) ausgewertet, siehe [Seiten-Content für Preview-Modus und Frontend ausgeben](/Seiten-Content_für_Preview-Modus_und_Frontend_ausgeben ). Der Modus wird schließlich auf `xml` gesetzt. Ist `php` die Erweiterung, wird der konfigurierte Standardmodus verwendet, als Fallback `html`. Andernfalls wird der Modus anhand der Dateiendung gesetzt:

**Ausgabemodus bestimmen**

~~~~ {.php}
switch ($this->requestData['ext']) {
case 'xml':
  $_GET['XML'] = 1;
  $this->mode = 'xml';
  break;
case 'php':
  $this->mode = defined('PAPAYA_URL_EXTENSION') ?
    PAPAYA_URL_EXTENSION : 'html';
  break;
default:
  $this->mode = $this->requestData['ext'];
}
~~~~

Als nächstes wird `papaya_output` benötigt. Diese Klasse initialisierte den Ausgabefilter:

**Ausgabefilter initialisieren**

~~~~ {.php}
include_once(PAPAYA_INCLUDE_PATH.'system/papaya_output.php');
unset($this->output);
$this->output = &new papaya_output;
~~~~

Wenn der aktuelle Ausgabemodus in der Liste `$pageModes` enthalten ist, darf eine Session gestartet werden. Allerdings ist nur der lesende Zugriff erlaubt, da diese Ausgaben keine Änderungen benötigen:

**Read-Only-Session starten**

~~~~ {.php}
$pageModes = array('urls', 'status', 'thumb', 'media', 'popup', 'download');
if (isset($this->mode) && in_array($this->mode, $pageModes)) {
  $this->readOnlySession = TRUE;
  $this->allowSession = TRUE;
~~~~

Die Session nur lesend zu starten verkürzt die Scriptlaufzeit.

Kommt ein anderer Ausgabemodus zum Tragen, wird das Ausgabemodul angewiesen die Ansichtsdaten für den Modus zu laden. Je nach Konfiguration des Ausgabefilters wird eine Session gestartet oder nicht und entschieden, ob sie schreibbar ist oder nicht (1 = Nur lesen, 2 = Keine Session, 0 = Standard):

**Konfiguration des Ausgabefilters laden**

~~~~ {.php}
} elseif ($this->output->loadViewModeData($this->mode)) {
  switch ($this->output->viewMode['viewmode_sessionmode']) {
  case 1 :
    //read only session
    $this->allowSession = TRUE;
    $this->readOnlySession = TRUE;
    break;
  case 2 :
    //no session
    $this->allowSession = FALSE;
    $this->readOnlySession = TRUE;
    break;
  case 0 :
  default :
    //default handling
    $this->allowSession = TRUE;
    $this->readOnlySession = FALSE;
    break;
  }
~~~~

Die Session-Einstellung wird beim Ausgabefilter für jede Erweiterung eingestellt.

Wurde kein passender Modus für die Ansicht gefunden, wird eine änderbare Session erlaubt:

**Standard-Session-Einstellungen ausgeben**

~~~~ {.php}
  } else {
    $this->allowSession = TRUE;
    $this->readOnlySession = FALSE;
  }
}
~~~~

Damit endet die Methode `papaya_page::initPageMode()`.

papaya_page::startSession()
----------------------------

Im folgenden Schritt wird in `papaya_page::execute()` die Instanzmethode `startSession()` ausgeführt:

**Instanzmethode startSession() ausführen**

~~~~ {.php}
$this->startSession();
~~~~

Wenn per Konfiguration eine Session für jeden Besucher gestartet werden soll, initialisiert `papaya_page::startSession()` diese:

**Sessionstart-Flag setzen**

~~~~ {.php}
function startSession() {
  if (!(isset($this->sessionObj) && is_object($this->sessionObj))) {
    if (!$this->public) {
       $startSession = TRUE;
    } elseif ($this->allowSession && PAPAYA_START_SESSION) {
       $startSession = TRUE;
    } else {
       $startSession = FALSE;
    }
~~~~

Wenn eine nicht-öffentliche Seite aufgerufen wird, muss der Benutzer angemeldet sein; es wird auf jeden Fall eine Session benötigt. Wenn `papaya_page::initPageMode()` ergeben hat, dass der Ausgabemodus eine Session benötigt und die Konfiguration Sessions erlaubt, wird auch eine Session gestartet, andernfalls nicht.

Wenn ein Fallback für die Session konfiguriert wurde, wird dieses verwendet. Ansonsten wird `rewrite` verwendet. Falls die Seite nicht öffentlich ist und der Fallbackmodus `rewrite` oder `get` ist, wird der Fallbackmodus auch auf `rewrite` gesetzt:

**Fallback-Modus für Session-IDs setzen**

    <nowiki>$fallback = defined('PAPAYA_SESSION_ID_FALLBACK') ?
      PAPAYA_SESSION_ID_FALLBACK : 'rewrite';
    if (!($this->public && ($fallback == 'rewrite' || $fallback == 'get'))) {
      $fallback = 'rewrite';
    }</nowiki>

Wenn eine Session gestartet werden und die Datenbankverbindung nicht persistent sein soll, wird diese geschlossen. Dadurch wird die DB-Verbindung nicht belegt, während die Session erstellt wird:

**Nicht-persistente Datenbankverbindung schließen**

    <nowiki>if ($startSession &&
        !(defined('PAPAYA_DB_CONNECT_PERSISTENT') && PAPAYA_DB_CONNECT_PERSISTENT)) {
      $this->output->databaseClose();
    }</nowiki>

Nun wird das Sessionobjekt instanziiert und die Session gestartet (falls erwünscht):

**Sessionobjekt instanziieren**

    <nowiki>include_once(PAPAYA_INCLUDE_PATH.'system/sys_session.php');
    $this->sessionObj = &rewrite_session::getInstance(
      $this->sessionName,
      $startSession,
      $fallback);</nowiki>

Wenn die Session nur gelesen werden muss, wird sie direkt geschlossen. Damit sind die Sessionparameter nur noch lesbar.

**Read-Only-Session direkt schließen**

    <nowiki>if ($this->readOnlySession) {
      $this->sessionObj->close();
    }</nowiki>

Dann werden die Sessionparameter der Seite geladen:

**Session-Parameter der Seite laden**

    <nowiki>$this->sessionParams = $this->sessionObj->getValue('PAPAYA_SESSION_PAGE_PARAMS');</nowiki>

Zuletzt wird noch das Surferobjekt instanziiert. Damit werden später u.a. Zugriffsrechte überprüft:

**Surfer-Objekt instanziiert**

    <nowiki>  include_once(PAPAYA_INCLUDE_PATH.'system/base_surfer.php');
      $this->surferObj = &base_surfer::getInstance();
    }</nowiki>

Damit wird die Methode `papaya_page::startSession()` beendet.

Exit-Seiten bearbeiten
----------------------

Wenn eine Exitseite per `$_GET['exit']` angegeben wurde, wird die Weiterleitung angestoßen:

**Auf Exit-Seiten weiterleiten**

~~~~ {.php}
if (isset($_GET['exit'])) {
  if (preg_match('#^/sid'.preg_quote($this->sessionName).
      '([a-zA-Z\d,-]{20,40})([^?]*)#i',
      $_SERVER['REQUEST_URI'], $regs)) {
    $protocol = (isset($_SERVER['HTTPS']) && $_SERVER['HTTPS'] == 'on') ?
      'https' : 'http';
    $url = $protocol.'://'.strtolower(PAPAYA_DEFAULT_HOST).$regs[2].
     '?exit='.urlencode($_GET['exit']);
    $this->sendHTTPStatus(301);
    $this->sendHeader('X-Papaya-Status: external link');
    $this->sendHeader('Location: '.$url);
~~~~

Wenn eine Session-ID in der aktuellen URL vorhanden ist, wird diese entfernt und eine Weiterleitung angestoßen. Der HTTP-Status 301 besagt, dass die URL mit der Session-ID nicht korrekt ist und die gültige URL für diesen Link die nachfolgende URL ist (die ohne Session-ID).

Wenn keine Session mehr in der URL zu finden ist, wird die Ziel-URL als Exit-Seite für die Statistik geloggt:

**Ziel-URL als Exit-Seite für die Statistik protokollieren**

~~~~ {.php}
  } else {
    $targetURL = $_GET['exit'];
    $this->logRequestExitPage($targetURL);
  }
    exit;
}
~~~~

Dazu wird die Session geschlossen und überprüft, ob überhaupt Statistikdaten geschrieben werden sollen. Wenn ja, wird das Statistikobjekt `base_statistic_logging` initialisiert, der Request und die Exitpage geloggt. Da anschließend das Script beendet wird, kann das normale Loggen der Statistik bei Seitenaufrufen nicht erfolgen.

papaya_page::protectedRedirect(): Geschützte Weiterleitung auf externe URLs
----------------------------------------------------------------------------

Anschließend wird zur Zielseite weitergeleitet, wieder mit dem HTTP-Status 301 (Moved Permanently):

**Geschützte Weiterleitung auf externe Seite**

~~~~ {.php}
$this->protectedRedirect(301, $targetURL);
~~~~

Dabei wird sichergestellt, dass keine externen oder unerlaubten Seiten eine papaya-Installation nutzen, um URLs harmlos aussehen zu lassen. Der Methodenaufruf benötigt den HTTP-Status, der übermittelt werden soll, und eine Ziel-URL:

**papaya_page::protectedRedirect() aufrufen**

~~~~ {.php}
function protectedRedirect($code, $targetURL) {
  if (!defined('PAPAYA_REDIRECT_PROTECTION') || PAPAYA_REDIRECT_PROTECTION) {
    $protocol = (isset($_SERVER['HTTPS']) && $_SERVER['HTTPS'] == 'on') ?
      'https' : 'http';
    $systemURL = $protocol.'://'.strtolower(PAPAYA_DEFAULT_HOST);
~~~~

Wenn die Redirect-Protection nicht explizit ausgeschaltet wurde, wird zunächst das Protokoll sowie die System-URL ermittelt.

Wenn die Ziel-URL die System-URL enthält, ist die Domain identisch und die Weiterleitung wird erlaubt:

**Domains von Ziel- und System-URL vergleichen**

~~~~ {.php}
if (0 === strpos($targetURL, $systemURL)) {
  //own hostname - just redirect
  $this->sendHTTPStatus($status);
  $this->sendHeader('X-Papaya-Status: absolute link');
  $this->sendHeader('Location: '.$targetURL);ie
~~~~

Wenn der Referrer die eigene Domain beinhaltet, kommt der Aufruf von der eigenen Domain, die Weiterleitung wird erlaubt:

**Weiterleitung bei eigener Domain**

~~~~ {.php}
} elseif (isset($_SERVER['HTTP_REFERER']) &&
          0 === strpos($_SERVER['HTTP_REFERER'], $systemURL)) {
  //from own hostname - just redirect
  $this->sendHTTPStatus($status);
  $this->sendHeader('X-Papaya-Status: external link (referer checked)');
  $this->sendHeader('Location: '.$targetURL);
~~~~

Wenn die obigen Fälle nicht zutreffen, wird die Ziel-URL geprüft. Dazu wird in der Domainverwaltung geschaut, ob die Ziel-Domain in der Liste enthalten ist:

**Ziel-URL in der Domainverwaltung überprüfen**

~~~~ {.php}
} else {
  include_once(PAPAYA_INCLUDE_PATH.'system/base_url_analyze.php');
  $urlData = base_url_analyze::parseURL($targetURL);
  if ($this->domains->load($urlData['host'], 0)) {
    $this->sendHTTPStatus($status);
    $this->sendHeader('X-Papaya-Status: external link (domain checked)');
    $this->sendHeader('Location: '.$targetURL);
  } else {
    $this->getRedirect($code, $targetURL);
  }
}
~~~~

In dem Fall wird sie als bekannt akzeptiert und weitergeleitet. Wenn nicht, wird die Methode `getRedirect()` ausgeführt. Die genaue Funktionsweise dieser Methode ist in der Klassenreferenz von `papaya_page` erklärt. Auf einer speziellen papaya-Seite wird dem Benutzer anzeigt, dass er im Begriff ist die Webseite zu verlassen. Über einen Link kann er diese Seite erreichen, falls er das wirklich möchte. Wenn keine spezielle Seite vorhanden ist, wird eine einfache HTML-Standardausgabe angezeigt.

Wenn die Sicherheitsprüfung abgeschaltet ist, wird einfach auf die Zielseite weitergeleitet:

**Weiterleitung bei deaktivierter Sicherheitsüberprüfung**

~~~~ {.php}
  } else {
    //protection disabled
    $this->sendHTTPStatus($status);
    $this->sendHeader('X-Papaya-Status: redirect link');
    $this->sendHeader('Location: '.$targetURL);
  }
  exit;
}
~~~~

Die Methode `papaya_page::protectedRedirect()` beendet das Script.

Zurück in die Methode `papaya_page::execute()`. Das Exit-Handling wird durch ein `exit` abgeschlossen. Das Script wird damit beendet, es gibt nichts mehr zu tun. Der Browser leitet den Benutzer auf die Zielseite weiter.

papaya_page::execute(): Redirect-Request verarbeiten
-----------------------------------------------------

Wenn der Parameter `$_REQUEST['redirect']` gesetzt ist, wird die absolute Ziel-URL ermittelt und erneut ein abgesicherter Redirect durchgeführt (Details s.o.):

**Redirect-Request verarbeiten**

~~~~ {.php}
/* redirect script handling */
if (isset($_REQUEST['redirect'])) {
  $targetURL = base_object::getAbsoluteURL($_REQUEST['redirect'], @$_REQUEST['title']);
  $this->protectedRedirect(302, $targetURL);
  exit;
}
~~~~

In diesem Fall wird der HTTP-Status 302, Found, übermittelt. Mit diesem Status-Code wird dem Client mitgeteilt, dass die aktuelle URL weiterhin gültig ist. Das Script wird beendet.

Wenn Sie einen Default-Host eingerichtet haben und die Default-Action „redirect“ (\>0) ist, wird der Status 301 (moved permanently) übermittelt und mit dem aktuellen Protokoll auf die URL im Default-Host weitergeleitet:

**Weiterleitung auf Standard-Host**

~~~~ {.php}
if (defined('PAPAYA_DEFAULT_HOST') && trim(PAPAYA_DEFAULT_HOST) != '' &&
    defined('PAPAYA_DEFAULT_HOST_ACTION') && PAPAYA_DEFAULT_HOST_ACTION > 0 &&
    strtolower($_SERVER['HTTP_HOST']) != strtolower(PAPAYA_DEFAULT_HOST)) {
  $this->sendHTTPStatus(301);
  $protocol = (isset($_SERVER['HTTPS']) && $_SERVER['HTTPS'] == 'on') ?
    'https' : 'http';
  $url = $_SERVER['REQUEST_URI'];
  $this->sendHeader('X-Papaya-Status: redirecting to default host');
  $this->sendHeader('Location: '.$protocol.'://'.strtolower(PAPAYA_DEFAULT_HOST).$url);
  exit;
}
~~~~

Das Error-Reporting wird auf den Wert gesetzt, den Sie in der Konfiguration angegeben haben:

**Werte für Error-Reporting setzen**

~~~~ {.php}
if (defined('PAPAYA_DBG_PHP_ERROR')) {
  error_reporting(PAPAYA_DBG_PHP_ERROR);
}
~~~~

Anschließend wird das Logging-Objekt initialisiert, wenn PAPAYA_GLOBAL_LOGOBJECT gesetzt ist, was in `base_options` erfolgt:

**Globales Logging-Objekt initialisieren**

~~~~ {.php}
if (defined('PAPAYA_GLOBAL_LOGOBJECT') && (trim(PAPAYA_GLOBAL_LOGOBJECT) != '')) {
  include_once(PAPAYA_INCLUDE_PATH.'system/base_log.php');
  $GLOBALS[PAPAYA_GLOBAL_LOGOBJECT] = &new base_log();
}
~~~~

Nun wird noch überprüft, ob Gzip verwendet werden kann:

**GZip-Unterstützung überprüfen**

~~~~ {.php}
$this->checkAcceptGzip();
~~~~

Diese Funktion überprüft folgende Eigenschaften:

1.  Die Funktion `gzopen()` existiert, d.h. die Funktionalität ist in PHP vorhanden.
2.  Die Ausgabekompression ist in der Systemkonfiguration aktiviert worden.
3.  Die Cache-Vorhaltezeit ist größer als 0. Komprimieren hat nur Sinn, wenn das Ergebnis auch zwischengespeichert und später weiterverwendet werden kann.
4.  Die Server-Variable `$_SERVER['HTTP_ACCEPT_ENCODING']` muss gzip enthalten, d.h. der Client muss mit Inhalten, die mit `gzip` komprimiert worden sind, etwas anfangen können.

Die Methode `papaya_page::execute()` ist damit beendet. Der nächste und letzte Aufruf in der `index.php` ist die Methode `papaya_page::get()`.

Die Methode papaya_page::get()
-------------------------------

Anhand des in `papaya_page::initPageMode()` ermittelten Modus wird in der Methode `papaya_page::get()` entschieden, was für eine Art Ausgabe erfolgt. Im Switch-Case-Block werden anschließend entsprechede Methoden aufgerufen:

**papaya_page::get()**

~~~~ {.php}
function get() {
  $this->sendHeader('X-Generator: papaya CMS');
  switch ($this->mode) {
  case 'urls':
    $this->getUrls();
    break;
  case 'status':
    $this->getStatus();
    break;
  case 'image':
    $this->getDynamicImage();
    break;
  case 'thumb':
    $this->getMediaThumbFile();
    break;
  case 'media':
    $this->getMediaFile();
    break;
  case 'popup':
    $this->getMediaPopup();
    break;
  case 'download':
    $this->outputDownload();
    break;
  case 'outputs' :
    $this->getOutputs();
    break;
  case 'xml':
    $this->getXMLOutput();
    break;
  default:
    return $this->getPageOutput();
  }
}
~~~~

Die folgende Tabelle schlüsselt die Bedeutung der jeweiligen Modi im Switch-Case-Block auf:

|Modus|Erklärung|
|-----|---------|
|urls|Ausgabe aller veröffentlichten Seiten als simple Linkliste für Suchmaschinen|
|status|Verbindungsstatus der Datenbank, die die Information enthält, ob die Verbindung verfügbar ist oder nicht.|
|image|Ausgabe eines dynamisch generierten Bildes, z.B. für Captchas.|
|thumb|Ausgabe eines skalierten Bildes.|
|media|Ausgabe eines Bildes oder einer anderen Media-Datei.|
|popup|Ausgabe eines Bildes in einem Popup-Fenster.|
|download|Ausgabe einer Media-Datei als Download.|
|outputs|Liste aller möglichen Ausgabeversionen der aktuellen Seite.|
|xml|XML-Ausgabe der Seite, die vom XSLT-Template transformiert wird.|
|„default“-Fall|Die Seite wird im angegebenen Ausgabemodus gerendert. Ob dieser vorhanden ist, wird später geprüft.|

Damit ist auch das Script `index.php` beendet.

Im folgenden Abschnitt wird die Methode `papaya_page::sendHeader()` ausführlich beschrieben. Diese Methode wird in der Klasse `papaya_page` sehr oft benutzt, um HTTP-Header auszugeben. Sie ist also von sehr zentraler Bedeutung.

Die Methode papaya_page::sendHeader()
--------------------------------------

Die Methode `papaya_page::sendHeader()` ist ein Wrapper für die PHP-Funktion `header()`. Wurden bereits Header ausgegeben (passiert, wenn das erste Mal Text ausgegeben wird), wird beim ersten folgenden Aufruf von `sendHeader()` ein Eintrag in das Protokoll geschrieben. Auf einem Produktivsystem sollte niemals der Fall eintreten, dass Header gesendet werden, nachdem die Ausgabe erfolgt ist. Meistens passiert dies auf dem Entwicklungssystem, wenn eine Fehlermeldung ausgegeben wird. Zum Thema Konfiguration der Fehlerausgabe und der Debugeinstellungen bitte im Handbuch für Administratoren nachschlagen.

Zunächst werden die Variablen `$headerSent`, `$file` und `$line` initialisiert. Die lokale Variable `$headerSent` ist `static`, damit es bei jedem Aufruf der Methode in einem Scriptdurchlauf den Wert behält. `$file` und `$line` werden von `headers_sent()` gefüllt:

**Methodensignatur und lokale Variablen von sendHeader()**

~~~~ {.php}
function sendHeader($headerStr) {
  static $headersSent;
  $file = '';
  $line = '';
~~~~

Wurde in einem früheren Aufruf von `papaya_page::sendHeader()` mittels `headers_sent()` festgestellt, dass Header ausgegeben worden sind, ist `$headerSent``TRUE` und nichts weiter geschieht:

**Auf Headerausgabe prüfen**

~~~~ {.php}
if (isset($headersSent) && $headersSent) {
  //already sent some headers and reported an error
  return FALSE;
~~~~

Wenn keine Header ausgegeben worden sind, wird überprüft, ob dies mittlerweile geschehen ist. Wenn ja, enthalten `$file` und `$line` die Information, in welcher Zeile und in welcher Datei die Header ausgegeben worden sind:

**Headerausgabe bei Fehlern protokollieren**

~~~~ {.php}
} elseif ($headersSent = headers_sent($file, $line)) {
  //report the error to log
  $errorMsg = 'WARNING #2 Cannot modify header information - headers already sent';
  $this->logMsg(MSG_WARNING, 9, $errorMsg, $errorMsg.' in '.$file.':'.$line, TRUE, 2);
  return FALSE;
~~~~

Wurden noch keine Header ausgegeben, wird geprüft, ob die Konfigurationsoption PAPAYA_DISABLE_XHEADERS gesetzt und TRUE ist. Wenn ja, werden alle Header-Zeilen nicht ausgegeben, die mit „X-“ beginnen und via `papaya_page::sendHeader()` gesendet werden sollen:

**X-Header-Ausgabe unterdrücken**

~~~~ {.php}
  } else {
    if (defined('PAPAYA_DISABLE_XHEADERS') && PAPAYA_DISABLE_XHEADERS &&
        strtoupper(substr($headerStr, 0, 2)) == 'X-') {
      return 0;
    }
    header($headerStr);
  }
  return 0;
}
~~~~

Damit wird verhindert, dass z.B. der X-Generator-Header Aufschluss über das verwendete Content-Management-System gibt. Andere von papaya CMS generierte X-Header dienen der Fehleranalyse. Damit lassen sich z.B. Weiterleitungen nachvollziehen und in geringem Umfang debuggen.

Im folgenden Abschnitt wird beschrieben, wie die Systemoptionen für papaya CMS aus der Datenbank gelesen werden.

[export_de/Kategorie:Wie sieht es unter der Haube aus?](export_de/Kategorie:Wie_sieht_es_unter_der_Haube_aus? )