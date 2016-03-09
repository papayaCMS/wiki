---
title: Moduloptionen hinzufügen
permalink: /Moduloptionen_hinzufügen/
---

Alle von `base_plugin` abgeleiteten Module können ihre eigenen Konfigurationsoptionen mitbringen. Diese sind in einem Array im Klassenattribut `$pluginOptionFields` enthalten. Im Grunde ist dieses Array ähnlich strukturiert wie die `$editFields` bei Content-Modulen. Der Unterschied besteht darin, dass diese Optionen in der Modulverwaltung bearbeitet werden und innerhalb einer Installation nur einen Wert haben, also unabhängig von einzelnen Seiten immer gleich sind.

Die Idee hinter den Moduloptionen besteht darin, dass bestimmte seiten- oder boxübergreifenden Daten seitenunabhängig zur Verfügung gestellt werden. Beispielsweise kann die ID einer bestimmten Seite von verschiedenen Seiten- und Boxmodulen eines Pakets benötigt werden. Die auf diese Weise verlinkte Seite kann spezielle Funktionen enthalten, auf die über eine Weiterleitung mit zusätzlichen Parametern zugegriffen werden kann.

Im folgenden Listing wird die Beispieldefinition für eine Moduloption dargestellt:

**Definition von Moduloptionen (\$pluginOptionFields)**

~~~~ {.php}
...
var $pluginOptionFields = array(
  'my_value' => array('Option 1', 'isNum', TRUE, 'input', 5),
);
...
~~~~

Wenn Sie in der Modulverwaltung das entsprechende Modul auswählen, stehen Ihnen die Moduloptionen unter dem Reiter Optionen zur Verfügung:

[miniatur|zentriert|1000px|Konfiguration von Moduloptionen im Backend](/images/File:moduloptionen-konfigurieren.png )

Und so greifen Sie auf den konfigurierten Wert im Modul zu:

**Moduloptionen mittels papaya_module_options laden**

~~~~ {.php}
include_once(PAPAYA_INCLUDE_PATH.'system/papaya_module_options.php');
$moduleOptionsObj = &new papaya_module_options();
$moduleOptions    = $moduleOptionsObj->getOptions($this->guid);
$value            = $moduleOptions['MY_VALUE'];
~~~~

Aus dem Schlüsselwort `my_value` ist beim Laden `MY_VALUE` geworden. Dadurch lassen sich Moduloptionen besser von normalen Werten unterscheiden.

[export_de/Kategorie.md:Eigene Anwendungen schreiben](export_de/Kategorie.md:Eigene_Anwendungen_schreiben )