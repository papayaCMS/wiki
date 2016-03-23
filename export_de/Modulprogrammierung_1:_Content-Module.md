
-   **Zusammenfassung**: Dieses Tutorial beschreibt, wie man ein einfaches Modul oder Plugin für [papaya CMS](Papaya_CMS.md) schreibt.
-   **Zielgruppe**: PHP-Entwickler
-   **Schwierigkeitsgrad**: Fortgeschrittene

Loslegen

Bevor Sie mit dem Schreiben von Modulen anfangen können, müssen Sie papaya CMS korrekt installieren und vorkonfigurieren, was detailliert in der [Dokumentation](http://www.papaya-cms.com/doku) beschrieben wird.

Das in diesem Tutorial entwickelte Modul ist ein einfaches Seitenmodul, das die berühmte Nachricht "Hello World" ausgibt. Es benötigt keinen Datenbankzugriff und besteht nur aus einer Datei. Im weiteren Verlauf wird das Modul etwas ausgebaut; neben "Hello World" wird ein vom Redakteur pflegbarer Text ausgegeben.

### Terminologie: Modul und Modulpaket

Wenn Sie das Unterverzeichnis *modules* im Verzeichnis *papaya-lib* durchsuchen, werden Sie verschiedene Unterverzeichnisse finden, die im nächsten Unterabschnitt genauer erläutert werden. Jedes dieser Unterverzeichnisse enthält entweder Dateien oder weitere Unterverzeichnisse. Ein Verzeichnis wie *modules/_base/community* wird als **Modulpaket** bezeichnet: Die meisten der darin befindlichen Dateien teilen sich dieselben Datenbanktabellen und arbeiten zusammen, um einen bestimmten Dienst bereitzustellen; das Community-Paket verarbeitet beispielsweise Frontend-Logins, Benutzerprofile, Kontakte und so weiter.

Eine einzelne Datei in einem Paket wird **Modul** genannt. Es stellt einen Aspekt der Aufgaben des Pakets zur Verfügung. Das Community-Paket enthält beispielsweise eine Datei namens *content_login.php*, eine Standard-Loginseite, oder *box_login.php*, eine Login-Box, die in andere Seiten integriert werden kann.

Sie können alle verfügbaren Pakete und ihre Module betrachten, indem Sie in der Hauptsymbolleiste des papaya-CMS-Backends auf *Module* klicken. Ein Kasten auf der linken Seite zeigt die Liste aller Pakete an. Sobald Sie eines von ihnen anklicken, wird in der mittleren Spalte eine Liste der Module (und Datenbanktabellen) in diesem Paket angezeigt. Klicken Sie ein Modul an, dann erscheinen rechts Informationen über das Modul, und Sie können einige Einstellungen dafür vornehmen.

### Das passende Verzeichnis auswählen

Wenn Sie sich im Verzeichnis *papaya-lib/modules* Ihrer papaya-Installation umschauen, werden Sie *mindestens* folgende Unterverzeichnisse entdecken:

-   *_base* -- Basismodule wie eine einfache HTML-Box oder sämtliche Community-Module
-   *free* -- freie Module unter der GNU GPL
-   *gpl* -- freie Module, die auf GPL-Software von Drittanbietern basieren
-   *sample* -- ein einfaches Beispielmodul; es ähnelt demjenigen, das wir in diesem Tutorial entwickeln.

Falls Sie ein oder mehrere kommerzielle papaya-Module kaufen, werden diese in einem weiteren Unterverzeichnis namens *commercial* gespeichert.

Wenn Sie ein Modul schreiben, das nur für den Bedarf Ihres spezifischen Projekts geeignet ist, erstellen Sie ein neues Unterverzeichnis namens *special* und darin wiederum ein Unterverzeichnis, dessen Name Ihrem Projekt entspricht. Module für einen weniger spezifischen Anwendungszweck sollten im Verzeichnis *free* abgelegt werden (oder in einem *beta*-Verzeichnis, solange sie in Entwicklung sind), und Sie könnten eines Tages beschließen, diese Module für die papaya-Community freizugeben.

Nachdem Sie das Verzeichnis ausgewählt oder erstellt haben, in dem Ihr Modul gespeichert werden soll, erstellen Sie darin ein Unterverzeichnis namens *tutorial*.

### Die Datei *modules.xml* vorbereiten

Jedes Modulpaket benötigt eine Modulinformationsdatei namens *modules.xml*. Diese Datei listet die Module und Datenbanktabellen innerhalb des Pakets auf, und indem papaya CMS die Modulverzeichnisse rekursiv nach diesen Dateien durchsucht, werden dem System neue, geänderte oder gelöschte Module bekannt gemacht.

Hier ein kurzes beispiel für eine *modules.xml*-Datei aus dem Modulpaket *_base/countries*:

~~~~ {.xml}
<?xml version="1.0"  encoding="ISO-8859-1" ?>
<modulegroup>
  <name>Countries</name>
  <description>
    The country package provides backend functionality to manage continents and
    countries as well as a connector with form callback functions.
  </description>
  <modules>
    <module type="page"
            guid="fd53aef2d8bb7cb4637a64dabaf7b424"
            name="State List XML"
            class="content_statelist"
            file="content_statelist.php">
      Returns an XML list of states for a specific country, to be used for Ajax
    </module>
    <module type="admin"
            guid="bf6e40b71d3cfb0e80682c64b11d33af"
            name="Countries"
            class="edmodule_countries"
            file="edmodule_countries.php"
            glyph="countries.png">
      The administration module provides the facility to manage continents,
      countries, and their localized names.
    </module>
    <module type="connector"
            guid="99db2c2898403880e1ddeeebf7ee726c"
            name="Country Connector"
            class="connector_countries"
            file="connector_countries.php">
      Country Connector
    </module>
  </modules>
  <tables>
    <table name="continents"/>
    <table name="countries"/>
    <table name="countrynames"/>
    <table name="states"/>
    <table name="countries_old"/>
    <table name="states_old"/>
  </tables>
</modulegroup>
~~~~

Das Wurzelelement *modulegroup* umfasst den gesamten Inhalt. Die Elemente *name* und *description* enthalten nur einfachen Text -- es handelt sich um den Namen beziehungsweise die Kurzbeschreibung des Pakets. Der Abschnitt *modules* enthält für jedes Modul des Pakets ein *module*-Element, während *tables* für jede Datenbanktabelle einen *table*-Eintrag besitzt.

Ein *module*-Element besteht aus fünf oder mehr Attributen und einer umschlossenen Beschreibung in Form von einfachem Text. Die Attribute werden wie folgt definiert:

-   *type* -- der Typ des Moduls, zum Beispiel "page" für ein Seitenmodul oder "box" für ein Boxmodul
-   *guid* -- eine eindeutige Identifikationsnummer für das Modul: ein String, der eine hexadezimale, 128 Bit lange Zahl enthält
-   *name* -- der Modulname, wie er im papaya-Backend angezeigt werden soll
-   *class* -- der eigentliche Name der PHP-Klasse, die das Modul definiert
-   *file* -- die Datei, in der sich das Modul befindet (optional können Sie für Dateien in Unterverzeichnissen relative Pfade angeben)
-   *glyph* -- der Name einer Icon-Datei für ein Modul (nur für Module vom Typ *admin* empfohlen, da diese verwendet werden, um das gesamte Paket zu konfigurieren)
-   *outputfilter* -- ein optionales Attribut für Content-Module (die Typen *page* oder *box*): Wenn Sie dieses Attribut auf den Wert "no" setzen, wird kein Ausgabefilter verwendet, um ihre finale Ausgabe zu erzeugen (das Konzept des Ausgabefilters wird detaillierter in \#\#papaya-Architektur\#\# erläutert)

Die *table*-Elemente im Abschnitt *tables* enthalten lediglich *name*-Attribute. Die Tabellenstrukturen selbst werden im Unterverzeichnis *DATA* eines jeden Modulpaket-Verzeichnisses abgelegt. Diese Dateien sollten *niemals* von Hand geschrieben werden; im nächsten Tutorial erfahren Sie, wie Sie sie aus dem papaya-Backend erzeugen können.

Wir benötigen nun eine neue *modules.xml*-Datei für das gewünschte Paket; sie wird nur ein einzelnes Modul und keine Datenbanktabellen enthalten. Öffnen Sie Ihren bevorzugten Text- oder XML-Editor und tippen (oder kopieren) Sie Folgendes:

~~~~ {.xml}
<?xml version="1.0" encoding="utf-8" ?>
<modulegroup>
  <name>Hello World tutorial module</name>
  <description>A tutorial to learn papaya CMS module development.</description>
  <modules>
    <module type="page"
            guid=""
            name="Hello World Page"
            class="HelloPage"
            file="Hello/Page.php">
      This simple page module displays a Hello World message
    </module>
  </modules>
</modulegroup>
~~~~

Bitte beachten Sie, dass wir das Attribut *guid* zunächst leer gelassen haben. Aber wir brauchen natürlich einen Wert, weil das Modul anhand dieser Identifikationsnummer registriert wird. Sie können entweder den [GUID-Generator](http://community.papaya-cms.com/guid) verwenden oder aber einfachen PHP-Code (die meisten anderen Sprachen funktionieren auch) wie diesen hier schreiben:

~~~~ {.php}
<?php

echo md5(rand());

?>
~~~~

Fügen Sie nun Ihren neuen Hashwert zwischen den Anführungszeichen des Attributs *guid* ein und speichern Sie die Datei.

Das Modul schreiben

Beii den empfohlenen Verzeichnis- und Dateinamensstrukturen für papaya-Module besteht ein Unterschied zwischen älteren, PHP-4-kompatiblen Entwicklungen und neuen Nur-PHP-5-Paketen. Das liegt daran, dass für neue Module *Unit Tests* verwendet werden sollten. Wenn Sie noch nie etwas von Unit Tests im Allgemeinen und PHPUnit (dem Standard-Test-Framework für PHP) gehört haben: keine Sorge; alles Nötige wird im Lauf des Tutorials erläutert. Eine der besten Ressourcen, um mit Unit Tests anzufangen, ist der Artikel [Test Infected: Programmers Love Writing Tests](http://junit.sourceforge.net/doc/testinfected/testing.htm) von *Erich Gamma* und *Kent Beck*. Detaillierte Informationen über PHPUnit erhalten Sie dagegen auf der offiziellen [PHPUnit-Site](http://www.phpunit.de/).

Für dieses Projekt verwenden wir sogar den *Test-First*-Ansatz: Schreiben Sie einen Unit Test für jeden Teil Ihres Codes, bevor Sie den eigentlichen Code schreiben.

Bevor wir beginnen, den eigentlichen Code zu schreiben, erstellen wir die grundlegende Verzeichnisstruktur.

Erstellen Sie die folgenden Verzeichnisse im weiter oben ausgewählten Bereich:

`+ tutorial [wurd bereits erstellt und enthält die Datei modules.xml]`
`|`
`+--+ Hello`
`   |`
`   +--+ Page`

Suchen Sie jetzt das Verzeichnis *testing/test-unittests* Ihrer papaya-Installation. Es sollte bereits den Unterpfad *papaya-lib/modules* enthalten (falls nicht, erzeugen Sie ihn einfach). Erstellen Sie als Nächstes dieselbe verschachtelte Struktur *tutorial/Hello/Page* in denjenigen Unterverzeichnissen der Testumgebung, die dem Ort dieser Struktur in der Modulverzeichnishierarchie entspricht.

### Top-down und testgetrieben: ein statisches Content-Modul

Testgetriebener, objektorientierter Code sollte nach einem Top-down-Ansatz geschrieben werden: Sie beginnen mit einer statischen Implementierung dessen, was der Benutzer auf dem Bildschirm zu sehen bekommt, und implementieren erst danach die zugrundeliegende Logik. Dieser Ansatz garantiert, dass jeder Teil des Codes unabhängig ist und zu jeder Zeit flexibel durch eine andere Implementierung ersetzt werden kann.

Der erste Teil, der implementiert wird, ist das Seitenmodul selbst. Erstellen Sie im Verzeichnis *tutorial/Hello* eine leere PHP-Datei namens *Page.php* und im Verzeichnis *tutorial/Hello* des Unit-Test-Verzeichnisbaums eine weitere leere PHP-Datei namens *PageTest.php*.

Gemäß dem Test-first-Ansatz sollten Sie die Unit-Test-Klassendatei vorbereiten und den ersten Test schreiben, bevor Sie irgendwelchen Implementierungscode schreiben. Ein PHPUnit-Test-Case erweitert die Basisklasse *PHPUnit_Framework_TestCase*. Für die spezifischen Bedürfnisse von papaya-CMS-Unit-Tests gibt es jedoch bereits die Klasse *PapayaTestCase*, die Sie erweitern können; sie befindet sich um Unterverzeichnis *Framework* des Pfads *testing/tests-unittests*.

Der Rahmen der Klassendatei *PageTest.php* sieht folgendermaßen aus:

~~~~ {.php}
<?php

require_once(substr(__FILE__, 0, -52).'/Framework/PapayaTestCase.php');
require_once(PAPAYA_INCLUDE_PATH.'modules/beta/tutorial/Hello/Page.php');

class HelloPageTest extends PapayaTestCase {
}

?>
~~~~

Um den korrekten Importpfad für die Datei *PapayaTestCase.php* zu ermitteln, müssen Sie die Zeichen des Unterpfads Ihrer Testdatei unterhalb von *testing/tests-unittests* zählen. Im obigen Beispiel werden 52 Zeichen verwendet, weil wir daon ausgehen, dass der Unterpfad entweder */papaya-lib/modules/free/tutorial/Hello/PageTest.php* oder */papaya-lib/modules/beta/tutorial/Hello/PageTest.php* ist. Sie müssen dies anpassen, falls Sie eine andere Verzeichnisstruktur verwenden. Ähnliches gilt auch für die nächste Zeile, die die zu testende Klasse selbst importiert.

Um eine papaya-Content-Modulklasse zu testen, muss ein weiteres Problem gelöst werden: Der Konstruktor von *base_plugin*, dem gemeinsamen Vorfahren der Basisklassen sowohl für Seiten- als auch für Boxmodule (*base_content* beziehungsweise *base_actionbox*), erwartet eine Referenz auf das Eigentümer-Objekt. Da eine Testklasse kein passender Eigentümer für ein Objekt wie dieses ist, stellen wir eine sogenannte Proxy-Klasse bereit, die unsere Modulklasse erweitert und den Konstruktur durch einen ersetzt, der keine Eigentümerreferenz benötigt. Fügen Sie hinter der schließenden geschweiften Klammer der Klassendefinition von *HelloPageTest* einfach folgenden Code hinzu:

~~~~ {.php}
class HelloPageProxy extends HelloPage {
  function __construct() {
    // Nothing to do here, just override parent's constructor
    // to get rid of the mandatory parameter
  }
}
~~~~

Sie können in der Testklasse eine private Methode schreiben, um die zu testende Klasse (oder, in diesem Fall, die Proxy-Klasse) zu instantiieren. Fügen Sie dazu folgende Methode zur Klasse *HelloPageTest* hinzu:

~~~~ {.php}
/**
* Instantiate the HelloPage object to be tested
*
* @return HelloPage
*/
private function getHelloPageObjectFixture() {
  return new HelloPageProxy();
}
~~~~

Die Methode, die wir testen und anschließend implementieren möchten, heißt *getParsedData(.md)*. Es handelt sich um die einzige Pflichtmethode in einem papaya-Content-Modul, die verwendet wird, um wohlgeformtes XML zu erzeugen, das mit Hilfe von XSLT-Templates in das endgültige Ausgabeformat umgewandelt wird. Da wir einen Top-down-Ansatz gewählt haben, ist die erste Implementierung dieser Methode statisch (im Sinne von festgelegt; nicht *static* im Sinne einer Klassenmethode): Wir schreiben den Test so, dass er die Rückgabe von festgelegtem XML erwartet, und implementieren die Methode dann so, dass genau dieses XML zurückgegeben wird.

Fügen Sie die folgende Methode zur Testklasse hinzu:

~~~~ {.php}
/**
* @covers HelloPage::getParsedData
*/
public function testGetParsedData() {
  $helloPageObject = $this->getHelloPageObjectFixture();
  $xml = '<title>Hello world!</title>
<text>Greetings from the new module</text>';
  $this->assertEquals($xml, $helloPageObject->getParsedData());
}
~~~~

Die Namen der Testmethoden in Unit Tests müssen stets mit *test* beginnen, damit PHPUnit sie ausführt, und die Methoden müssen public sein. Die PHPDoc-Annotation *@covers* wird verwendet, um die *Code Coverage* Ihrer Unit Tests zu ermitteln -- einfach gesagt den Prozentsatz des Codes, der durch Tests abgedeckt ist. Mit Hilfe dieser Annotationen können Sie unter anderem verhindern, dass Ihre Tests andere Methoden abdecken, die von den getesteten Methoden implizit aufgerufen werden.

Der wichtgiste Bestandteil von Unit Tests sind die diversen *assert...(.md)*-Methoden, die verwendet werden können, um die Rückgabewerte der getesteten Methoden gegen beinahe beliebige Bedingungen zu prüfen. *assertEquals(.md)* testet auf Gleichheit und ist eine der am häufigsten verwendeten Methoden aus der Gruppe. Bitte beachten Sie, dass es üblich ist, die oben gezeigte Reihenfolge einzuhalten: der erwartete Wert ist das erste Argument und der eigentliche, zu testende Methodenaufruf das zweite.

Als Nächstes können Sie beginnen, Ihre *HelloPage*-Klasse zu schreiben und den Kopf der Methode *getParsedData(.md)* hinzufügen:

~~~~ {.php}
<?php

/**
* Hello World tutorial page module
*
* @package Papaya-Modules
* @subpackage tutorial
*/

/**
* Base class base_content
*/
require_once(PAPAYA_INCLUDE_PATH.'system/base_content.php');

/**
* Hello World tutorial page module class
*
* @package Papaya-Modules
* @subpackage tutorial
*/
class HelloPage extends base_content {

  /**
  * Get the page output XML
  *
  * @return string XML
  */
  public function getParsedData() {
  }

}

?>
~~~~

Genau wie in diesem Beispiel sollten Sie stets Gebrauch von PHPDoc machen, um Ihren Code zu dokumentieren. Details über PHPDoc finden Sie \#\#add_internal_tutorial_and/or_external_link\#\#.

Es mag ein wenig absurd klingen, aber Sie sollten jetzt Ihren Unit Test ausführen, bevor Sie den eigentlichen Code implementieren, obwohl Sie wissen, dass der Test scheitern wird. Die Arbeitsreihenfolge der testgetriebenen Entwicklung ist:

1.  Schreiben Sie einen Test, der fehlschlägt
2.  Implementieren Sie den Code, der notwendig ist, um den Test zu bestehen
3.  Führen Sie ein Code-Refactoring durch, damit die Implementierung dem Gesamtziel Ihres Projekts entspricht

Noch kürzer gesagt: "red, green, refactor" -- in den meisten grafischen Darstellungen von Unit Tests werden fehlgeschlagene Tests rot und bestandene grün dargestellt.

Um Ihren PHPUnit-Test von der Konsole aus auszuführen, geben Sie Folgendes ein:

`$ `**`phpunit` `[path/to/]HelloPageTest.php`**

Da der Test scheitert, sieht die Ausgabe so aus:

`PHPUnit 3.4.2 by Sebastian Bergmann.`
`F`
`Time: 0 seconds`
`There was 1 failure:`
`1) HelloPageTest::testGetParsedData`
`Failed asserting that two strings are equal.`
`+++ Actual`
`@@ @@`
`-`

<title>
Hello world!

</title>
`-`<text>`Greetings from the new module`</text>
`+`
`.../testing/tests-unittests/papaya-lib/modules/beta/tutorial/Hello/PageTest.php:23`
`FAILURES!`
`Tests: 1, Assertions: 1, Failures: 1.`

Wie Sie sehen, unterscheidet sich der erwartete Rückgabewert (der XML-Code) vom tatsächlichen Rückgabewert (gar nichts). Das heißt, dass es definitiv Zeit ist, eine statische Version von *getParsedData(.md)* zu implementieren, die den Test erfüllt:

~~~~ {.php}
  /**
  * Get the page output XML
  *
  * @return string XML
  */
  public function getParsedData() {
    $result = '<title>Hello world!</title>'.LF;
    $result .= '<text>Greetings from the new module</text>';
    return $result;
  }
~~~~

Führen Sie Ihren Test erneut aus; diesmal sollte er Erfolg melden (ein einzelner '.' für einen einzelnen bestandenen Test statt eines 'F' wie 'failed' für einen gescheiterten):

`PHPUnit 3.4.2 by Sebastian Bergmann.`
`.`
`Time: 0 seconds`
`OK (1 test, 1 assertion)`

... und das war's! Sie haben erfolgreich ein -- wenn auch statisches -- papaya-Seitenmodul erstellt. Wenn Sie mit SVN oder einem anderen Versionsverwaltungssystem arbeiten, ist es jetzt Zeit, Ihren Code in das Repository zu committen. Diese Arbeitsweise wird als *Continuous Integration* bezeichnet, ein grundlegender Bestandteil agiler Softwareentwicklungsmethoden: Committen Sie so oft, wie Sie können, wobei durch das Bestehen zuvor geschriebener Unit Tests sichergestellt ist, dass jederzeit "clean code that works" bereitgestellt wird.

### Zeit für einen Live-Test im Frontend

Sie sollten sich jedoch nicht allein auf Unit Tests verlassen, sondern Ihr Modul auch in Ihrem Browser testen. Da wir für das Ergebnis die XML-Elemente *title* und *text* gewählt haben, können wir das bestehende XSLT-Template für Standardseiten verwenden, so dass wir jetzt kein Template zu schreiben brauchen. Um Ihr Modul in papaya CMS zu testen, führen Sie folgende Schritte durch:

1.  Melden Sie sich im papaya-Backend als Administrator an.
2.  Wählen Sie in der Hauptsymbolleiste *Module* und klicken Sie dann auf *Module suchen*. Ihr Modul sollte nach einer gewissen Suchdauer, die durch einen Fortschrittsbalken dargestellt wird, hinzugefügt werden.
3.  Klicken Sie nun in der Hauptsymbolleiste auf *Ansichten*. Eine Ansicht ist die Verknüpfung zwischen einem Modul und einem Template, woraus sich eine bestimmte Art der Frontend-Ausgabe und -Funktionalität ergibt.
4.  Im Bereich *Ansichten* sollten Sie ein Formular mit den Feldern *Titel* und *Modul* sehen, mit dem eine neue Ansicht erstellt werden kann; falls nicht, klicken Sie die Schaltfläche *Ansichten* in der Untersymbolleiste an.
5.  Geben Sie als Modultitel *Hello World Page* ein, wählen Sie im Modulselektor *[page] Hello World Page* aus dem Bereich *Hello World tutorial module* aus und klicken Sie auf *Hinzufügen*.
6.  Rechts sehen Sie eine Leiste mit dem Titel *Ausgabefilter* (wenn nicht, haben Sie Ihr papaya CMS noch nicht konfiguriert). Kreuzen Sie das Kontrollkästchen *Verknüpft* neben der Erweiterung *html* an. Die Warnung *Keine XSLT-Datei festgelegt.* wird ausgegeben.
7.  Wählen Sie *page_main.xsl* aus dem XSL-Stylesheet-Auswahlfeld, ignorieren Sie die restlichen Einstellungen und klicken Sie auf *Speichern*. Die Meldung *Änderungen gespeichert.* und die Warnung *XSLT-Datei "page_main.xsl" unterstützt das Modul "HelloPage" möglicherweise nicht.* werden angezeigt; Letztere können Sie im Moment getrost ignorieren.
8.  Wählen Sie *Seiten* aus der Hauptsymbolleiste.
9.  Navigieren Sie mit Hilfe der Leiste am linken Bildschirmrand duch den Seitenbaum. Wenn Sie einen Platz für Ihre Seite gefunden haben, klicken Sie auf *Seite hinzufügen*, um eine Seite auf der Ebene der aktuell ausgewählten Seite hinzuzufügen, oder auf *Unterseite hinzufügen*, um eine Seite auf einer Ebene unterhalb der aktuellen zu erstellen.
10. Geben Sie auf dem Tab *Eigenschaften* der neuen Seite den Titel *Hello World Page* ein und klicken Sie auf *Speichern*.
11. Klicken Sie danach den Tab *Ansicht* an und wählen Sie die Ansicht *Hello World Page* aus dem Abschnitt *Hello World tutorial module* aus, indem Sie ihren Titel anklicken.
12. Sie brauchen keinen Inhalt für die neue Seite festzulegen, da dieser statisch ist; der Tab *Inhalt* meldet lediglich *Kein Einstellungsdialog für dieses Modul.* Deshalb können Sie gleich den Tab *Vorschau* aktivieren und die Frontend-Ausgabe Ihres neuen Moduls betrachten: *Hello world!* als Überschrift und *Greetings from the new module* als Seiteninhalt.
13. Der Bereich *Vorschau* ermöglicht es Ihnen, zwischen den Ansichtsmodi HTML und XML zu wechseln, so dass Sie sehen können, wie das Seiten-XML vom Standard-Template in HTML umgewandelt wird.

### Refactoring: ein dynamisches Modul erstellen

Our static module is ready, but now we want to make it dynamic: The text underneath the *Hello World* headline should be editable by a website editor. This requires some refactoring. The common approach for modern, test-driven papaya modules is to have a Base class underneath the frontend class handle the dynamic output and other logic. In this step, we are going to create this Base class and connect it to the frontend class. Unser statisches Modul ist fertig, aber nun wollen wir es dynamisch machen: Der Text unter der Überschrift *Hello World* soll durch einen Redakteur bearbeitet werden können. Dazu ist ein wenig Refactoring nötig. Der übliche Ansatz für moderne, testgetrieben entwickelte papaya-Module umfasst eine Base-Klasse unterhalb der Frontend-Klasse, die sich um die dynamische Ausgabe und andere Aspekte der Logik kümmert. In diesem Schritt erstellen wir die Base-Klasse und verbinden sie mit der Frontend-Klasse.

Um eine Frontend-Klasse konfigurierbar zu machen, fügen Sie einfach ein öffentliches Attribut namens *\$editFields* vom Typ Array hinzu. Wenn es viel zu editieren gibt, können Sie als Alternativ *\$editGroups* verwenden, ein verschachteltes Array, in dem Sätze von Eingabefeldern auf mehreren Seiten angezeigt werden. Bleiben wir vorerst bei den klassischen *\$editFields*, da unser Modul nur ein einziges Feld erhalten soll.

Fügen Sie folgenden Code zu Ihrer *HelloPage*-Klasse hinzu, direkt über dem Docblock und der Deklaration der Methode *getParsedData(.md)*:

~~~~ {.php}
/**
* Edit fields for page configuration
* @var array
*/
public $editFields = array(
  'text' => array(
    'Text',
    'isNoHTML',
    TRUE,
    'textarea',
    5,
    '',
    'Greetings from the new module'
 .md)
);
~~~~

Nachdem Sie diese Änderung gespeichert haben, können Sie sie sofort ausprobieren: Wählen Sie im Bereich *Seiten* des papaya-CMS-Backends den Tab *Inhalt* Ihrer neuen Seite; statt der Warnung von vorhin sehen Sie dort jetzt einen Textbereich, um den Seitentext zu bearbeiten, sowie eine *Speichern*-Schaltfläche.

Im Array *\$editFields* ist der Schlüssel für jedes Feld dessen interner name, während der Wert ein Array mit folgenden Elementen ist:

-   Die Beschriftung, die im Inhaltsformular der Seite erscheint
-   Check type (as defined in the *sys_checkit* class of papaya's *system* directory). 'isNoHTML' is used to check for arbitrary text that must not contain HTML markup. \# Überprüfungstyp (wie in der Klasse *sys_checkit* im papaya-Verzeichnis *system*
-   A boolean that determines whether data in this field is mandatory (*TRUE*) or not (*FALSE*)
-   The field type (e.g. 'input' for a single-lined text field, 'textarea', 'checkbox', etc.)
-   The field settings (depends on the field type; for 'input' it determines the maximum number of chars that can be entered, for 'textarea' it's the visible number of lines)
-   Optional tooltip that will be displayed as a mouseover text for a little lamp icon next to the caption, if present
-   Default content -- optional as well, but you should make sure that each field with a mandatory value of *TRUE* has got default content

You can also add simple strings without dedicated indexes to the *\$editFields* array. These will serve as section headings in your edit dialog.

Each field you define in the *\$editFields* array will later be present in an attribute called *\$data*, either with its default value or with the one set by the website editor.

The next step is to create the *Base.php* class file in the *Page* subdirectory and its unit test. The stub of the Base class looks as follows:

~~~~ {.php}
<?php

/**
* Hello World tutorial page module, base class
*
* @package Papaya-Modules
* @subpackage tutorial
*/

/**
* Hello World tutorial page module class, base class
*
* @package Papaya-Modules
* @subpackage tutorial
*/
class HelloPageBase {
}

?>
~~~~

And here's the basic content of the the unit test, *Page/BaseTest.php*:

~~~~ {.php}
<?php

require_once(substr(__FILE__, 0, -57).'/Framework/PapayaTestCase.php');
require_once(PAPAYA_INCLUDE_PATH.'modules/beta/tutorial/Hello/Page/Base.php');

class HelloPageBaseTest extends PapayaTestCase {
}

?>
~~~~

The Base class doesn't have a dedicated constructor, and it doesn't extend any other class, either, so the code to load the tested object is as straightforward as this:

~~~~ {.php}
  /**
  * Instantiate the HelloPageBase object to be tested
  *
  * @return HelloPageBase
  */
  private function getHelloPageBaseObjectFixture() {
    return new HelloPageBase();
  }
~~~~

The first method we test and implement is called *setPageData(.md)*. It is used to pass the configuration data from the page class's edit fields to the Base class, and to pass other data from the unit test. This is the test for the intended method:

~~~~ {.php}
  /**
  * @covers HelloPageBase::setPageData
  */
  public function testSetPageData() {
    $helloPageBaseObject = $this->getHelloPageBaseObjectFixture();
    $data = array('text' => 'Hello');
    $helloPageBaseObject->setPageData($data);
    $this->assertAttributeEquals($data, '_data', $helloPageBaseObject);
  }
~~~~

As the configuration data will be stored in a private attribute called *\$_data*, the assertion has to be done using the *assertAttributeEquals(.md)* method. This method takes in three parameters: The value you want to compare the attribute to, the attribute name as a string without leading \$ sign, and the object whose attribute you want to read.

Now add the following code to the Base class, right underneath the opening curly brace of the class definition:

~~~~ {.php}
   /**
   * Page configuration data
   * @var array
   */
   private $_data = array();

   /**
   * Set page configuration data
   *
   * @param array $data
   */
   public function setPageData($data) {
   }
~~~~

Run the unit test, watch it fail, and then add the actual implementation of the *setPageData(.md)* method to a line bewteen its curly braces:

~~~~ {.php}
    $this->_data = $data;
~~~~

Now the test should work, so you can commit your code once again. Using the same test-failure-implementation workflow, you can now implement a method called *getPageXML(.md)* which will be called by the page module's *getParsedData(.md)* method to create the page's dynamic output. Here's the test:

~~~~ {.php}
  /**
  * @covers HelloPageBase::getPageXML()
  */
  public function testGetPageXML() {
    $helloPageBaseObject = $this->getHelloPageBaseObjectFixture();
    $helloPageBaseObject->setPageData(array('text' => 'Hello'));
    $xml = '<title>Hello world!</title>
<text>Hello</text>';
    $this->assertEquals($xml, $helloPageBaseObject->getPageXML());
  }
~~~~

And this is the method implementation itself (but don't forget to run the test without the actual code first):

~~~~ {.php}
  /**
  * Get the page's XML output
  *
  * @return string XML
  */
  public function getPageXML() {
    $result = '<title>Hello world!</title>'.LF;
    $result .= sprintf('<text>%s</text>', papaya_strings::escapeHTMLChars($this->_data['text']));
    return $result;
  }
~~~~

The implementation is pretty straightforward. Please note the static *papaya_strings::escapeHTMLChars(.md)* method call, though. It makes sure that HTML charactars are escaped in strings that are supposed to be plain text. It should be used whereever plain text user input or configuration data are returned for output in both frontend and backend. *LF* is a system-wide papaya CMS constant that creates a line break.

Now our Base class is ready, and we can refactor the content class to make use of it. First, we implement a method called *setBaseObject(.md)* to set the instance of the Base class to be used. This is called *dependency injection*. It can be used both for unit testing (to replace the real object by a so-called mock object that behaves just like we want it to) and to simply change implementation details later by inserting another object.

As usual, we write the test first (note that we switch back to the *HelloPageTest* class now). First, add one more *require_once(.md)* statement underneath the others:

~~~~ {.php}
require_once(PAPAYA_INCLUDE_PATH.'modules/beta/tutorial/Hello/Page/Base.php');
~~~~

We won't use a real instance of this class, but if the definition is present, the mock object we are going to use instead of it will automatically be modeled matching the real class, with all public attributes and methods. Now for the test method:

~~~~ {.php}
  /**
  * @covers HelloPage::setBaseObject
  */
  public function testSetBaseObject() {
    $helloPageObject = $this->getHelloPageObjectFixture();
    $helloPageBaseObject = $this->getMock('HelloPageBase');
    $helloPageObject->setBaseObject($helloPageBaseObject);
    $this->assertAttributeSame($helloPageBaseObject, '_baseObject', $helloPageObject);
  }
~~~~

Please note that we use *assertAttributeSame(.md)* instead of *assertAttributeEquals(.md)* this time. The parameter syntax is the same, but as we're dealing with an object reference rather than a plain value, it's appropriate to test for strict object identity.

Before you implement the method, add the following attribute declaration to the start of the class body:

~~~~ {.php}
  /**
  * Instance of the HelloPageBase class
  * @var HelloPageBase
  */
  private $_baseObject = NULL;
~~~~

And here's the method's implementation:

~~~~ {.php}
  /**
  * Set the HelloPageBase object to be used
  *
  * @param HelloPageBase $baseObject
  */
  public function setBaseObject($baseObject) {
    $this->_baseObject = $baseObject;
  }
~~~~

The counterpart of the *setBaseObject(.md)* method is *getBaseObject(.md)* which instantiates the *HelloPageBase* class only if necessary, i.e. if the object has not been set using *setBaseObject(.md)* before. This is another important technique called *lazy initialization*. Along with dependency injection, it's one of the key patterns of test-driven, object-oriented software design.

Write the following test for the *getBaseObject(.md)* method:

~~~~ {.php}
  /**
  * @covers HelloPage::getBaseObject
  */
  public function testGetBaseObject() {
    $helloPageObject = $this->getHelloPageObjectFixture();
    $baseObject = $helloPageObject->getBaseObject();
    $this->assertTrue($baseObject instanceof HelloPageBase);
  }
~~~~

The implementation of the method looks as follows:

~~~~ {.php}
  /**
  * Get (and, if necessary, initialize) the HelloPageBase object
  *
  * @return HelloPageBase
  */
  public function getBaseObject() {
    if (!is_object($this->_baseObject)) {
      include_once(dirname(__FILE__).'/Page/Base.php');
      $this->_baseObject = new HelloPageBase();
    }
    return $this->_baseObject;
  }
~~~~

As everything is up and running now, we only need to reimplement the *getParsedData(.md)* method to pass the configuration data to the Base object and return whatever the Base object's *getPageXML(.md)* method returns. Here's the revised test:

~~~~ {.php}
  /**
  * @covers HelloPage::getParsedData
  */
  public function testGetParsedData() {
    $helloPageObject = $this->getHelloPageObjectFixture();
    $helloPageBaseObject = $this->getMock('HelloPageBase');
    $xml = '<title>Hello world!</title>
<text>Greetings from the new module</text>';
    $helloPageBaseObject
      ->expects($this->once())
      ->method('getPageXML')
      ->will($this->returnValue($xml));
    $helloPageObject->setBaseObject($helloPageBaseObject);
    $this->assertEquals($xml, $helloPageObject->getParsedData());
  }
~~~~

Here's one of the virtues of dependency injection in action: We create our own Base object and model it to behave however we like, and then set it using the *setBaseObject(.md)* method. To create this custom object, we call PHPUnit's *getMock(.md)* method with the class name (there are more, optional parameters for *getMock(.md)* like an array of method names, but we don't need them because we required the original class). The *expects(.md)* structure defines the intended return value for a given method -- in this case, we expect the method *getPageXML(.md)* of our Base object to be called exactly once, returning an XML string value. Within the tested method, the mock object will return the expected value, and the test will fail if the method is not called exactly once.

After we've written the test, we can reimplement the *getParsedData(.md)* method using the Base object:

~~~~ {.php}
  /**
  * Get the page output XML
  *
  * @return string XML
  */
  public function getParsedData() {
    $this->setDefaultData();
    $baseObject = $this->getBaseObject();
    $baseObject->setPageData($this->data);
    return $baseObject->getPageXML();
  }
~~~~

One last thing that needs to be explained here is the *setDefaultData(.md)* method in content modules: If edit fields have got default values (the optional last element in their arrays), this call sets all elements in the *\$this-\>data* attribute to those default values unless a special value has been set in the page configuration.

Run the test to see it pass. Aftwards, you can use the page's *Content* tab to enter an arbitrary message. Test the output using the *Preview* section, and your custom content will be displayed. Congratulations -- you have written your first papaya CMS module including dynamic data, and along the way you should have learned two or three things about test-driven development if you're not already familiar with it.

[Kategorie:Papaya CMS](export_de/Kategorie:Papaya_CMS.md) [export_de/Kategorie:Papaya CMS Development](export_de/Kategorie:Papaya_CMS_Development.md) [en:papaya Module Development](/export_en/Papaya_Module_Development.md)
