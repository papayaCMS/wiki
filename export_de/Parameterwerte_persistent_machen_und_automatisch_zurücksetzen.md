---
title: Parameterwerte persistent machen und automatisch zurücksetzen
permalink: /Parameterwerte_persistent_machen_und_automatisch_zurücksetzen/
---

Sie können papaya CMS anweisen, Parameter automatisch in der Session vorzuhalten. Damit werden die Parameter solange beibehalten, bis ein bestimmter anderer Parameter übermittelt wurde oder sie von Ihnen zurückgesetzt werden.

Anhand der Beispielanwendung „Stickers“ aus [:Kategorie:Module für das Backend programmieren](/:export_de/Kategorie:Module_für_das_Backend_programmieren.md) soll die Funktionsweise der Sessiondatenverwaltung in papaya CMS vorgestellt werden. Die „Stickers“-Anwendung verwaltet verschiedene Zitate, die nach Sammlungen geordnet werden können. Die Anwendung erlaubt es also, dass Sie verschiedene Sammlungen hinzufügen und verwalten können, wobei jede Sammlung beliebig viele Einträge enthalten darf.

Wenn Sie in der „Stickers“-Anwendung gerade einen Eintrag bearbeiten, soll der aktuell ausgewählte Eintrag in der Liste der Einträge markiert bleiben, auch wenn Sie gerade auf den Speichern -Button geklickt haben. Während Sie also den Eintrag bearbeiten, darf diese Information nicht verloren gehen. Sie wollen jedoch nicht jedesmal die ID des Eintrags ( `sticker_id`.md) übermitteln. Indem Sie den Parameter `sticker_id` mittels `base_object::initializeSessionParam()` registrieren, wird dieser in der Session vorgehalten. Beim nächsten Seitenaufruf prüft die Methode `initializeParams()`, ob ein Parameter registriert ist und nicht erneut übermittelt wurde. Dann wird der Wert aus der Session geladen.

Im Folgenden soll Schrittweise erklärt werden, wie Sie den Status eines Formulars in der Session speichern:

1.  Definieren Sie zunächst den Parameternamen für die Session. Speichern Sie diesen in der Instanzvariablen \$this-\>sessionParamName ab. Verknüpfen Sie den Session-Parameternamen mit dem Parameternamen aus der Instanzvariablen \$this-\>paramName, um Konflikte mit anderen Klassen zu vermeiden: **Parameternamen für die Session speichern**
    ~~~~ {.php}
    ...
    function getParsedData() {
      $this->sessionParamName = 'PAPAYA_SESS_'.$this->paramName;
    }
    ...
    ~~~~

2.  Initialisieren Sie die Parameter mit `base_object::initializeParams()` und laden Sie die Sessiondaten mit `base_object::getSessionValue()` in das Attribut `$sessionParams`: **Sessionparameter initialisieren und Sessiondaten laden**
    ~~~~ {.php}
    ...
    function getParsedData() {
      $this->sessionParamName = 'PAPAYA_SESS_'.$this->paramName;
      $this->initializeParams();
      $this->sessionParams = $this->getSessionValue($this->sessionParamName);
    }
    ...
    ~~~~

3.  Nach der Initialisierung der einzelnen Sessionparameter müssen die geänderten Sessiondaten mittels `base_object::setSessionValue()` gespeichert werden: **Sessionparameter initialisieren und geänderte Sessiondaten speichern.**
    ~~~~ {.php}
    ...
    function getParsedData() {
      $this->sessionParamName = 'PAPAYA_SESS_'.$this->paramName;
      $this->initializeParams();
      $this->sessionParams = $this->getSessionValue($this->sessionParamName);

      //Einzelne Sessionparameter initialisieren
      $this->initializeSessionParams('sticker_id');
      $this->initializeSessionParams('col_id', array('sticker_id'));

      //Sessiondaten speichern
      $this->setSessionValue($this->sessionParamName, $this->sessionParams);
    }
    ...
    ~~~~

In diesem Beispiel enthält der zweite Aufruf von `initializeSessionParams()` als zweiten Parameter ein Array mit dem Wert `sticker_id`. Dieser zweite Parameter enthält die IDs aller Sessiondaten, die entfernt werden sollen, wenn der erste Parameter in POST oder GET enthalten ist. Im obigen Beispiel wird also der Wert des Sessioneintrags `sticker_id` entfernt, wenn sich der Wert des Eintrags für `col_id` (die ID der Collection/Sammlung) geändert hat.

Die ID des Stickers wird entfernt, um die Datenkonsistenz zu gewährleisten. Wenn der Nutzer eine andere Collection auswählt, soll die ID des zuvor ausgewählten Stickers nicht gespeichert bleiben, da der Sticker zu einer anderen Collection gehört.

[Kategorie:POST/GET-Parameter lesen und Sessiondaten verwalten](export_de/Kategorie:POST/GET-Parameter_lesen_und_Sessiondaten_verwalten.md)