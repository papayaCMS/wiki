
Das Administrationsmodul besteht in papaya CMS aus zwei Klassen. Die erste Klasse ist das eigentliche Administrationsmodul und leitet von `base_module` ab. Sie erhält das Klassenpräfix `edmodule_`. Die eigentliche Darstellungs- und Anwendungslogik ist dabei in eine weitere Klasse ausgelagert. Diese Klasse leitet direkt von der Basisklasse der Anwendung ab und erhält das Klassenpräfix `admin_`.

Die `edmodule` -Klasse Instanziiert ein Objekt der `admin` -Klasse und definiert darüber hinaus noch Zugriffsrechte für die Klasse.

Die Klasse edmodule_stickers

Die Klasse `edmodule_stickers` ist sehr klein, da sie lediglich ein Attribut und eine Methode enthält. Das folgende Listing stellt das Grundgerüst der Klasse dar:

**Grundgerüst der edmodule_stickers-Klasse**

~~~~ {.php}
require_once(PAPAYA_INCLUDE_PATH.'system/base_module.php');

class edmodule_stickers extends base_module {
  var $permissions;
  function execModule() {}
}
~~~~

Mit `require_once` wird die Basisklasse `base_module.php` aus dem Basissystem geladen. Die Klasse `edmodule_stickers` leitet anschließend von `base_module` ab. Der Konvention nach beginnen Dateiname und Klassenname mit dem Präfix `edmodule_`.

Das Attribut `$permissions` ist ein Array, das eine Liste von Rechten enthält. Sie können bestimmte Funktionen in der Klasse `admin_stickers` mit Rechten verknüpfen, die in diesem Array definiert worden sind. Dadurch können die Funktionen nur durch berechtigte Nutzer oder Gruppen verwendet werden. Die entsprechenden Berechtigungen können in der Benutzerverwaltung erteilt werden, indem die Rechte aus dem Attribut `$permissions` mit der Benutzergruppe oder direkt mit dem Benutzer verknüpft werden.

**Attribut \$permissions mit Rechten**

~~~~ {.php}
var $permissions = array(
  1 => 'Manage',
);
~~~~

Im obigen Listing ist lediglich das Recht *Manage* definiert worden. Nutzer, die dieses Recht haben, können mit der Anwendung arbeiten und alle Funktionen der Anwendung nutzen. Sie können jedoch auch ein sehr viel feineres Rechtesystem anlegen. Erstellen Sie dazu weitere Rechte wie *Edit* oder *Delete* und verknüpfen Sie sie mit den entsprechenden Funktionen. Ein Nutzer muss also in diesem Fall mindestens das Recht *Manage* haben, um die Stickers-Anwendung benutzen zu können. Um Daten anlegen und bearbeiten zu können, benötigt er das Recht *Edit*. Um Daten Löschen zu können, ist schließlich das Recht *Delete* notwendig. Damit können Sie einzelnen Nutzern oder Gruppen explizit das Recht einräumen Datensätze zu erstellen, nicht jedoch sie zu löschen. Ein Nutzer muss zumindest das Recht *Manage* besitzen, damit er das Modul verwenden darf.

Das folgende Listing stellt die Implementation der Methode `execModule()` dar:

**Implementation der Methode execModule()**

~~~~ {.php}
function execModule() {
  if ($this->hasPerm(1, TRUE)) {
    include_once(dirname(__FILE__).'/admin_stickers.php');
    $stickers           = &new admin_stickers($this);
    $stickers->module   = &$this;
    $stickers->images   = &$this->images;
    $stickers->msgs     = &$this->msgs;
    $stickers->layout   = &$this->layout;
    $stickers->authUser = &$this->authUser;
    $stickers->initialize();
    $stickers->execute();
    $stickers->getXML();
  }
}
~~~~

Wenn Sie das Modul im Backend von papaya CMS starten, ruft die Modulverwaltung von papaya CMS als erstes die Methode `execModule()` der Klasse `edmodule_stickers` auf. Zunächst wird mit der Methode `base_module::hasPerm()` geprüft, ob der aktuelle Nutzer das Recht hat, das Modul zu verwenden. Wenn ja, wird das eigentliche Administrationsmodul eingebunden, instanziiert und initialisiert. Dazu werden einige Attribute als Referenz weitergegeben, die in der folgenden Tabelle näher aufgeschlüsselt werden:

|Attribut|Bedeutung|
|`$module`|Die Instanz von `edmodule`, also eine Subklasse von `base_module`. Diese wird benötigt, um beispielsweise die Modul-GUID zu ermitteln. Im Admin-Modul kann über das Attribut `$this->module->guid` auf die GUID zugegriffen werden.|
|`$images`|Enthält die papaya-internen Icons.|
|`$msgs`|Enthält das globale Messages-Array, an das mit `$this->addMsg()` Meldungen angehängt werden.|
|`$layout`|Das Layout-Objekt für das Backend. Es handelt sich dabei um eine Instanz von `papaya_xsl`. Mit diesem Objekt werden Dialog- sowie Formularelemente mit `add()`, `addLeft()` oder `addRight()` hinzugefügt oder Layoutparameter mit `setParam()` gesetzt.|
|`$authUser`|Objekt mit Daten des angemeldeten Benutzers, Instanz von `base_auth`.|

