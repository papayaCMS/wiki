---
title: Separate Klasse als Session-Speicher verwenden
permalink: /Separate_Klasse_als_Session-Speicher_verwenden/
---

Für sehr umfangreiche Anwendungen bietet es sich an, Sessiondaten in einem separaten Objekt zu verwalten. Sie können mit Hilfe eines Singletons und statischen Methoden eine schlanke, leicht zu nutzende Klasse schreiben:

**Beispiel einer Sessionspeicherklasse**

~~~~ {.php}
<?php

require_once(PAPAYA_INCLUDE_PATH.'system/sys_base_object.php');

class my_app_options extends base_object {

  var $_sessionParamName = '';

  function &_getInstance() {
    static $instance;
    if (!isset($instance) ||
        !is_object ($instance) ||
        !is_a($instance, 'my_app_options') {
      $instance = new my_app_options;
      $instance->initialize();
    }
    return $instance;
  }

  function initialize() {
    $this->_sessionParamName = get_class().'_app_options';
    $this->sessionParams = $this->getSessionValue($this->_sessionParamName);
  }

  function saveSession() {
    return $this->setSessionValue($this->_sessionParamName, $this->sessionParams);
  }

  function setOptionExampleValue($value = '') {
    $optionObj = self::_getInstance();
    $optionObj->sessionParams['example_value'] = $value;
    return $this->saveSession();
  }

  function getOptionExampleValue() {
    $optionObj = self::_getInstance();
    if (isset($optionObj->sessionParams['example_value'])) {
      return $optionObj->sessionParams['example_value'];
    }
  }
}
?>
~~~~

Verwendet werden kann die Sessionspeicherklasse `my_app_options` folgendermaßen:

**Sessionspeicherklasse benutzen**

~~~~ {.php}
...
my_app_options::setOptionExampleValue('test');
$value = my_app_options::getOptionExampleValue();
...
~~~~

[export_de/Kategorie.md:POST/GET-Parameter lesen und Sessiondaten verwalten](export_de/Kategorie.md:POST/GET-Parameter_lesen_und_Sessiondaten_verwalten )