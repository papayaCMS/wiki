---
title: HTML-Rohling erstellen
permalink: /HTML-Rohling_erstellen/
---

Wenn Sie vorher noch nie Templates für papaya CMS erstellt haben, sollten Sie zuerst einen HTML-Rohling erstellen und anschließend in das Template umbauen. Schreiben Sie also das HTML nicht sofort in ein XSLT-Template, wenn Sie zuerst ein passendes Design mit Layoutgrafiken und ansprechender CSS-Formatierung entwickeln möchten. Diese Herangehensweise bringt einige Vorteile mit sich:

1.  Das „Look and Feel“ der Seite steht schon fest. Sie müssen die Seite lediglich in ein XSLT-Template umschreiben und für die CSS-Ressourcen und Layout-Grafiken ein Theme erstellen.
2.  Sie kennen den Grundaufbau des HTML-Baumes bereits, wenn Sie das XSLT-Template zu schreiben beginnen, was Ihnen die Erstellung des Templates wesentlich erleichtert.
3.  Sie sparen Zeit bei der Entwicklung des Templates, da Sie die Layoutentwicklung von der Templateerstellung trennen.

Anhand der HTML-Ausgabe des Demo-Templates lässt sich das Templatekonzept von papaya CMS veranschaulichen. Sie sollten sich an diesem Konzept orientieren, wenn Sie den HTML-Rohling erstellen:

**HTML-Ausgabe des Demo-Templates**

~~~~ {.xml}
<!DOCTYPE html PUBLIC "-//W3C//DTD XHTML 1.0 Transitional//EN" "http://www.w3.org/TR/xhtml1/DTD/xhtml1-transitional.dtd">
<html ns="http://www.w3.org/1999/xhtml" lang="de-DE" xml:lang="de-DE">
  <head>
  <title>www.domain.tld - Startseite</title>
  <meta http-equiv="Content-Type" content="text/html; charset=utf-8" />
  <link rel="shortcut icon" href="/papaya-themes/default/favicon.ico" />
  <meta http-equiv="imagetoolbar" content="false" />
  <meta name="MSSmartTagsPreventParsing" content="true" />
  <meta name="robots" content="index,follow" />
  <link rel="stylesheet" type="text/css" href="/papaya-themes/default/basic.css?DEV"
    media="screen, projection" />
  <link rel="stylesheet" type="text/css" href="/papaya-themes/default/main.css?DEV"
    media="screen, projection" />
  <link rel="stylesheet" type="text/css" href="/papaya-themes/default/box_navigation.css?DEV"
    media="screen, projection" />
  <link rel="stylesheet" type="text/css" href="/papaya-themes/default/colors.css?DEV"
    media="screen, projection" />
  <link rel="stylesheet" type="text/css" href="/papaya-themes/default/print.css?DEV"
    media="print" /><!--[if IE 7]>
  <link rel="stylesheet" type="text/css" href="/papaya-themes/default/ie7.css?DEV" />
  <![endif]--><!--[if lt IE 7]>
    <link rel="stylesheet" type="text/css" href="/papaya-themes/default/ie6win.css?DEV" />
  <![endif]-->
  <script type="text/javascript" src="/papaya-themes/default/papaya/popup.js?rev=DEV">
  <!-- //--></script>
  <script type="text/javascript" src="/papaya-themes/default/papaya/swfobject/swfobject.js?rev=DEV">
  <!-- //--></script>
  </head>
  <body>
    <div class="accessibilityElement">
      <a name="jump-top" id="jump-top"></a>
      <em>Springen Sie direkt: </em>
      <ul>

        <li>
          <a href="#jump-content" accesskey="2">zum Inhalt</a>
        </li>
        <li>
          <a href="#jump-navigation" accesskey="3">zum Hauptmenü</a>
        </li>
        <li>
          <a href="./" accesskey="0">zur Startseite</a>

        </li>
      </ul>
    </div>
    <hr class="accessibilityElement" />
    <div id="header" class="outerCorners">
      <div class="topCorners">
        <div>
<!-- header -->
        </div>

      </div>
      <div class="leftBorder">
        <div class="rightBorder">
          <div class="headerBackground">
            <div class="headerContent">
              <a href="./">
                <img src="/papaya-themes/default/pics/logo.gif" alt="www.domain.tld" />
              </a>
              <div class="floatFix"> </div>

            </div>
            <ul class="pageTranslations">
              <li class="selected">
                <a href="startseite.99.de.html.preview" title="Startseite">Deutsch</a>
              </li>
              <li>
                <a href="startseite.99.en.html.preview" title="Startseite">English</a>
              </li>

            </ul>
            <div class="floatFix"> </div>
          </div>
        </div>
      </div>
      <div class="bottomBorder">
        <div>
<!-- /header -->
        </div>

      </div>
    </div>
    <hr class="accessibilityElement" />
    <div id="page" class="outerCorners">
      <div class="topCorners">
        <div>