Anschließend werden die Methoden `initialize()`, `execute()` und `getXML()` des Adminobjekts aufgerufen. Mit `initialize()` wird das Objekt für die Ausführung vorbereitet. Dazu werden nötige Hilfsobjekte instanziiert und Default-Parameter gesetzt. In `execute()` werden vom Anwender ausgelöste Aktionen durchgeführt. Dazu gehört beispielsweise das Auswählen, Hinzufügen, Ändern oder Löschen eines Datensatzes. Der Name der Methode `getXML()` ist missverständlich, da nicht XML an die aufrufende Instanz zurückgegeben wird. Stattdessen wird das Objekt angewiesen, Ausgabe-XML an das Layoutobjekt weiterzugeben.

Registrieren Sie das Modul in der `modules.xml` als ein Modul vom Typ *admin*. Dort können Sie auch ein Icon angeben, das in der Anwendungsliste erscheint. Nähere Informationen zur Registrierung von Modulen finden Sie im Kapitel [modules.xml erstellen](/modules.xml_erstellen.md).

Die Klasse admin_stickers

Die Klasse `admin_stickers` beinhaltet die Darstellungs- und Anwendungslogik des Administrationsmoduls. Vom Benutzer angestoßene Aktionen werden durchgeführt, die Ausgabedaten zusammengestellt und in das Ausgabe-XML überführt. Das folgende Listing stellt das Grundgerüst des Administrationsmoduls vor. Die einzelnen Funktionen sind als Methodenrümpfe aufgelistet:

**Grundgerüst des Administrationsmoduls**

~~~~ {.php}
require_once(dirname(__FILE__).'/base_stickers.php');

class admin_stickers extends base_stickers {
  var $paramName;
  function initialize() {}
  function getXML() {}
  function getMenubarXML() {}
  function getCollectionsList() {}
  function getStickersList() {}
  function initializeCollectionDialog($cmd, $loadParams = TRUE) {}
  function getDeleteCollectionConfirmDialog() {}
}
~~~~

Die Klasse `admin_stickers` erweitert `base_stickers` und erhält dadurch die zuvor implementierten Datenbankfunktionen. Diese Datenbankfunktionen werden in der Klasse `admin_stickers` nun um die Anwendungs- und Darstellungslogik erweitert.

papaya CMS verwendet Parameternamen in POST und GET um Kollisionen zwischen Anwendungen zu vermeiden. Der Identifikator jeder Anwendung wird im Attribut `$paramName` festgelegt:

**Attribut \$paramName der Klasse admin_stickers**

~~~~ {.php}
  var $paramName = 'st';
~~~~

Das Attribut `$paramName` wird von Link-Methoden wie `getWebLink()` in `base_object` genutzt. Nähere Informationen zu Links finden Sie in [Links ausgeben](/Links_ausgeben.md).

Die Methode initialize()

Als erstes wird von `edmodule_sticker` die Methode `initialize()` aufgerufen. Diese Methode initialisiert zunächst die Sessions und macht den Parameter `col_id` persistent:

**Implementation der Methode initialize()**

~~~~ {.php}
  function initialize() {
    $this->sessionParamName = 'PAPAYA_SESS_'.$this->paramName;
    $this->initializeParams();
    $this->sessionParams = $this->getSessionValue($this->sessionParamName);
    $this->initializeSessionParam('col_id');
    $this->setSessionValue($this->sessionParamName, $this->sessionParams);

    $this->moduleImages['sticker']
      = sprintf('module:%s/pics/stickers.png', $this->module->guid);

    $this->layout->setParam('COLUMNWIDTH_LEFT', '50%');
    $this->layout->setParam('COLUMNWIDTH_CENTER', '0');
    $this->layout->setParam('COLUMNWIDTH_RIGHT', '50%');

    include_once(PAPAYA_INCLUDE_PATH.'system/base_btnbuilder.php');
    $this->menubar = &new base_btnbuilder;
    $this->menubar->images = &$this->images;

    if (!isset($this->collections)) {
      $this->collections = $this->getCollections();
    }
  }
~~~~

`Mehr Informationen zur Verwendung der Session finden Sie in `[`:Kategorie:POST/GET-Parameter` `lesen` `und` `Sessiondaten` `verwalten`](/:export_de/Kategorie:POST/GET-Parameter_lesen_und_Sessiondaten_verwalten.md)`. `

Um eine zentrale Stelle für modulspezifische Icons zu haben, werden die anwendungsspezifischen Icons im Attribut `$moduleImages` abgelegt. Mehr Informationen zur Verwendung von modulspezifischen Icons finden Sie in [Eigene Icons aus dem Paket referenzieren](/Eigene_Icons_aus_dem_Paket_referenzieren.md).

Das Layout der Administrationsoberfläche wird in zwei gleichgroße Spalten unterteilt. Dies wird dadurch erreicht, dass entsprechende Parameter im Layoutobjekt gesetzt werden, indem die Methode `$this->layout->setParam()` aufgerufen wird. Dabei werden die linke und die rechte Spalte des Layoutobjekts jeweils auf eine Breite von 50 % gesetzt, während die Breite der mittleren Spalte auf „0“ gesetzt wird.

Im folgenden Schritt wird das Attribut `$this->menubar` mit der Instanz der Klasse `base_btnbuilder` initialisiert. Mit dieser Klasse wird die Menüleiste zusammengestellt. Die Instanz `$this->menubar` erhält noch die Referenz auf das Attribut `$this->images`.

