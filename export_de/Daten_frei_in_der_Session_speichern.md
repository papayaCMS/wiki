---
title: Daten frei in der Session speichern
permalink: /Daten_frei_in_der_Session_speichern/
---

Bei umfangreichen Anwendungen ist der soeben beschriebene Ansatz mit Parametern nicht günstig. Komplexe Anwendungen können verschiedene Seiten- und Boxmodule besitzen, die Informationen miteinander teilen. Jedes Modul der Anwendung muss die Parameternamen kennen und die Parameter initialisieren, was schnell unübersichtlich werden würde. Es kann daher sinnvoll sein, Daten explizit in der Session abzulegen. Um Konflikte zu verhindern und den Zugriff auf `$_SESSION` zu abstrahieren, stellt `base_object` zwei Funktionen zur Verfügung: `base_object::setSessionValue()` und `base_object::getSessionValue()`.

Im folgenden Beispiel wird eine Session-Speicherklasse vorgestellt, die drei Methoden implementiert:

**Beispiel einer Session-Speicherklasse**

~~~~ {.php}
...
function initialize() {}
function setUserLoggedIn($userId) {}
function getUserLoggedIn($userId) {}
...
~~~~

Methode zum Initialisieren der Sessiondaten
-------------------------------------------

Die Methode `initialize()` liest die Session-Daten beim Initialisieren der Klasse in die lokale Variable `$this->sessionParams`. Bei einem Zugriff auf die Session-Daten müssen also nicht jedes Mal die kompletten Session-Daten ausgelesen werden. Die Daten stehen anderen Methoden für die gesamte Lebenzeit der Klasse im Attribut `$this->sessionParams` zur Verfügung:

**Sessionparameternamen für Klasse initialisieren**

~~~~ {.php}
...
function initialize() {
  $this->_sessionParamName = get_class() . '_user_options';
  $this->sessionParams = $this->getSessionValue($this->_sessionParamName);
}
...
~~~~

Damit es keine Überschneidungen gibt, können Sie `$sessionParamName` aus dem Klassennamen und einem Identifier zusammensetzen.

Methode für den Schreibzugriff auf Sessiondaten
-----------------------------------------------

Die Methode setUserLoggedIn() schreibt die modifizierten Session-Daten zuerst in das lokale array `$this->sessionParams`. Anschließend werden die Sessiondaten mit der Methode `$this->setSessionValue()` in die Session geschrieben: **Wert in der Session ändern**

~~~~ {.php}
...
function setUserLoggedIn($userId) {
  $this->sessionParams['logged_in'] = 1;
  $this->setSessionValue($this->_sessionParamName, $this->sessionParams);
}
...
~~~~

Methode für den Lesezugriff auf Sessiondaten
--------------------------------------------

Die Methode getUserLoggedIn() überprüft, ob der gesuchte Wert in der lokalen Variablen `$this->sessionParams` enthalten ist. Ist dies der Fall, können Sie ihn mit `return` an die aufrufende Instanz zurückgeben:

**Wert aus der Session auslesen**

~~~~ {.php}
...
function getUserLoggedIn($userId) {
  if (isset($this->sessionParams['logged_in'])) {
    return $this->sessionParams['logged_in'];
  }
}
...
~~~~

Die Methode benutzt also nicht `$this->getSessionValue()`, um die Sessiondaten zuerst in ein lokales Array zu lesen, sondern greift auf die Daten im Klassenattribut `$this->sessionParams` zu.

[Kategorie:POST/GET-Parameter lesen und Sessiondaten verwalten](Kategorie:POST/GET-Parameter_lesen_und_Sessiondaten_verwalten )