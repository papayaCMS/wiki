---
title: Action Dispatcher
permalink: /Action_Dispatcher/
---

Der **Action Dispatcher** ist ein freies papaya-Modulpaket. Es handelt sich um die papaya-spezifische Implementierung eines Observers oder Event-Handlers. Eine Aktion ist eine Methode, die automatisch in jedem Connector-Modul aufgerufen werden kann, das in der Administrationsoberfläche des Dispatchers registriert wird. Das ist praktisch für Aufräumarbeiten und andere Aufgaben, die mehr als ein Modul betreffen und beim Auftreten bestimmter Ereignisse automatisch in Gang gesetzt werden sollten.

Verwendung
----------

Der Action Dispatcher definiert *Gruppen*, *Aktionen* und *Observer*. Eine Aktion wird durch einen Gruppennamen (üblicherweise ein papaya-Modulpaket) und den Eigennamen der Aktion spezifiziert. Die Observer können aus den verfügbaren Connector-Modulen der vorliegenden papaya-Installation einschließlich sämtlichen projektspezifischen Erweiterungen ausgewählt werden. Um eine Aktion anzustoßen, müssen Sie eine Instanz der Dispatcher-eigenen Connector-Klasse erzeugen und ihre Methode call() mit einem Gruppennamen, einem Aktionsnamen und einem optionalen Parameter aufrufen. Der Dispatcher erzeugt dann Instanzen aller Observer (Connector-Klassen), die für die jeweilige Aktion registriert sind, ruft deren gleichnamige Methoden auf (falls verfügbar) und übergibt ihnen den Parameter (oder NULL, falls dieser nicht gesetzt ist).

Hier ein Beispiel: Angenommen, eine Gruppe namens 'community' wurde definiert und innerhalb dieser Gruppe existiert eine Aktion namens 'onDeleteSurfer'. Die Methode deleteSurfer() in der Basisklasse der papaya-Community enthält die folgenden Zeilen (die nur ausgeführt werden, wenn der Surfer selbst erfolgreich gelöscht werden kann):

`
 // Call the action dispatcher method for other modules who need to do surfer cleanup stuff
 include_once(PAPAYA_INCLUDE_PATH.'system/base_pluginloader.php');
 $actionsObj = base_pluginloader::getPluginInstance('79f18e7c40824a0f975363346716ff62', $this);
 $num = $actionsObj->call('community', 'onDeleteSurfer', $surferId);
`

Falls ein oder mehrere Observer für die Aktion 'onDeleteSurfer' in der Gruppe 'community' registriert sind und eine Methode namens onDeleteSurfer() enthalten, ruft der Dispatcher diese Methoden nacheinander auf und übergibt ihnen die Surfer-ID. Dies ermöglicht es Ihnen, in jedem Modul Surfer-spezifische Aufräumarbeiten durchzuführen, sowohl in offiziellen papaya-Modulen als auch in Ihren eigenen Erweiterungen. Beispielsweise könnten Sie die Medieninhalte eines Surfers löschen oder den Autorennamen in dessen Forenbeiträgen auf 'Anonym', 'Ex-Benutzer' oder ähnliches setzen.

Sowohl direkte als auch zyklische Rekursion im Aufrufprozess werden durch Untersuchung des Backtrace verhindert; sobald die aktuelle Klasse und Methode bereits zuvor aufgetreten sind, wird die Ausführung unverzüglich beendet. Das Ereignis wird als Fehler im papaya-Protokoll verzeichnet. Wenn eine registrierte Observer-Klasse nicht mehr existiert, wird auch dies im Protokoll vermerkt.

Administration
--------------

Die Administrationsoberfläche des Action Dispatchers ist recht einfach gehalten (siehe Screenshot).

![File:Dispatcher.png](images/File:Dispatcher.png)

In der Hauptsymbolleiste können Sie Gruppen, Aktionen und Observer hinzufügen oder entfernen (Aktionen sind erst verfügbar, sobald eine Gruppe ausgewählt wurde, und Observer erst dann, wenn eine Aktion ausgewählt wurde). Zwei zusätzliche Schaltflächen ermöglichen es Ihnen, die aktuelle Konfiguration als XML zu exportieren beziehungsweise aus einer XML-Datei zu importieren. Das ist praktisch, wenn Sie mehr als einen Server betreiben (zum Beispiel einen Entwicklungs- und einen Produktiv-Server), weil Sie nicht auf jedem einzelnen Server eine manuelle Konfiguration durchzuführen brauchen. Beim Import können Sie wählen, ob die aktuelle Konfiguration vollständig durch die importierten Daten ersetzt werden soll oder ob diese zur bestehenden Konfiguration hinzugefügt werden sollen. Der Versuch, eine formal gültige, aber leere Konfigurationsdatei (d.h. <action-observers/> und nichts weiter) zu importieren, löscht Ihre Konfiguration jedoch auch im Ersetzungsmodus nicht, sondern führt zu einer Fehlermeldung.

Das XML-Format für Action Dispatcher-Konfigurationsdateien sieht wie folgt aus (Beispiel):

`
 <?xml version="1.0" encoding="UTF-8" standalone="yes" ?>
 <action-observers>
   <action-group>
     <name>community</name>
     <action name="onBlockSurfer">
       <observer guid="99db2c2898403880e1ddeeebf7ee726c"/>
     </action>
     <action name="onDeleteSurfer">
     </action>
     <action name="onValidateSurfer">
     </action>
   </action-group>
   <action-group>
     <name>emptyGroup</name>
   </action-group>
 </action-observers>
`

Bitte beachten Sie, dass **strikt davon abzuraten** ist, diese Dateien manuell zu erstellen oder zu editieren. Verwenden Sie stets die Export-Funktion, um sie zu erstellen.

Unter der Symbolleiste wird in der linken Spalte eine Liste der existierenden Gruppen angezeigt. Sobald Sie eine Gruppe auswählen, werden ihre Aktionen (falls vorhanden) in einer zweiten Liste unterhalb der Gruppenliste angezeigt. Wenn Sie eine Aktion auswählen, werden deren registrierte Observer (falls vorhanden) in einer dritten Liste im Haupt-Inhaltsbereich angezeigt. Die Schaltflächen, um die Observer-Registrierungen zu löschen, befinden sich in der rechten Spalte dieser Liste.

Unter *Module \> Action dispatcher \> Action dispatcher \> Optionen* können Sie eine einzige Moduloption einstellen: Ob Aktionen beim Aufruf der Methode call() automatisch registriert werden sollen oder nicht. Standardmäßig ist diese Funktion deaktiviert (vor allem, weil die Datenbanktabellen des Action Dispatchers in einer neuen papaya-Installation noch gar nicht existieren). Wenn Sie sie einschalten, überprüft jeder Aufruf von call(), ob die Gruppe/Aktion, die Sie verwenden, bereits registriert ist, und schreibt sie in die Datenbank, wenn dies nicht der Fall ist. Das Ereignis wird als Information ins papaya-Protokoll eingetragen. Bitte beachten Sie, dass die Autoregistrierung NICHT dafür sorgt, dass tatsächlich Observer aufgerufen werden, weil Sie diese immer noch manuell oder per XML-Import registrieren müssen, aber Sie erfahren aus dem papaya-Protokoll, welche neuen Aktionen aufgerufen werden.

[Category:papaya CMS Entwicklung](/Category:papaya_CMS_Entwicklung "wikilink")