Im letzten Schritt der Initialisierungsmethode werden noch alle bestehenden Sammlungen in das Attribut `$this->collections` geladen. Damit stehen sie für den Rest des Programmablaufes zur Verfügung.

Die Methode getXML()

Sie können die Programmlogik implementieren, ohne die Ausgabe implementiert zu haben. Für den Zweck dieses Tutorials ist es nützlicher, zunächst die Ausgabe zu betrachten und dann die Logik, da diese Vorgehensweise anschaulicher ist.

Für die Ausgabe benötigen Sie ein Menü, das Bearbeitungsfunktionen zur Verfügung stellt, eine Übersicht über alle Sammlungen, eine Liste der Sticker in der ausgewählten Sammlung und Bearbeitungsformulare. Die Ausgabe wird von der Methode `getXML()` gesteuert, deren Implementation im folgenden Listing dargestellt wird:

**Implementation der Methode getXML()**

~~~~ {.php}
function getXML() {
  $this->layout->addMenu($this->getMenubarXML());
  $this->layout->addLeft($this->getCollectionsList());
  $this->layout->addLeft($this->getStickersList());
  if (isset($this->dialog) && is_object($this->dialog) &&
      'base_dialog' == get_class($this->dialog)) {
    $this->layout->addRight($this->dialog->getDialogXML());
  }
}
~~~~

Die Methode `getXML()` ruft die XML-Ausgabemethoden der Klasse auf und fügt die Ausgaben in das Layoutobjekt `$this->layout` ein. Im ersten Schritt wird mit `addMenu()` das Bearbeitungsmenü dem Layout-Objekt hinzugefügt. Die Ausgabe hierzu kommt von der Methode `getMenubarXML()`. Anschließend wird der linken Spalte des Content-Bereichs mit der Layoutobjektmethode `addLeft()` eine Liste mit den Sammlungen hinzugefügt. Schließlich wird auch die Liste mit den Stickers in die linke Spalte des Content-Bereichs eingefügt. Dazu wird wieder die Layoutobjektmethode `addLeft()` benutzt.

Sofern ein Dialogobjekt im Klassenattribut `$this->dialog` vorhanden ist, wird die XML-Ausgabe des Dialogs in die rechte Spalte des Content-Bereichs gesetzt. Dazu wird die Methode `$this->layout->addRight()` des Layoutobjekts benutzt. Das Dialogobjekt `$this->dialog` besitzt die Methode `getDialogXML()` für die XML-Ausgabe, die in diesem Zusammenhang genutzt wird.

In den folgenden Abschnitten werden die Methoden `getMenubarXML()`, `getCollectionsList()` sowie `getStickersList()` im Detail vorgestellt.

Die Methode getMenubarXML()

Mit der Methode `getMenubarXML()` wird das XML für das Bearbeitungsmenü erzeugt:

**Implementation der Methode getMenubarXML()**

~~~~ {.php}
function getMenubarXML() {
  $result = '';
  $this->menubar->addButton('add collection', $this->getLink(
    array('cmd' => 'add_collection')), 'actions-folder-add',
    'add a new collection',
    (!empty($this->params['cmd']) && $this->params['cmd'] == 'add_collection'));
  if (isset($this->params['col_id']) && $this->params['col_id'] > 0) {
    $this->menubar->addButton('delete collection', $this->getLink(array(
      'cmd'        => 'del_collection',
      'sticker_id' => $this->params['col_id'])), 'actions-folder-delete',
      'delete the current collection',
      (!empty($this->params['cmd']) && $this->params['cmd'] == 'del_collection'));
    $this->menubar->addSeperator();
    $this->menubar->addButton('add sticker', $this->getLink(array(
      'cmd'    => 'add_sticker',
      'col_id' => $this->params['col_id'])), 'actions-page-add',
      'add a sticker to the current collection',
      (!empty($this->params['cmd']) && $this->params['cmd'] == 'add_sticker'));
  }
  if ($menu = $this->menubar->getXML()) {
    $result = sprintf('<menu>%s</menu>'.LF, $menu);
  }
  return $result;
}
~~~~

Das Klassenattribut `$menubar` wurde in der Methode `initialize()` mit einer Instanz von `base_btnbuilder` belegt. Diesem Objekt werden in der Methode `getMenubarXML()` Buttons hinzugefügt. Dazu wird die Methode addButton() aufgerufen, die fünf Parameter enthält:

1.  Der erste Parameter ist der Buttontext. Er wird automatisch übersetzt.
2.  Der zweite Parameter ist der Link, der bei einem Klick auf den Button aufgerufen wird. Er wird mit der Methode `getLink()` erzeugt.
3.  Der dritte Parameter enthält den Iconnamen und entspricht dem Schlüssel des Icons im Array `$this->images`.
4.  Der vierte Parameter enthält einen Tooltip-Text, die bei Mouseover eingeblendet wird.
5.  Der fünfte Parameter ist ein boolescher Wert, der aussagt, ob der Button eingedrückt dargestellt werden soll oder nicht.

