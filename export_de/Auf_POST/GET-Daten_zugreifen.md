---
title: Auf POST GET-Daten zugreifen
permalink: /Auf_POST/GET-Daten_zugreifen/
---

Jede dynamische Webanwendung lebt davon, dass sie auf Benutzereingaben in Form von Formularen und parametrisierten Links reagieren kann. Damit unterschiedliche Anwendungen sich dabei nicht ins Gehege kommen, verwendet jedes Modul in papaya CMS einen anderen Identifier, den so genannten `$paramName`. papaya CMS verwendet diesen, um Links und Formularfelder entsprechend zu benennen. Die HTML-Ausgabe sieht folgendermaßen aus:

**Identifier für Parameternamen**

~~~~ {.xml}
<form action="mymodule.123.html?my[mode]=test" method="post">
  <input type="text" name="my[name]" />
</form>
~~~~

Die Methode `base_object::initializeParams()` lädt alle für die Anwendung relevanten Parameter in die Instanzvariable `$this->params`. `$this->params` hat den Typ Array. Damit die Methode `initializeParams()` ausschließlich diejenigen Daten in das Array `$this->params` lädt, die auch zur Anwendung gehören, müssen Sie dem Attribut `$paramName` einen eindeutigen Parameternamen zuweisen. Dieser Parametername sollte sofort kennzeichnen, zu welcher Anwendung die jeweiligen Parameter gehören. So könnte der `$paramName` für die Anwendung „Meine tolle Anwendung“ beispielsweise „mta“ lauten.

Wenn Sie also ein Seiten- oder Boxmodul geschrieben haben, das Nutzerdaten entgegennimmt, müssen Sie auf jeden Fall `base_object::initializeParams()` in Ihrer Klasse ausführen. Erst dann können Sie auf diese Daten über `$this->params` zugreifen. Die geeignete Stelle dafür in Seiten- und Boxmodulen ist die Methode `getParsedData()`.

Im folgenden Codebeispiel wird dargestellt, wie `base_object::initializeParams()` in einem Seitenmodul benutzt wird:

**base_object::initializeParams() aufrufen**

~~~~ {.php}
...
var $paramName = 'my';

function getParsedData() {
  $this->initializeParams();
  // ...
}
...
~~~~

Anschließend stehen die übermittelten Parameter in `$this->params` zur Verfügung. Ausgehend vom o.g. Formularbeispiel enthält `$this->params` beispielsweise folgende Inhalte:

**Beispielinhalte des Arrays \$this-\>params**

~~~~ {.php}
array(
  'mode' => 'test',
  'name' => 'Friedrich',
);
~~~~

Wenn POST- und GET-Parameter eingelesen werden, sind folgende Punkte zu beachten:

1.  Wird ein POST-Parameter mit dem selben Namen wie ein GET-Parameter übertragen, wird der GET-Parameter überschrieben. POST-Parameter haben also in der Auswertung stets Vorrang vor gleichnamigen GET-Parametern.
2.  Die Methode `initializeParams()` kümmert sich auch um Magic Quotes, also das Maskieren der Daten.
3.  Strings werden automatisch nach UTF-8 umgewandelt.

Wenn Sie die `getLink()` -ähnlichen Methoden verwenden um Links zu generieren, müssen Sie nicht manuell die GET-Parameter abhängig von `$paramName` zusammensetzen. Es genügt, wenn Sie den Methoden den Parameternamen sowie die Liste der Parameter und Werte in Form eines assoziativen Arrays übergeben. Näheres zu den Link-Methoden erfahren Sie in [Links ausgeben](/Links_ausgeben "wikilink"). Auch `base_dialog` sowie `base_frontend_form` kümmert sich automatisch um die korrekte Ausgabe der Parameter, siehe [Formulare für die Seitenausgabe ausgeben](/Formulare_für_die_Seitenausgabe_ausgeben "wikilink").

[Kategorie:POST/GET-Parameter lesen und Sessiondaten verwalten](/Kategorie:POST/GET-Parameter_lesen_und_Sessiondaten_verwalten "wikilink")