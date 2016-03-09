---
title: Formulare für die Seitenausgabe ausgeben
permalink: /Formulare_für_die_Seitenausgabe_ausgeben/
---

Wenn Sie Formulare für die Seitenausgabe erzeugen wollen, können Sie das dazu erforderliche XML natürlich selbst im Content-Modul schreiben. Sie können sich jedoch viel Arbeit ersparen, wenn Sie das Formular-Toolkit verwenden, das in papaya CMS integriert ist. Dadurch erhalten alle erzeugten Formulare eine standardisierte Struktur, die dann später im Template einfacher in das Zielformat transformiert werden kann. Das Formular-Toolkit ist in der Klasse `base_dialog` implementiert.

base_dialog benutzen
---------------------

Das folgende Listing zeigt, wie Sie ein Formular mit der Klasse `base_dialog` anlegen können:

1.  Definieren Sie zunächst ein Array, in dem Dialogfelder definiert werden: **Dialogfelder definieren**
    ~~~~ {.php}
    function getDialogXML() {
      $fields = array(
        'user_name' => array('Name', 'isNoHTML', TRUE,
          'input', 200, '', 'Name'),
        'user_email' => array('E-Mail', 'isNoHTML', TRUE,
          'input', 200, '', 'username@domain.tld', 'left'),
        'subject' => array('Betreff', 'isNoHTML', TRUE,
          'input', 200, '', 'Betreff'),
        'message' => array('Nachricht', 'isSomeText', TRUE
          'textarea', 5, '', 'Deine Nachricht')
      );
    }
    ~~~~

    Näheres zur Definition von Dialogformularen erfahren Sie in [Eingabemasken für Inhaltsmodule definieren](/Eingabemasken_für_Inhaltsmodule_definieren ).

2.  Definieren Sie ein Array mit Standarddaten. Achten Sie darauf, dass die Schlüssel der jeweiligen Datenfelder mit den entsprechenden Schlüsseln im Dialogfeld-Array identisch sind: **Array mit Standarddaten definieren**
    ~~~~ {.php}
    function getDialogXML() {
      //Ausgeschnitten: $fields-Array
      $data = (
        //entspricht $field['user_name']
        'user_name'  => 'Name',
        //entspricht $field['user_mail']
        'user_email' => 'example@domain.tld',
        //entspricht $field['subject']
        'subject'    => 'Betreff',
        //entspricht $field['message']
        'message'    => 'Ihre Nachricht'
      );
    }
    ~~~~

3.  Optional können Sie ein Array für versteckte Parameterfelder definieren: **Array mit versteckten Parameterfeldern definieren**
    ~~~~ {.php}
    function getDialogXML() {
      //Ausgeschnitten: $fields-Array
      //Ausgeschnitten: $data-Array
      $hidden = array(
        'user_id' => $userId,
        'submit'  => 1
      );
    }
    ~~~~

4.  Legen Sie eine Instanz der Klasse `base_dialog` an: **Klasse base_dialog instantiieren**
    ~~~~ {.php}
    function getDialogXML() {
      //Ausgeschnitten: $fields-Array
      //Ausgeschnitten: $data-Array
      //Ausgeschnitten: $hidden-Array

      $this->dialog = &new base_dialog(
        $this, $this->paramName, $fields,
        $data, $hidden
      );
    }
    ~~~~

5.  Laden Sie die Formularfelder mit den Daten, die der Nutzer zuvor eingegeben hat. Diese liegen immer dann vor, wenn der Nutzer das Formular abgesendet hat, einige Eingaben jedoch fehlerhaft waren: **Daten von Nutezrn in Formularfelder laden**
    ~~~~ {.php}
    function getDialogXML() {
      //Ausgeschnitten: $fields-Array
      //Ausgeschnitten: $data-Array
      //Ausgeschnitten: $hidden-Array

      $this->dialog = &new base_dialog(
        $this, $this->paramName, $fields,
        $data, $hidden
      );
      $this->dialog->loadParams();
    }
    ~~~~

6.  Geben Sie den Titel für den Dialog und den Absende-Button an: **Titel für Button und Dialog eingeben**
    ~~~~ {.php}
    function getDialogXML() {
      //Ausgeschnitten: $fields-Array
      //Ausgeschnitten: $data-Array
      //Ausgeschnitten: $hidden-Array

      $this->dialog = &new base_dialog(
        $this, $this->paramName, $fields,
        $data, $hidden
      );
      $this->dialog->loadParams();
      $this->dialog->dialogTitle = 'Kontaktformular';
      $this->dialog->buttonTitle = 'Absenden';
    }
    ~~~~

7.  Geben Sie das XML des Dialogs aus: **XML des Dialogs ausgeben**
    ~~~~ {.php}
    function getDialogXML() {
      //Ausgeschnitten: $fields-Array
      //Ausgeschnitten: $data-Array
      //Ausgeschnitten: $hidden-Array

      $this->dialog = &new base_dialog(
        $this, $this->paramName,
        $fields, $data, $hidden);
      $this->dialog->loadParams();
      $this->dialog->dialogTitle = 'Kontaktformular';
      $this->dialog->buttonTitle = 'Absenden';
      return $this->dialog->getDialogXML();
    }
    ~~~~

[export_de/Kategorie.md:Content ausgeben und Nutzereingaben maskieren](export_de/Kategorie.md:Content_ausgeben_und_Nutzereingaben_maskieren )