Standardmäßig wird stets der Button Add collection dargestellt. Wenn der Parameter `$this->params['col_id']` gesetzt ist und einen Wert größer `0` besitzt, ist eine Sammlung ausgewählt. In diesem Fall werden zusätzlich noch die Buttons Delete collection sowie Add Sticker dargestellt. Zusätzlich werden die Buttons, deren Funktionen sich auf eine Sammlung beziehen, von den Sticker-spezifischen Buttons durch einen Separator getrennt. Hierzu fügt die Methode `addSeparator()` einen vertikalen grafischen Trenner in die Toolbar ein.

Bei der Ausgabe in XML wird jeder Button als `<button>` -Element ausgegeben. Damit diese Elemente ein übergeordnetes Element erhalten, wird die Ausgabe in ein `<menu>` -Tag eingebettet. Die Rückgabe der Methode `getMenubarXML()` entspricht folgender XML-Struktur:

**Beispielausgabe der Methode getMenubarXML()**

~~~~ {.xml}
<menu>
  <button title="add collection"
          href="module_edmodule_stickers.php?st[cmd]=add_collection"
          glyph="actions/folder-add.png"
          hint="add a new collection"
          target="_self"/>
  ...
</menu>
~~~~

Die Methode getCollectionsList()

Die Methode `getCollectionsList()` erzeugt eine Listview der vorhandenen Sammlungen und gibt diese als XML-Präsentation zurück. Eine Listview ist eine tabellarische Darstellung der Daten. Das folgende Listing stellt die Struktur der XML-Ausgabe vor:

**Beispielausgabe der Methode getCollectionsList()**

~~~~ {.xml}
<listview title="Titel">
  <items>
    <listitem title="Name"><subitem>Inhalt</subitem></listitem>
    ...
  </items>
</listview>
~~~~

Näheres zum Aufbau von Listviews erfahren Sie in [:Kategorie:Backend-Komponenten](/:export_de/Kategorie:Backend-Komponenten.md).

Grundsätzlich gehen Sie wie folgt vor, wenn Sie eine Listviewausgabe erstellen:

1.  Sie initialisieren die Ergebnisvariable `$result`.
2.  Sie laden die benötigten Daten in ein Array.
3.  Sie fügen ein öffnendes `<listview>` -Element in die Ausgabe und geben der Liste einen Titel.
4.  Sie geben für jedes Datenelement ein `<listitem>` -Element aus. Ist die Datenmenge leer, geben Sie optional eine Statusmeldung für den Benutzer aus.
5.  Sie fügen ein schließendes `<listview>` -Element der Ergebnisvariablen `$result` hinzu. Damit wird die Listview geschlossen.
6.  Geben Sie mit `return` die `$result` -Variable zurück.

Das folgende Listing stellt das Grundgerüst der Methode `getCollectionsList()` vor:

**Grundgerüst der Methode getCollectionsList()**

~~~~ {.php}
function getCollectionsList() {
  $result = '';
  $this->collections = $this->getCollections();
  $result .= sprintf('<listview title="%s">'.LF,
    papaya_strings::escapeHTMLChars($this->_gt('Collections')));
  if (is_array($this->collections)&& count($this->collections) > 0) {
    // ...
  } else {
    $result .= sprintf('<status>%s</status>', $this->_gt('No collections found.'));
  }
  $result .= '</listview>';
  return $result;
}
~~~~

Im Grundgerüst der Methode getCollectionsList() werden zunächst die Sammlungen geladen. Anschließend wird ein öffnendes `<listitem>` -Element mit dem passenden Titel an die `$result` -Variable gehängt. Wenn die Liste der Sammlungen `$this->collections` leer ist, wird eine Statusmeldung in einem `<status>` -Element ausgegeben, andernfalls werden die `<listitem>` -Elemente ausgegeben. Anschließend wird das `<listitem>` -Element geschlossen und die `$result` -Variable per `return` zurückgegeben.

Wenn der Titel für das `<listitem>` -Element in das `title` -Attribut ausgegeben wird, muss der Text mit der Methode `papaya_strings::escapeHTMLChars()` maskiert werden. Damit wird verhindert, dass das XML von Eingaben wie `"><` zerstört wird.

Für die Ausgabe der `<listitem>` -Elemente werden alle Objekte des Datenarrays iteriert. Für jeden Datensatz wird dabei ein `<listitem>` -Element ausgegeben. Die `<listitem>` -Elemente werden alle durch ein `<item>` -Element umschlossen, wie das folgende Listing darstellt:

**<listitem>-Ausgabe in getCollectionsList()**

~~~~ {.php}
$result .= '<items>'.LF;
foreach($this->collections as $collectionId => $collection) {
  if (isset($this->params['col_id']) && $collectionId == $this->params['col_id']) {
    $selected = ' selected="selected"';
  } else {
    $selected = '';
  }
  $result .= sprintf('<listitem image="%s" title="%s" href="%s" %s>'.LF,
    $this->images['items-folder'],
    papaya_strings::escapeHTMLChars($collection['collection_title']),
    $this->getLink(array('cmd' => 'edit_collection', 'col_id' => $collectionId)), $selected);
  $result .= '</listitem>';
}
$result .= '</items>'.LF;
~~~~

Bei der Ausgabe der `<listitem>` -Elemente wird überprüft, ob die aktuelle Sammlung ausgewählt ist. Ist dies der Fall, erhält das `<listitem>` -Element das Attribut `selected="selected"`. Dieses Attribut bewirkt, dass das betroffene `<listitem>` -Element in der Benutzeroberfläche hervorgehoben wird.

