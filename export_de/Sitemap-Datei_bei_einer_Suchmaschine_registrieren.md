---
title: Sitemap-Datei bei einer Suchmaschine registrieren
permalink: /Sitemap-Datei_bei_einer_Suchmaschine_registrieren/
---

Damit die Sitemap-Datei durch den Crawler der Suchmaschine gefunden werden kann, müssen Sie die Suchmaschine informieren. Es existieren drei verschiedene Wege, die Sitemap-Datei zu registrieren:

1.  Sie registrieren die Sitemap-Datei direkt beim Betreiber einer Suchmaschine. Die Vorgehensweise hängt dabei von den Regeln des jeweiligen Suchmaschinenbetreibers ab.
2.  Sie tragen die URI der Sitemap-Datei in die robots.txt ein, siehe [Sitemap-Datei bei einer Suchmaschine registrieren](/Sitemap-Datei_bei_einer_Suchmaschine_registrieren ).
3.  Sie senden die URL der Sitemap-Datei über einen HTTP-Request an die Suchmaschine, siehe [Sitemap-Datei bei einer Suchmaschine registrieren](/Sitemap-Datei_bei_einer_Suchmaschine_registrieren ).

URI der Sitemap-Datei in die robots.txt eintragen
-------------------------------------------------

Sie können die URI der Sitemap-Datei einfach in die `robots.txt` eintragen. Auf diese Weise wird die Sitemap-Datei automatisch beim Suchmaschinenbetreiber registriert. Zu diesem Zweck fügen Sie folgende Zeile in Ihre `robots.txt` ein:

**Eintrag in robots.txt**

~~~~ {.robots}
Sitemap: <sitemap_location>
~~~~

Anstelle von `<sitemap_location>` setzen Sie die URL Ihrer Sitemap-Datei ein.

Sitemap über HTTP-Request an Suchmaschine senden
------------------------------------------------

Sie können einen HTTP-Request absetzen, um die Sitemap-Datei der Suchmaschine bekannt zu machen. Dazu müssen Sie lediglich folgende Zeile in die Adressleiste Ihres Browsers eingeben:

**Sitemap über HTTP-Request versenden**

    <nowiki><searchengine_URL>/ping?sitemap=sitemap_url</nowiki>

Ersetzen Sie `<searchengine_URL>` mit der URL der Suchmaschine. Sie können dann den letzten Teil `sitemap_url` durch die URL Ihrer Sitemap-Datei ersetzen. Wenn Ihre URL also <http://www.meine-domain.tld/sitemapdatei.11.sitemap> lautet, hat der HTTP-Request folgende Form:

**Beispiel-HTTP-Request**

    <nowiki>http://www.google.de/ping?sitemap=http://www.meine-domain.tld/sitemapdatei.11.sitemap</nowiki>

Sie müssen jedoch die URL-Angabe mit URL-Encoding maskieren, damit die Sitemap-URL korrekt gelesen werden kann. Der korrekte HTTP-Request hat dann folgende Form:

**Vollständiges und korrektes Beispiel eines HTTP-Requests**

    <nowiki>http://www.google.de/ping?sitemap=http%3A%2F%2Fwww.meine-domain.tld%2Fsitemapdatei.11.sitemap</nowiki>

[Kategorie:Sitemap-Template installieren und der Suchmaschine bekannt machen](/Kategorie:Sitemap-Template_installieren_und_der_Suchmaschine_bekannt_machen )