
Wenn Sie eigene Webprojekte mit papaya CMS implementieren und ein eigenes SVN-Repository betreiben, können Sie Verzeichnisse des öffentlichen papaya-SVN als externals einbinden. Sie müssen also nicht umständlich papaya CMS aus dem öffentlichen papaya-SVN exportieren und in ihrem SVN-Repository einchecken.

Was sind Externals?
===================

Ein External bezeichnet ein Verzeichnis, das in einem anderen SVN-Projekt verwaltet wird, das unter Umständen auch auf einem anderen SVN-Server liegen kann. Wenn Sie den Quellcode dieses externen Projekts in Ihr eigenes Projekt einbinden können, müssen Sie nicht den Quellcode umständlich aus dem anderen Repository exportieren und in Ihr Projektverzeichnis einchecken, sondern definieren eine Einbindung in Form einer External. Der Vorteil besteht darin, dass der Quellcode, den Sie auf diese Weise einbinden, bei jedem Update aktualisiert wird und Sie somit immer die aktuellen Bugfixes sowie neuere Features haben.

Eine External wird als Property eines Verzeichnisses gesetzt. Der Kommandozeilenaufruf kann wie folgt aussehen:

    svn propedit "svn:external" system http://svn.papaya-cms.com/trunk/papayaCMS/papaya-lib/system

Der oben bezeichnete Befehl setzt im aktuellen Verzeichnis ein <svn:external-Property>, das das externe Projektverzeichnis papaya-lib/system aus dem trunk des papaya-SVN in das lokale Unterverzeichnis system setzt. Dieses lokale Unterverzeichnis ist noch nicht angelegt.

Wenn Sie im aktuellen Verzeichnis anschließend ein svn update ausführen, wird das Unterverzeichnis system angelegt und die Daten aus dem externen Projektverzeichnis in dieses Unterverzeichis kopiert.

Aufbau des SVN-Repositorys
==========================

Die folgende Tabele beschreibt die grundlegenden Verzeichnisse des papaya-Repositorys. Entwickler, die bereits mit SVN oder CSV gearbeitet haben, dürfte diese Aufteilung vertraut sein:

|Path|Bedeutung|svn.papaya-cms.com/trunk|Verzeichnis mit der aktuellen Entwicklungsversion.|svn.papaya-cms.com/tags|Verzeichnis mit den stabilen Versionen des papaya CMS|svn.papaya-cms.com/branches|Verzeichnis mit den (stabilen) Builds.|