Das folgende Listing stellt die Beispiel-Ausgabe der `getCollectionsList()` -Methode dar:

**Beispielausgabe der Methode getCollectionsList()**

~~~~ {.xml}
<items>
  <listitem image="items/folder.png"
            title="meine Sammlung"
            href="module_edmodule_stickers.php?st[cmd]=edit_collection&st[col_id]=1"
            selected="selected">
  </listitem>
</items>
~~~~

Die Methode `getStickersList()` soll eine Liste aller in einer Sammlung vorhandenen Sticker anzeigen. Die Methode ist ähnlich implementiert wie `getCollectionsList()`. Im folgenden Listing ist das Grundgerüst der `getStickersList()` -Methode dargestellt:

**Grundgerüst der Methode getStickersList()**

~~~~ {.php}
function getStickersList() {
  $result = '';
  if (isset($this->params['col_id']) && $this->params['col_id'] > 0) {
    $stickers = $this->getStickersByCollection($this->params['col_id'], 100, 0);
    $result .= sprintf('<listview title="%s">'.LF,
      papaya_strings::escapeHTMLChars($this->_gt('Stickers')));
    if (is_array($stickers) && count($stickers) > 0) {
      // ...
    } else {
      $result .= sprintf('<status>%s</status>'.LF, $this->_gt('No stickers in this collection.'));
    }
    $result .= '</listview>';
  }
  return $result;
}
~~~~

Die Methode getStickersList()

Die Listview für die Stickerliste wird wie bei der Sammlung ausgegeben. Bei der `<items>` -Ausgabe unterscheidet sich das Vorgehen, weil drei Spalten benötigt werden:

1.  Sticker-ID
2.  Sticker-Titel
3.  Löschfunktion (Im Gegensatz zu Sammlungen, die über den Button im Bearbeitungsmenü gelöscht werden)

**<listitem>-Ausgabe in getStickersList()**

~~~~ {.php}
$result .= '<items>'.LF;
foreach($stickers as $stickerId => $sticker) {
  if (isset($this->params['sticker_id']) && $this->params['sticker_id'] == $stickerId)
    $selected = ' selected="selected"';
  } else {
    $selected = '';
  }
  $result .= sprintf('<listitem image="%s" title="#%d" href="%s" %s>'.LF,
    $this->moduleImages['sticker'], $stickerId,
    $this->getLink(array('cmd' => 'edit_sticker', 'sticker_id' => $stickerId)),
    $selected);
  $result .= sprintf('<subitem><a href="%s">%s</a></subitem>'.LF,
    $this->getLink(array('cmd' => 'edit_sticker', 'sticker_id' => $stickerId)),
    papaya_strings::truncate(strip_tags($sticker['sticker_text'])));
  $result .= sprintf('<subitem><a href="%s"><glyph src="%s" /></a></subitem>'.LF,
    $this->getLink(array('cmd' => 'del_sticker', 'sticker_id' => $stickerId)),
    $this->images['actions-page-delete']);
  $result .= '</listitem>';
}
$result .= '</items>'.LF;
~~~~

Für das Icon in der ersten Spalte wird das Icon aus dem Stickers-Paket benutzt („sticker“), das zuvor in der `initialize()` -Methode gesetzt worden ist. Der Text in der zweiten Spalte wird als Link ausgegeben. Wenn der Nutzer auf diesen Link klickt, wird der Sticker mit der Bearbeiten -Funktion geöffnet. Für die Löschfunktion in der dritten Spalte wird ein `<glyph>` -Element für das Icon ausgegeben (nur die erste Spalte (das `<listitem>` -Element) unterstützt das image-Attribut). Das <glyph>-Element wird mit einem Link-Element ( `<a
  href=""></a>`.md) umgeben, das mit der Löschen -Funktion verknüpft ist.

**Beispielausgabe der Methode getStickersList()**

~~~~ {.xml}
<listview title="Stickers">
  <items>
    <listitem image="module:fadd3cc21cdfd5dba2492f6d7cd059b1/pics/stickers.png"
              title="#1"
              href="module_edmodule_stickers.php?st[cmd]=edit_sticker&st[sticker_id]=1">
      <subitem>
        <a href="module_edmodule_stickers.php?st[cmd]=edit_sticker&st[sticker_id]=1">
          It is by the fortune of God that, in this country, we have three benefits:...
        </a>
      </subitem>
      <subitem>
        <a href="module_edmodule_stickers.php?st[cmd]=del_sticker&st[sticker_id]=1">
          <glyph src="actions/page-delete.png" />
        </a>
      </subitem>
    </listitem>
  </items>
</listview>
~~~~

Die Methode execute()

Im Programmablauf wird als zweites die Methode `execute()` aufgerufen. In dieser Methode wird zunächst überprüft, ob ein Befehl übermittelt wurde. Anschließend entscheidet die Methode anhand des Befehlsparameters `$this->params['cmd']`, welche Funktion ausgeführt wird:

**Grundaufbau der execute()-Methode**