<!-- page -->
        </div>
      </div>

      <div class="leftBorder">
        <div class="rightBorder">
          <div class="threeColumnLayout">
            <div class="pageBackground">
              <a name="jump-navigation" id="jump-navigation"> </a>
<!-- noindex -->
              <div class="boxGroup" id="pageNavigation">
                <div class="box first last">
                  <div class="boxTitle">News</div>

                  <div class="boxData"><ul class="navigation">
  <li class="active first">
    <a href="http://papaya5-siddi.dev2.papaya.local/startseite.99.de.html.preview" class="active first">
      <dfn class="accessibilityElement">1 </dfn>
      <span>Startseite</span>
      <span class="accessibilityElement acessibilitySeparator">. </span>
    </a>

  </li>
  <li>
    <a href="http://papaya5-siddi.dev2.papaya.local/aktuelles.100.de.html.preview">
      <dfn class="accessibilityElement">2 </dfn>
      <span>Aktuelles</span>
      <span class="accessibilityElement acessibilitySeparator">. </span>
    </a>

  </li>
  <li>
    <a href="http://papaya5-siddi.dev2.papaya.local/papaya-cms.107.de.html.preview">
      <dfn class="accessibilityElement">3 </dfn>
      <span>papaya CMS</span>
      <span class="accessibilityElement acessibilitySeparator">. </span>
    </a>

  </li>
  <li>
    <a href="http://papaya5-siddi.dev2.papaya.local/sitemap.108.de.html.preview">
      <dfn class="accessibilityElement">4 </dfn>
      <span>Sitemap</span>
      <span class="accessibilityElement acessibilitySeparator">. </span>
    </a>

  </li>
  <li>
    <a href="http://papaya5-siddi.dev2.papaya.local/forum.109.de.html.preview">
      <dfn class="accessibilityElement">5 </dfn>
      <span>Forum</span>
      <span class="accessibilityElement acessibilitySeparator">. </span>
    </a>

  </li>
  <li>
    <a href="http://papaya5-siddi.dev2.papaya.local/podcast-channel.110.de.html.preview">
      <dfn class="accessibilityElement">6 </dfn>
      <span>Podcast-Channel</span>
      <span class="accessibilityElement acessibilitySeparator">. </span>
    </a>

  </li>
  <li>
    <a href="http://papaya5-siddi.dev2.papaya.local/faqs.114.de.html.preview">
      <dfn class="accessibilityElement">7 </dfn>
      <span>FAQs</span>
      <span class="accessibilityElement acessibilitySeparator">. </span>
    </a>

  </li>
  <li class="last">
    <a href="http://papaya5-siddi.dev2.papaya.local/kontakt.116.de.html.preview" class="last">
      <dfn class="accessibilityElement">8 </dfn>
      <span>Kontakt</span>
      <span class="accessibilityElement acessibilitySeparator">. </span>
    </a>

  </li>
</ul>
</div>
                </div>
              </div>
<!-- /noindex -->
              <hr class="accessibilityElement" />
              <div id="pageContent">
                <a name="jump-content" id="jump-content"> </a>
<!-- noindex -->

<div class="boxGroup" id="ariadne">
<div class="box first last">
 <div class="boxData"><ul class="navigation">
  <li class="first">
    <a href="http://papaya5-siddi.dev2.papaya.local/startseite.99.de.html.preview" class="first">
      <dfn class="accessibilityElement">1 </dfn>
      <span>Startseite</span>
      <span class="accessibilityElement acessibilitySeparator">. </span>

    </a>
  </li>
</ul>
</div>
</div>
</div>
<!-- /noindex -->
   <div id="content">
     <h1>Neues</h1>
       <div class="contentData"><p>Unser SuperServer SuperSuSe 2000 XXXXL ist total toll. Außerdem
verfügt er über einen eingebauten Laser-Pointer - praktisch, <acronym title="foo">wenn</acronym>
Sie im Serverraum eine Präsentation abhalten möchten.</p>

<p><a href="hausbuch-klein-flv.download.preview.49d65e02e5c035fe801f431865fd31e0" title="">hausbuch
klein.FLV (9,47 MB)</a></p>
<p>Sie können aber auch ganz andere Dinge machen, zum Beispiel den SuperSuperSuSe 3000 XXXXXXXXXXXL
kaufen, der mindestens doppelt so toll ist - und zehn Mal mehr kostet als der SuperSuSe 2000 XXXXL,
dafür aber die milliardenfache Leistung bringt, die Sie Tag für Tag nur für den zehnfachen Preis
abrufen können - in Wirklichkeit also ein wahres Schnäppchen, das Sie sich nicht entgehen lassen
sollten. Bestellen Sie jetzt unter ...</p>
<table border="0">
<caption>Preisliste</caption>
<tbody>
<tr>
<td><strong>Position</strong></td>
<td><strong>Preis</strong></td>
</tr>
<tr>
<td>SuperSuSe 2000 XXXXL</td>