~~~~ {.php}
function execute() {
  if (!empty($this->params['cmd'])) {
    switch ($this->params['cmd']) {
    case 'add_collection':
      // ...
      break;
    case 'add_sticker':
      // ...
      break;
    case 'edit_collection':
      // ...
      break;
    case 'edit_sticker':
      // ...
      break;
    case 'del_collection':
      // ...
      break;
    case 'del_sticker':
      // ...
      break;
    }
  }
}
~~~~

Der Befehlsparameter wird in einer Switch-Case-Konstruktion ausgewertet. Je nach Befehl werden entsprechende Funktionen ausgeführt. Im Prinzip kann man zwei verschiedene Arten von Befehlen unterscheiden:

1.  Ein Befehl zum Anlegen oder Bearbeiten eines Datensatzes führt dazu, das eine Eingabemaske dargestellt wird. Der Nutzer kann Daten eingeben oder bestehende Daten bearbeiten.
2.  Ein Befehl zum Löschen eines Datensatzes führt dazu, dass ein Dialog mit einer Sicherheitsabfrage dargestellt wird. Der Nutzer muss bestätigen, dass der Datensatz gelöscht werden soll.

Wenn Sie also den Befehl zum Darstellen einer Eingabemaske implementieren wollen, gehen Sie wie folgt vor:

1.  Initialisieren Sie das entsprechende Formular.
2.  Prüfen Sie, ob Formulardaten übermittelt worden sind.
3.  Führen Sie die Methode aus, mit der die Validität der Formulardaten überprüft werden.
4.  Wenn die Daten gültig sind, schreiben Sie die Daten mit der Datenbankmethode aus der base-Klasse in die Datenbank.
5.  Wenn der Datensatz angelegt werden konnte oder die Änderung erfolgreich war, geben Sie eine entsprechende Infomeldung aus.

Der komplette Abschnitt für den Befehl `add_collection` ist wie folgt implementiert:

**Sammlung hinzufügen in execute()-Methode**

~~~~ {.php}
...
case 'add_collection':
  $this->initializeCollectionDialog($this->params['cmd']);
  if (isset($this->params['submit']) && $this->params['submit']) {
    if ($this->dialog->checkDialogInput()) {
      if ($collectionId = $this->addCollection($this->params['collection_title'],
          $this->params['collection_description'])) {
        $this->addMsg(MSG_INFO, sprintf($this->_gt('Collection "%s" (#%d) added.'),
          $this->params['collection_title'], $collectionId);
          $this->initializeCollectionDialog($this->params['cmd'], FALSE);
      }
    }
  }
  break;
...
~~~~

Zunächst wird die Methode `initializeCollectionDialog()` ausgeführt, um das Eingabeformular für die Sammlung auszugeben. Anschließend wird überprüft, ob das Formular abgesendet worden ist. Dies ist der Fall, wenn im Parameter-Array der Wert „submit“ enthalten ist und dieser Parameter den Wert TRUE besitzt. Wenn dies der Fall ist, können Sie die versendeten Formulardaten auf Validität prüfen. Dazu wird die Methode `checkDialogInput()` der `base_dialog` -Instanz `$this->dialog` ausgeführt. Da das aktuelle Eingabeformular immer über `$this->dialog` erreichbar ist, können Sie die `checkDialogInput()` -Methode einfach über diese Objektinstanz aufrufen.

Wenn `checkDialogInput()` wahr ist, sind die Daten gültig und können in die Datenbank geschrieben werden. Dazu wird im obigen Listing die Methode `addCollection()` benutzt, die in der Basisklasse `base_stickers` implementiert worden ist. Dieser Methode wird der Titel sowie der Beschreibungstext der Sammlung übergeben. Wenn die Daten geschrieben werden konnten, gibt die Methode die ID der neu angelegten Sammlung zurück. Im letzten Schritt wird ein Infodialog dargestellt, der den Titel der Sammlung inklusive der ID in einem Infotext ausgibt. Der Infodialog wird mit der Methode `addMsg()` ausgegeben. Nähere Informationen zur Verwendung von `addMsg()` finden Sie in [:Kategorie:Meldungen mit base_object::logMsg() protokollieren](/:export_de/Kategorie:Meldungen_mit_base_object::logMsg()_protokollieren.md).

Alle notwendigen Daten wie der submit-Parameter oder die Formulardaten kommen aus dem Eingabeformular, das in der Methode `initializeCollectionDialog()` erzeugt wird. Im folgenden soll die Implementation dieser Methode vorgestellt werden.

Die Methode initializeCollectionDialog()

Mit der Klasse `base_dialog` können Sie mit wenig Aufwand ein umfangreiches Formular gestalten. Weitere Informationen zur Verwendung finden Sie in der Dokumentation zur Klasse `base_dialog`. Der Konstruktor von `base_dialog ` erwartet ein Elternobjekt, den Parameternamen, eine Felddefinition (siehe [Eingabemasken für Inhaltsmodule definieren](/Eingabemasken_für_Inhaltsmodule_definieren.md).md), Standarddaten für das Formular und versteckte Parameter.

Der Vorgang einen bestehenden Datensatz zu ändern unterscheidet sich im Prinzip nicht von dem einen neuen Datensatz hinzuzufügen. Es müssen lediglich andere Methoden verwendet werden und die ID des zu ändernden Datensatzes mit übergeben werden.

Die Werte, die bearbeitet werden, sind identisch. Die Methode `initializeCollectionDialog()` können Sie deshalb mit geringem Aufwand so implementieren, dass sie beide Fälle behandeln kann: Hinzufügen und Bearbeiten.

**Implementation der Methode initializeCollectionDialog()**

~~~~ {.php}
  function initializeCollectionDialog($cmd, $loadParams = TRUE) {
    include_once(PAPAYA_INCLUDE_PATH.'system/base_dialog.php');
    $hidden = array(
      'cmd'    => $cmd,
      'submit' => 1,
   .md);
    if ($cmd == 'edit_collection' && $this->params['col_id'] > 0) {
      $title = $this->_gt('Edit collection');
      $hidden['col_id'] = $this->params['col_id'];
      $data = $this->collections[$this->params['col_id']];
      $buttonTitle = 'Save';
    } else {
      $title = $this->_gt('Add a collection');
      $buttonTitle = 'Add';
      $data = array();
    }
    $fields = array(
      'collection_title'       => array('Title', 'isNoHTML', TRUE, 'input', 200),
      'collection_description' => array('Description', 'isNoHTML', FALSE, 'textarea', 5),
   .md);
    $this->dialog = &new base_dialog($this, $this->paramName, $fields, $data, $hidden);
    if ($loadParams) {
      $this->dialog->loadParams();
    }
    $this->dialog->dialogTitle = $title;
    $this->dialog->buttonTitle = $buttonTitle;
    $this->dialog->inputFieldSize = 'large';
  }
~~~~

Sie erstellen ein Array `$hidden` für die versteckten Parameter. In diesem Array fügen Sie den Befehl ein, der ausgeführt werden soll (cmd = `$cmd`.md), sowie die Information, dass das Formular übermittelt wurde (submit = 1). Der Befehl wird dabei aus dem Parameter `$cmd` ausgelesen. Dadurch können Sie den selben Befehl für den Aufruf des Formulars über einen Link als auch für das Versenden des Formulars verwenden und dennoch unterscheiden, welcher Fall vorliegt.

Im nächsten Schritt wird in der If-Abfrage getestet, ob ein bestehender Datensatz bearbeitet werden soll. In diesem Fall hat der Parameter `$cmd` den Wert `edit_collection`. Entsprechend diesem Befehl wird der Titel des Dialogs und des Button-Titels gesetzt. Außerdem wird das `$data` -Array mit den bereits geladenen Daten der aktuell ausgewählten Sammlung gefüllt. Das `$hidden` -Array mit den versteckten Parametern wird noch um die ID der aktuell ausgewählten Sammlung erweitert.

Wenn ein neuer Datensatz erstellt werden soll, lassen Sie das `$data` -Array zunächst leer. Der Grund hierfür ist, dass das Formular in diesem Fall nicht mit bestehenden Daten vorbelegt werden soll. Der Konstruktor von base_dialog erwartet für den Parameter `$data
  ` eine Referenz. Daher ist es nicht möglich, einfach ein leeres Array oder NULL zu übergeben. Das Array `$data` muss also stets initialisiert werden.

Die Dialogfelder selbst werden im Array `$fields` angegeben. Für den Titel wird dabei ein einzeiliges Textfeld, für die Beschreibung ein mehrzeiliges Textfeld definiert. Die Dialogfelder werden nach dem selben Schema angelegt wie die Edit-Fields bei Content-Modulen. Näheres zu den Dialogfeldern erfahren Sie in [Eingabemasken für Inhaltsmodule definieren](/Eingabemasken_für_Inhaltsmodule_definieren.md).

Im letzten Schritt wird eine Instanz der Klasse `base_dialog` im Klassenattribut `$this->dialog` angelegt. Übergeben Sie dem Konstruktor von `base_dialog` die Instanz der Klasse `admin_stickers` ( `$this`.md), den Parameternamen sowie die Arrays mit den Formularfeldern, den Formulardaten und den versteckten Parametern. Dadurch kann auch aus der `execute()` -Methode auf die Instanz zugegriffen werden.

Laden Sie die bereits übermittelten Formulardaten in das Formular, wenn der Parameter `$loadParams``TRUE` ist. Dadurch können Fehleingaben bearbeitet werden ohne das ganze Formular erneut ausfüllen zu müssen. Wenn das Formular erfolgreich abgesendet wird, setzen Sie es zurück, indem Sie der Methode FALSE übergeben.

Analog zu `add_collection` wird die Funktion für den Befehl `add_sticker` implementiert.

Daten bearbeiten

Um Daten zu bearbeiten, wird ebenso wie beim Hinzufügen von Daten das Eingabeformular initialisiert. Anschließend wird überprüft, ob das Formular abgesendet worden ist. Dies ist der Fall, wenn das Array `$params` den Parameter „ `submit` “ enthält. Der Wert des Parameters ist zudem „1“ oder `TRUE`. Wenn dies der Fall ist, werden die übermittelten Daten mit der `checkDialogInput()` -Methode überprüft. War die Überprüfung erfolgreich, wird der Datensatz mit der Datenbankmethode `updateCollection()` aus der Elternklasse `base_stickers` aktualisiert. Wenn die Daten erfolgreich geändert werden konnten, wird ein Infodialog mit einer entsprechenden Erfolgsmeldung ausgegeben:

**Sammlung bearbeiten (execute()-Methode)**

~~~~ {.php}
...
case 'edit_collection':
  $this->initializeCollectionDialog($this->params['cmd']);
  if (isset($this->params['submit']) && $this->params['submit']) {
    if ($this->dialog->checkDialogInput()) {
      if ($this->updateCollection($this->params['col_id'],
          $this->params['collection_title'], $this->params['collection_description'])) {
        $this->addMsg(MSG_INFO, $this->_gt('Collection updated.'));
        unset($this->dialog);
      }
    }
  }
  break;
...
~~~~

Daten löschen

Natürlich sollten Sie eine Funktion implementieren, um bestehende Einträge zu löschen. In papaya CMS werden Vorgänge, bei denen Datensätze umfassend geändert oder gelöscht werden, grundsätzlich mit einer Bestätigungsmeldung versehen. Der Benutzer muss den Vorgang also explizit bestätigen, damit Daten nicht bereits durch einen Buttonklick gelöscht werden. Auf diese Weise wird der versehentliche Datenverlust verhindert.

Der Abschnitt `del_collection` in der Methode `execute()` ist wie folgt implementiert:

**Sammlung löschen (execute())**

~~~~ {.php}
case 'del_collection':
  if (isset($this->params['confirm']) && $this->params['confirm']) {
    if ($this->deleteCollection($this->params['col_id'])) {
      $this->addMsg(MSG_INFO, sprintf($this->_gt('Collection %d deleted.'),
        $this->params['col_id']));
    }
  } else {
    $this->layout->addRight($this->getDeleteCollectionConfirmDialog());
  }
  break;
~~~~

Zunächst wird überprüft, ob der Parameter `confirm` gesetzt und wahr ist. Dies ist immer dann der Fall, wenn der Fragedialog vom Nutzer bestätigt worden ist. Bei einer Bestätigung wird der Datensatz gelöscht. Ist der Löschvorgang erfolgreich, wird dies dem Benutzer mit `addMsg()` mitgeteilt. Ist der Parameter nicht gesetzt, fügen Sie den Bestätigungdialog mit `addRight()` in die rechten Spalte des Content-Bereichs hinzu.

Der Löschvorgang bei Stickern wird analog implementiert. Datensätze werden also nur gelöscht, wenn der Nutzer dies explizit bestätigt hat.

Die Methode getDeleteCollectionConfirmDialog()

Verwenden Sie für Bestätigungsdialoge die Klasse `base_msgdialog`. Sie funktioniert ähnlich wie `base_dialog`, nur noch einfacher:

**Implementation von getDeleteCollectionConfirmDialog()**

~~~~ {.php}
function getDeleteCollectionConfirmDialog() {
  include_once(PAPAYA_INCLUDE_PATH.'system/base_msgdialog.php');
  $hidden = array(
    'cmd'     => $this->params['cmd'],
    'col_id'  => $this->params['col_id'],
    'confirm' => 1,
 .md);

  $msg = sprintf(
    $this->_gt('Do you really want to delete collection "%s" #%d?'),
    $this->collections[$this->params['col_id']['collection_title'],
    $this->params['col_id']
 .md);
  $this->dialog = &new base_msgdialog(
    $this,
    $this->paramName,
    $hidden,
    $msg,
    'warning'
 .md);
  $this->dialog->buttonTitle = 'Delete';
  return $this->dialog->getMsgDialog();
}
~~~~

Um einen Dialog zu erzeugen, gehen Sie wie folgt vor:

1.  Binden Sie die Klasse `base_msgdialog` ein.
2.  Erstellen Sie ein `$hidden` -Array, das die versteckt zu übermittelnden Parameter enthält. Üblicherweise sind das die durchzuführende Aktion (Parameter `cmd`.md), der Datensatz, auf den sich die Aktion bezieht (in diesem Fall der Parameter `col_id`.md), sowie die Bestätigung, dass der Benachrichtigungsdialog übermittelt wurde ( `confirm`.md).
3.  Erstellen Sie den Text der Bestätigungsfrage. Der Text muss den Nutzer darauf hinweisen, dass er durch einen Klick auf den Button einen Datensatz löscht.
4.  Im letzten Schritt instanziieren Sie den Dialog. Der Konstruktor erhält als fünften Parameter den Dialogtyp, siehe Tabelle "Mögliche Typen für den Messagedialog" in [Administrationsmodul schreiben](/Administrationsmodul_schreiben.md).
5.  Setzen Sie als Button-Titel (Klassenattribut `$this->dialog->buttonTitle`.md) den Namen der Funktion ein, die der Nutzer bestätigen muss. Beim Löschen von Datensätzen wäre dies beispielsweise die Beschriftung Löschen .
6.  Die XML-Ausgabe des Dialogs wird schließlich mit `$this->dialog->getMsgDialog()` ausgegeben.

Die folgende Tabelle schlüsselt alle Dialogtypen auf, die Sie für die Klasse `base_msgdialog` benutzen können:

|Dialogtyp|Bedeutung|
|question|Einfache Bestätigungsabfrage|
|warning|Bestätigungsabfrage erhöhter Wichtigkeit|
|info|Bestätigung, dass der Benutzer die Information gelesen hat.|
|error|Bestätigung, dass der Benutzer die Fehlermeldung gelesen hat.|

[Kategorie:Eigene Anwendungen schreiben](export_de/Kategorie:Eigene_Anwendungen_schreiben.md)