<td>  99.999,-- €</td>
</tr>
<tr>
<td>SuperSuperSuSe 3000 XXXXXXXXXXXL</td>
<td>999.999,-- €</td>
</tr>
</tbody>
</table>
<p><a href="kurzfilm.download.preview.a2335168605c9289cb465b6475c2f40b" title="">Kurzfilm.flv (6,66 MB)</a>
</p><div class="floatFix"> </div></div>
                  <div class="floatFix"> </div>

                </div>
                <a href="#jump-top" class="accessibilityElement">topJump</a>
                <div class="floatFix"> </div>
              </div>
<!-- noindex -->
              <div class="boxGroup" id="pageAdditional">
                <div class="box first last">
                  <div class="boxData"><?xml version="1.0"?>

<div class="forumBox">
  <h2 class="boxCaption">Forenübersicht</h2>
  <div class="forum">
    <h3 class="forumTitle">Forum "Installation"</h3>
    <div class="forumDesc">Wie Sie papaya CMS installieren können und was dabei zu beachten ist.</div>
    <div class="forumEntry">Letzter Eintrag: <a
      href="startseite.99.de.html.preview?ff[forum_id]=2&ff[thread_id]=13&ff[entry_id]=13">
      Re: papaya CMS installieren</a>
    </div>
  </div>
  <div class="forum">
    <h3 class="forumTitle">Forum "Benutzung"</h3>
    <div class="forumDesc">Wie Sie papaya CMS benutzen, um bspw. Seiten anzulegen oder Inhalte
    einzupflegen.</div>
    <div class="forumEntry">Letzter Eintrag: <a href="startseite.99.de.html.preview?
           ff[forum_id]=3&ff[thread_id]=14&
           amp;ff[entry_id]=14">Jetzt neu!</a>
    </div>
  </div>
</div>
</div>
    </div>
  </div>
<!-- /noindex -->
  <a href="#jump-top" class="accessibilityElement">topJump</a>

  <div class="floatFix"> </div>
 </div>
</div>
 </div>
 <div class="floatFix"> </div>
 </div>
      <div class="bottomBorder">
        <div>

<!-- /page -->
        </div>
      </div>
    </div>
    <hr class="accessibilityElement" />
    <div id="footer" class="outerCorners">
      <div class="topCorners">
        <div>
<!-- footer -->
        </div>

      </div>
      <div class="leftBorder">
        <div class="rightBorder">
          <div class="footerContent">
            <div id="footerNavigation">
              <ul>
                <li class="last">
                  <a href="#jump-top" class="last" accesskey="1">Seitenanfang</a>

                </li>
              </ul>
            </div>
            <div id="copyright">
    powered by <a href="http://www.papaya-cms.com/">papaya CMS</a></div>
            <div class="floatFix"> </div>
          </div>

        </div>
      </div>
      <div class="bottomBorder">
        <div>
<!-- /footer -->
        </div>
      </div>
    </div>
    <div class="floatFix"> </div>

  </body>
</html>
~~~~

Die HTML-Ausgabe weist folgende Eigenschaften auf:

1.  CSS-Formatangaben sowie JavaScript-Programme liegen alle als externe Dateien vor.
2.  Die verschiedenen zentralen Bereiche der Seite werden durch `<div>` -Elemente gebildet. `<div>` -Elemente zeichnen sich dadurch aus, dass sie geringe Anzeigeeigenschaften besitzen. Diese geringen Anzeigeneigenschaften können durch das CSS genauer spezifiziert werden.
3.  Die Position und Größe der Seitenbereiche wird ausschließlich durch CSS bestimmt.
4.  Die Hintergrundgrafiken werden durch entsprechende CSS-Anweisungen eingebunden.

Durch diese Eigenschaften wird eine effektive Trennung von Inhalt und Form erreicht. Der HTML-Baum fungiert ausschließlich als Container für Inhalte, während Formate durch CSS bestimmt werden.

Hinweise zur Barrierefreiheit

Wenn Sie sich den Quellcode genauer betrachten, werden Sie einige Elemente erkennen, die in der Webseitenausgabe nicht sichtbar sind:

**Verbesserung der Barrierefreiheit durch Sprungmarken**

~~~~ {.xml}
<div class="accessibilityElement">
  <a name="jump-top" id="jump-top"> </a>
  <em>Springen Sie direkt: </em>
  <ul>
    <li>
      <a href="#jump-content" accesskey="2">zum Inhalt</a>
    </li>
    <li>
      <a href="#jump-navigation" accesskey="3">zum Hauptmenü</a>
    </li>
    <li>
      <a href="./" accesskey="0">zur Startseite</a>
    </li>
  </ul>
</div>
<hr class="accessibilityElement" />
~~~~

Diese Elemente werden durch das CSS augeblendet, bleiben aber in Textbrowsern wie Lynx oder speziellen blindengerechten Browsern sichtbar.

[Kategorie:Implementierungsphase: Webseitenvorlage erstellen](/Kategorie:Implementierungsphase:_Webseitenvorlage_erstellen )