---
title: XSLT-Haupttemplate erstellen
permalink: /XSLT-Haupttemplate_erstellen/
---

Grundsätzlich haben Sie zwei Möglichkeiten, für papaya CMS ein Tempate zu erstellen:

1.  Sie erstellen alle notwendigen XSLT-Templatedateien selber.
2.  Sie passen die XSLT-Templates aus dem papaya-Demotemplate, indem Sie die XSLT-Templates überladen.

Wenn Sie die kompletten XSLT-Templates selber erstellen möchten, müssen Sie sich nicht in die Systematik des Demo-Templates einarbeiten. Diese Vorgehensweise kann insbesondere dann von Vorteil sein, wenn Sie sich mit XSLT noch nicht so gut auskennen und daher eine sehr einfache Webseitenvorlage erstellen möchten. Der Nachteil ist, dass Sie die kompletten Templates für die benötigten Seiten und Boxen alleine erstellen müssen und nicht auf die Hilfstemplates zugreifen können.

Die zweite Vorgehensweise hat den Vorteil, dass Sie nur einige Templates anzupassen brauchen, um das zentrale HTML ausgeben zu können. Dabei können Sie auf zahlreiche Hilfstemplates und Funktionen zurückgreifen, die im Demo-Template enthalten sind. Zu diesen Funktionen gehört beispielsweise das Phrasensystem, mit dem Sie Beschriftungen für Standard-Buttons oder Formularfelder in der aktuell ausgewählten Content-Sprache ausgeben können. Mit den Stylesheets aus dem papaya-Demo-Template steht Ihnen also ein XSLT-Framework für die Template-Entwicklung zur Verfügung.

Grundlegende Schritte beim Erstellen eines Templates
----------------------------------------------------

Um ein XSLT-Template für papaya CMS zu erstellen, sind folgende Schritte notwendig:

1.  Legen Sie einen Ordner mit der notwendigen Verzeichnisstruktur an. Näheres zur Verzeichnisstruktur erfahren Sie in [Verzeichnisstruktur](/Verzeichnisstruktur ). Dazu können Sie wie folgt vorgehen:
    1.  Sie legen die notwendigen Verzeichnisse selbst an. Dieser Schritt ist notwendig, wenn Sie alle notwendigen Templates selbst erstellen möchten.
    2.  Sie kopieren das Verzeichnis `default-xhtml/` des papaya-Demo-Templates und benennen es um. Dieser Schritt ist empfohlen, wenn Sie die XSLT-Templates aus dem Demo-Template anpassen wollen.

2.  Erstellen Sie das Hauptstylesheet für die Seite ( `page_main.xsl` ), die von allen anderen Seitenstylesheets importiert wird.
    1.  Wenn Sie die Templates sebst erstellen: Legen Sie in diesem Stylesheet ein Template an, dass das HTML-Grundgerüst ausgibt. Für Boxbereiche sowie für den Content-Bereich sollten entsprechende Templates aufgerufen werden.
    2.  Wenn Sie das Demo-Template überladen: Überladen Sie in diesem Stylesheet das Template "page" sowie alle weiteren abhängigen Basistemplates, die Sie benutzen möchten.

3.  Erstellen Sie die Seiten- sowie die Boxtemplates für die benötigten Seiten- und Boxmodule. Die Stylesheets für Seiten importieren dabei immer die Stylesheetdatei `page_main.xsl` und überladen das Template für den Content-Bereich der Seite.
4.  Legen Sie eine `info.xml` an. Diese Datei enthält die Namen aller Boxgruppen, die von Ihrem Template unterstützt werden.

Verzeichnisstruktur anlegen
---------------------------

Legen Sie im Ordner `papaya-data/templates/` einen Unterordner für Ihr Template an. In dieses Verzeichnis können Sie alle Unterverzeichnisse des Demo-Templates, die innerhalb von default-xhtml/ kopieren

Dieser Ordner, beispielsweise `mein-Templateset/`, muss unbedingt ein Verzeichnis enthalten, in das alle von Ihnen erstellten Website-Templates eingefügt werden. Im Default-Template hat dieses Verzeichnis den Namen `html/` (Näheres zur Verzeichnisstruktur des Default-Templates erfahren Sie in [Verzeichnisstruktur](/Verzeichnisstruktur ).). Aus Gründen der Übersichtlichkeit sollten Sie sich an die vorgegebene Namenskonvention halten und das Verzeichnis für die Webseiten ebenso `html/` nennen. Die Verzeichnisstruktur innerhalb von `papaya-data/template/mein-Templateset/` hat dann folgende Verzeichnisstruktur:

_functions
Standard-Templates zum Formatieren und Verarbeiten von Strings.

_lang
Templates zum Übersetzen von Phrasen (Default-Template).

html
Verzeichnis für die Webseitenvorlagen.

html/base
Basistemplates für die Webseitenvorlagen aus dem Default-Template.

html/modules
Templates für Seitenmodule, nach Paketen geordnet (Default-Template).

Wenn Sie in papaya CMS den Ausgabefilter für die Webseitenausgabe konfigurieren, können Sie dieses Verzeichnis `html/` auswählen.

Hauptstylesheet erstellen
-------------------------

Das Demo-Template ist so aufgebaut, dass das HTML-Grundgerüst in der zentralen Stylesheetdatei `defaults.xsl` aus dem Verzeichnis `html/base/` definiert wird. Die Stylesheetdatei `defaults.xsl` definiert mehrere Templates. Das zentrale Template mit dem Namen `page` erzeugt dabei das Seiten-HTML, in dem Bereiche für die Boxen und den Content-Bereich definiert sind. Im `<div>` -Element für den Content-Bereich wird das Template mit dem Namen `content-area` aufgerufen. Zudem werden weitere Templates für die Navigationsboxen, den Seiten-Header und den Seiten-Footer aufgerufen.

Im Basisverzeichnis `html/` ist ferner die Datei `page_main.xsl` enthalten. Dieses Stylesheet importiert die Datei `defaults.xsl`. `page_main.xsl` definiert eine Reihe von Parametern und enthält ein einziges Template mt dem `match` -Attribut, das das Muster „page“ enthält. Das Muster in diesem `match` -Attribut passt dabei zum Wurzelelement des papaya-Seiten-XMLs. Der einzige Inhalt dieses Templates ist es, das Basistemplate `page` aus `base/defaults.xsl` aufzurufen.

Für jedes andere Seitenmodul wird im Basisverzeichnis `html` des Default-Templates eine Stylesheetdatei mit dem Präfix `page_` angelegt, welche diese zentrale Stylesheetdatei `page_main.xsl` importiert und im Prinzip nur das Template `content-area` zu überladen braucht.

Ein weiteres wesentliches Merkmal des Demo-Templates ist es, dass die einzelnen Stylesheet-Dateien für die Seitenmodule im Verzeichnis `html/` nicht direkt für die Modulausgabe zuständig sind. Stattdessen rufen sie entsprechende Templates auf, die sich in Stylesheetdateien im Unterordner `html/modules/` befinden. Das Template `content-area` enthält also selbst keine eigenen Definitionen für die HTML-Ausgabe, sondern ruft weitere Templates auf.

Wenn Sie eigene Templates schreiben, müssen Sie sich nicht an diese Vorgabe halten und können das HTML direkt in die Seiten- und Box-Templates im Verzeichnis html schreiben. Für kleinere Projekte, die nur einige Seiten- und Boxmodule benutzen, ist diese Vorgehensweise übersichtlicher.

Die folgende Beschreibung stellt eine Schritt-für-Schritt-Anleitung dar, in der Ihnen erklärt wird, wie das Hauptstylesheet des Demo-Templates erstellt wird. Sie finden dieses Stylesheet im Verzeichnis `default-html/html/` unter dem Namen `page_main.xsl`. Gehen Sie nun wie folgt vor, um dieses Hauptstylesheet zu erstellen:

1.  Legen Sie im Verzeichnis `mein-Template/html/` die Datei `page_main.xsl` an. Sie können natürlich auch jede andere beliebige Bezeichnung wählen, jedoch sollten Sie sich dabei an die Namenskonvention halten und für Seitentemplates das Präfix `page_` beibehalten.
2.  Erzeugen Sie das XSLT-Grundgerüst: **XSLT-Grundgerüst erzeugen**
    ~~~~ {.xml}
    <?xml version="1.0"?>
    <xsl:stylesheet version="1.0" xmlns:xsl="http://www.w3.org/1999/XSL/Transform"
      ns="http://www.w3.org/TR/REC-html40">
    <!--
      @papaya:modules content_imgtopic, content_errorpage,
      content_categimg, content_tagcateg, content_xhtml
    -->

    <xsl:output method="html" encoding="utf-8" standalone="yes"
      doctype-public="-//W3C//DTD HTML 4.01 Transitional//EN"
      indent="yes" omit-xml-declaration="yes" />

      <xsl:template match="page">
        <xsl:call-template name="page"/>
      </xsl:template>

    </xsl:stylesheet>
    ~~~~

    Eine Besonderheit des Templates ist der Kommentar direkt unterhalb des `<xsl:stylesheet>` -Elements. Die Kommentarzeile enthält eine kommaseparierte Liste mit den Namen der Seitenmodule, die durch dieses Template unterstützt werden. Näheres dazu erfahren Sie in [Modulunterstützung des XSLT-Templates festlegen](/Modulunterstützung_des_XSLT-Templates_festlegen ).

3.  Fügen Sie nun die Import-Deklarationen für externe Stylesheetdateien ein: **Import-Deklarationen in page_main.xsl einfügen**
    ~~~~ {.xml}
    ...
    <xsl:import href="./base/defaults.xsl" />
    ...
    ~~~~

    Die einzige zu importierende Stylesheet-Datei ist `base/defaults.xsl`.

4.  Fügen Sie nun die XSLT-Parameter ein, über die die Template-Engine von papaya CMS vielfältige Informationen wie Pfadangaben für Theme und JavaScript einfügt (Näheres zu den hier verwendeten Parametern erfahren Sie in [Templates und Parameter in ./html/page_main.xsl](/Templates_und_Parameter_in_./html/page_main.xsl ) ): **XSLT-Parameter (papaya CMS) einfügen**
    ~~~~ {.xml}
    ...
    <!--
      papaya CMS parameters
    -->

    <!-- page title (like in navigations) -->
    <xsl:param name="PAGE_TITLE" />
    <!-- content language (example: en-US) -->
    <xsl:param name="PAGE_LANGUAGE"></xsl:param>
    <!-- base installation path in browser -->
    <xsl:param name="PAGE_WEB_PATH" />
    <!-- url of this page -->
    <xsl:param name="PAGE_URL" />
    <!-- theme name -->
    <xsl:param name="PAGE_THEME" />
    <!-- theme path in browser -->
    <xsl:param name="PAGE_THEME_PATH" />
    <!-- theme path in server file system -->
    <xsl:param name="PAGE_THEME_PATH_LOCAL" />

    <!-- current ouput mode (file extension in url) -->
    <xsl:param name="PAGE_OUTPUTMODE_CURRENT" />
    <!-- default/main ouput mode -->
    <xsl:param name="PAGE_OUTPUTMODE_DEFAULT" />
    <!-- public or preview page? -->
    <xsl:param name="PAGE_MODE_PUBLIC" />
    <!-- website version string if available -->
    <xsl:param name="PAGE_WEBSITE_REVISION" />

    <!-- papaya cms version string if available -->
    <xsl:param name="PAPAYA_VERSION" />
    <!-- installation in dev mode? (option in conf.inc.php) -->
    <xsl:param name="PAPAYA_DBG_DEVMODE" />

    <!-- current local time and offset from UTC -->
    <xsl:param name="SYSTEM_TIME" />
    <xsl:param name="SYSTEM_TIME_OFFSET" />
    ...
    ~~~~

5.  Fügen Sie nun die XSLT-Parameter ein, die in den Templates des Default-Templates benutzt werden (Näheres zu den hier verwendeten Parametern erfahren Sie in [Templates und Parameter in ./html/page_main.xsl](/Templates_und_Parameter_in_./html/page_main.xsl ) ): **XSLT-Parameter (Template) einfügen**
    ~~~~ {.xml}
    ...
    <!--
      template set parameters
    -->

    <!-- use favicon.ico in theme directory -->
    <xsl:param name="FAVORITE_ICON" select="true()" />

    <!-- IE only, disable the mouseover image toolbar, default: true -->
    <xsl:param name="IE_DISABLE_IMAGE_TOOLBAR" select="true()" />
    <!-- IE only, disable the smart tag linking, default: true -->
    <xsl:param name="IE_DISABLE_SMARTTAGS" select="true()" />
    <!-- IE only, optional user agent compatibility definition, default: not used -->
    <xsl:param name="USER_AGENT_COMPATIBILITY"></xsl:param>

    <!-- define Indexing for robots -->
    <xsl:param name="PAGE_META_ROBOTS">index,follow</xsl:param>

    <!-- add css classes to boxes based on the module class name -->
    <xsl:param name="BOX_MODULE_CSSCLASSES" select="false()" />
    <!-- load file containing module specific css files name definitions -->
    <xsl:param name="BOX_MODULE_CSSFILES" select="document('boxes.xml')" />
    <!-- do not index box output (puts noindex comments around it) -->
    <xsl:param name="BOX_DISABLE_INDEX" select="true()" />

    <!-- disable the navigation column, even if the xml contains boxes for it -->
    <xsl:param name="DISABLE_NAVIGATION_COLUMN" select="false()" />
    <!-- disable the additional content column, even if the xml contains boxes for it -->
    <xsl:param name="DISABLE_ADDITIONAL_COLUMN" select="false()" />

    <!-- maximum columns for multiple column outputs of items (like subtopics) -->
    <xsl:param name="MULTIPLE_COLUMNS_MAXIMUM" select="3" />

    <!-- use international date time formatting, ISO 8601 -->
    <xsl:param name="DATETIME_USE_ISO8601" select="false()" />
    <!-- char between date and time (ISO 8601 = T, default =  ) -->
    <xsl:param name="DATETIME_SEPARATOR"> </xsl:param>
    <!-- default date time format: short, medium or large -->
    <xsl:param name="DATETIME_DEFAULT_FORMAT">short</xsl:param>

    <!-- load current language texts -->
    <xsl:param name="LANGUAGE_TEXTS_CURRENT" select="document(concat($PAGE_LANGUAGE, '.xml'))" />
    <!-- load fallback language texts -->
    <xsl:param name="LANGUAGE_TEXTS_FALLBACK" select="document('en-US.xml')" />

      <xsl:template match="page">
    ...
    ~~~~

6.  Erstellen Sie nun das HTML-Grundgerüst. Dazu definieren Sie einfach ein Template mit dem `name` -Attribut „page“. Dieses Template ist im importierten Stylesheet `base/defaults.xsl` bereits definiert und wird durch die erneute Definition überladen. Beginnen Sie mit dem HTML-Head-Element: **HTML-Grundgerüst einfügen (Head-Element)**
    ~~~~ {.xml}
    ...
    <xsl:template name="page">
      <html lang="{$PAGE_LANGUAGE}">
        <head>
          <xsl:call-template name="html-head" />
        </head>
        <body>
    ...
        </body>
    </xsl:template>
    ...
    ~~~~

    Wie Sie in Beispiel "HTML-Grundgerüst einfügen (Head-Element)" in [XSLT-Haupttemplate erstellen](/XSLT-Haupttemplate_erstellen ) sehen können, wird der Seitentitel über den Wert im Parameter \$PAGE_TITLE in die Seitenausgabe eingefügt. Der gesamte Inhalt des `<head>` -Elements wird über das Template mit dem Namen `html-head` erzeugt, siehe [Templates in ./html/base/defaults.xsl](/Templates_in_./html/base/defaults.xsl ).

7.  Fahren Sie mit dem Body-Element fort. Sie fügen zunächst die HTML-Elemente für den Content-Bereich ein: '''Template-Aufrufe am Anfang des
    <body>
    -Elements'''

    ~~~~ {.xml}
    ...
    <body>
      <xsl:call-template name="accessibility-navigation" />
      <xsl:call-template name="header" />

      <xsl:variable name="hasNavigation" select="not($DISABLE_NAVIGATION_COLUMN) and
         (count(boxes/box[@group = 'navigation']) > 0)" />
      <xsl:variable name="hasAdditional" select="not($DISABLE_ADDITIONAL_COLUMN) and
         (count(boxes/box[@group = 'additional']) > 0)" />
    ...
    ~~~~

    In der ersten Zeile wird das Template `accessibility-navigation` aufgerufen. Dieses Template erzeugt eine Navigation, die für Screenreader gedacht ist. Die entsprechenden HTML-Elemente sind dabei per CSS „unsichtbar“ gestellt. Näheres zu diesesm Template erfahren Sie in [Templates in ./html/base/defaults.xsl](/Templates_in_./html/base/defaults.xsl ). Mit dem zweiten Template-Aufruf `header` wird das XSLT-Template für den Seitenkopf aufgerufen, siehe [Templates in ./html/base/defaults.xsl](/Templates_in_./html/base/defaults.xsl ). Mit den zwei folgenden Variablen wird ermittelt, ob die Navigationsspalte sowie die rechte Spalte angezeigt werden sollen. Es wird getestet, ob die Seite Boxen für die Navigation ( `hasNavigation` ) enthält und der XSLT-Parameter `DISABLE_NAVIGATION_COLUMN` auf `false` gestellt ist. Für die Variable `hasAdditional` wird überprüft, ob die Seite Boxen für die rechte Spalte enthält und ob die Variable `DISABLE_ADDITIONAL_COLUMN` auf `false` gestellt ist. Die Variablen erhalten jeweils den Wert `true`, wenn die Spalten dargestellt werden können.

8.  Anschließend wird das zentrale Seiten-HTML erzeugt. Das `<div>` -Element in Zeile 6 erhält über die `<xsl:attribute>` -Anweisung ein passendes Layout-Attribut, je nachdem, ob die linke und rechte Spalte oder beide aktiviert worden sind: **HTML-Grundgerüst einfügen (Body-Element)**
    ~~~~ {.xml}
    ...
    <div id="page" class="outerCorners">
      <div class="topCorners"><div><xsl:comment> page </xsl:comment></div></div>
        <div class="leftBorder">
          <div class="rightBorder">
            <div>
              <xsl:attribute name="class">
                <xsl:choose>
                  <xsl:when test="$hasNavigation and $hasAdditional">threeColumnLayout</xsl:when>
                  <xsl:when test="$hasNavigation">twoColumnLayoutLeft</xsl:when>
                  <xsl:when test="$hasAdditional">twoColumnLayoutRight</xsl:when>
                  <xsl:otherwise>singleColumnLayout</xsl:otherwise>
                </xsl:choose>
              </xsl:attribute>
              <div class="pageBackground">
                <xsl:if test="$hasNavigation">
                  <a name="jump-navigation"><xsl:text> </xsl:text></a>
                  <xsl:call-template name="box-group">
                    <xsl:with-param name="boxes" select="boxes/box[@group = 'navigation']"/>
                    <xsl:with-param name="groupId">pageNavigation</xsl:with-param>
                  </xsl:call-template>
                  <hr class="accessibilityElement" />
                </xsl:if>
                <div id="pageContent">
                  <a name="jump-content"><xsl:text> </xsl:text></a>
                  <xsl:call-template name="box-group">
                    <xsl:with-param name="boxes" select="boxes/box[@group = 'ariadne']"/>
                    <xsl:with-param name="groupId">ariadne</xsl:with-param>
                  </xsl:call-template>
                  <xsl:call-template name="box-group">
                    <xsl:with-param name="boxes" select="boxes/box[@group = 'before-content']"/>
                    <xsl:with-param name="groupId">beforeContent</xsl:with-param>
                  </xsl:call-template>

                  <div id="content">
                    <xsl:call-template name="content-area"/>
                    <xsl:call-template name="float-fix"/>
                  </div>

                  <xsl:call-template name="box-group">
                    <xsl:with-param name="boxes" select="boxes/box[@group = 'after-content']"/>
                    <xsl:with-param name="groupId">afterContent</xsl:with-param>
                  </xsl:call-template>

                  <xsl:call-template name="accessibility-jump-to-top" />
                  <xsl:call-template name="float-fix"/>
                </div>
                <xsl:if test="$hasAdditional">
                  <xsl:call-template name="box-group">
                    <xsl:with-param name="boxes" select="boxes/box[@group = 'additional']"/>
                    <xsl:with-param name="groupId">pageAdditional</xsl:with-param>
                  </xsl:call-template>
                  <xsl:call-template name="accessibility-jump-to-top" />
                </xsl:if>
                <xsl:call-template name="float-fix"/>
              </div>
            </div>
          </div>
          <xsl:call-template name="float-fix"/>
        </div>
        <div class="bottomBorder"><div><xsl:comment> /page </xsl:comment></div></div>
      </div>
    ...
    ~~~~

    `<div class="pageBackground">`: Alle Boxen aus der Boxgruppe `navigation` werden mit dem generischen Template `box-group` in die Seitenausgabe eingefügt, siehe [Templates und Parameter in ./html/base/base.xsl](/Templates_und_Parameter_in_./html/base/base.xsl ). Dies erfolgt allerdings nur dann, wenn die genannte Boxgruppe entsprechende Boxen enthält. `<div id="pageContent">`: Enthält die Box mit der Brotkrümelnavigation (Boxgruppe `ariadne` ) sowie alle Boxen, die über dem Content-Bereich (Boxgruppe `before-content` ) dargestellt werden sollen. Die Boxen werden über das Templa `box-group` eingebunden, siehe [Templates und Parameter in ./html/base/base.xsl](/Templates_und_Parameter_in_./html/base/base.xsl ). `<div id="content">`: Dieses Element enthält den zentralen Content-Bereich der Seite. In diesem Element wird das Template content-area aufgerufen, das die Inhalte des Seitenmoduls einfügt, siehe [Templates und Parameter in ./html/base/base.xsl](/Templates_und_Parameter_in_./html/base/base.xsl ). Unterhalb des `content` -Elements werden Boxen aus der Boxgruppe `after-content` eingefügt. Das `<div>` -Element mit der ID `pageContent` wird anschließend abgeschlossen. Anschließend wird in einem <xsl:if>-Element getestet, ob die Variable `$hasAdditional` den Wahrheitswert `true` besitzt. Ist dies der Fall, werden die Boxen für die rechte Spalte ( `additional` ) ausgegeben.

9.  Im letzten Schritt wird das HTML für die Fußzeile der Seite ausgegeben: **HTML-Grundgerüst einfügen (Fußzeile)**
    ~~~~ {.xml}
    ...
        <xsl:call-template name="footer" />
        <xsl:call-template name="float-fix"/>
        <xsl:call-template name="page-scripts-lazy" />
      </body>
    </html>
    ~~~~

    Das restliche HTML wird ausschließlich durch weitere Templates erzeugt: Das Template `footer` gibt die Fußleiste der Seite aus. Näheres zu diesem Template erfahren Sie in [Templates in ./html/base/defaults.xsl](/Templates_in_./html/base/defaults.xsl ). Das Template `float-fix`, das an mehreren Stellen ausgegeben wird, gibt ein speziell formatiertes Element aus, das den Textumfluss auf `none` stellt. Mit dem Template `page-scripts-lazy` werden alle JavaScript-Dateien in die Seite eingebunden, die durch andere Seitenmodule eingebunden worden sind. Näheres zu diesem Template erfahen Sie in [Templates in ./html/base/defaults.xsl](/Templates_in_./html/base/defaults.xsl ).

10. Die komplette Stylesheetdatei sieht wie folgt aus: **Die komplette Stylesheetdatei page_main.xsl**
    ~~~~ {.xml}
    <?xml version="1.0"?>
    <xsl:stylesheet version="1.0"
                    xmlns:xsl="http://www.w3.org/1999/XSL/Transform"
                    ns="http://www.w3.org/1999/xhtml">
    <!--
      @papaya:modules content_imgtopic, content_errorpage,
                      content_categimg, content_tagcateg, content_xhtml
    -->

    <!-- default templates to use and maybe overload -->
    <xsl:import href="./base/defaults.xsl" />

    <xsl:output method="xml" encoding="utf-8" standalone="no"
                doctype-public="-//W3C//DTD XHTML 1.0 Transitional//EN"
                doctype-system="http://www.w3.org/TR/xhtml1/DTD/xhtml1-transitional.dtd"
                indent="yes"
                omit-xml-declaration="yes" />

    <!--
      papaya CMS parameters
    -->

    <!-- page title (like in navigations) -->
    <xsl:param name="PAGE_TITLE" />
    <!-- content language (example: en-US) -->
    <xsl:param name="PAGE_LANGUAGE"></xsl:param>
    <!-- base installation path in browser -->
    <xsl:param name="PAGE_WEB_PATH" />
    <!-- url of this page -->
    <xsl:param name="PAGE_URL" />
    <!-- theme name -->
    <xsl:param name="PAGE_THEME" />
    <!-- theme path in browser -->
    <xsl:param name="PAGE_THEME_PATH" />
    <!-- theme path in server file system -->
    <xsl:param name="PAGE_THEME_PATH_LOCAL" />

    <!-- current ouput mode (file extension in url) -->
    <xsl:param name="PAGE_OUTPUTMODE_CURRENT" />
    <!-- default/main ouput mode -->
    <xsl:param name="PAGE_OUTPUTMODE_DEFAULT" />
    <!-- public or preview page? -->
    <xsl:param name="PAGE_MODE_PUBLIC" />
    <!-- website version string if available -->
    <xsl:param name="PAGE_WEBSITE_REVISION" />

    <!-- papaya cms version string if available -->
    <xsl:param name="PAPAYA_VERSION" />
    <!-- installation in dev mode? (option in conf.inc.php) -->
    <xsl:param name="PAPAYA_DBG_DEVMODE" />

    <!-- current local time and offset from UTC -->
    <xsl:param name="SYSTEM_TIME" />
    <xsl:param name="SYSTEM_TIME_OFFSET" />

    <!--
      template set parameters
    -->

    <!-- use favicon.ico in theme directory -->
    <xsl:param name="FAVORITE_ICON" select="true()" />

    <!-- IE only, disable the mouseover image toolbar, default: true -->
    <xsl:param name="IE_DISABLE_IMAGE_TOOLBAR" select="true()" />
    <!-- IE only, disable the smart tag linking, default: true -->
    <xsl:param name="IE_DISABLE_SMARTTAGS" select="true()" />
    <!-- IE only, optional user agent compatibility definition, default: not used -->
    <xsl:param name="USER_AGENT_COMPATIBILITY"></xsl:param>

    <!-- define Indexing for robots -->
    <xsl:param name="PAGE_META_ROBOTS">index,follow</xsl:param>

    <!-- add css classes to boxes based on the module class name -->
    <xsl:param name="BOX_MODULE_CSSCLASSES" select="false()" />
    <!-- load file containing module specific css files name definitions -->
    <xsl:param name="BOX_MODULE_CSSFILES" select="document('boxes.xml')" />
    <!-- do not index box output (puts noindex comments around it) -->
    <xsl:param name="BOX_DISABLE_INDEX" select="true()" />

    <!-- disable the navigation column, even if the xml contains boxes for it -->
    <xsl:param name="DISABLE_NAVIGATION_COLUMN" select="false()" />
    <!-- disable the additional content column, even if the xml contains boxes for it -->
    <xsl:param name="DISABLE_ADDITIONAL_COLUMN" select="false()" />

    <!-- maximum columns for multiple column outputs of items (like subtopics) -->
    <xsl:param name="MULTIPLE_COLUMNS_MAXIMUM" select="3" />

    <!-- use international date time formatting, ISO 8601 -->
    <xsl:param name="DATETIME_USE_ISO8601" select="false()" />
    <!-- char between date and time (ISO 8601 = T, default =  ) -->
    <xsl:param name="DATETIME_SEPARATOR"> </xsl:param>
    <!-- default date time format: short, medium or large -->
    <xsl:param name="DATETIME_DEFAULT_FORMAT">short</xsl:param>

    <!-- load current language texts -->
    <xsl:param name="LANGUAGE_TEXTS_CURRENT" select="document(concat($PAGE_LANGUAGE, '.xml'))" />
    <!-- load fallback language texts -->
    <xsl:param name="LANGUAGE_TEXTS_FALLBACK" select="document('en-US.xml')" />

    <!--
      template definitions
    -->

    <!-- call the page template for the root tag -->
    <xsl:template match="/page">
      <xsl:call-template name="page" />
    </xsl:template>

    <xsl:template name="page">
      <html lang="{$PAGE_LANGUAGE}">
        <head>
          <xsl:call-template name="html-head" />
        </head>
        <body>
          <xsl:call-template name="accessibility-navigation" />
          <xsl:call-template name="header" />

          <xsl:variable name="hasNavigation"
                        select="not($DISABLE_NAVIGATION_COLUMN) and
                               (count(boxes/box[@group = 'navigation']) > 0)" />
          <xsl:variable name="hasAdditional"
                        select="not($DISABLE_ADDITIONAL_COLUMN) and
                               (count(boxes/box[@group = 'additional']) > 0)" />

          <div id="page" class="outerCorners">
            <div class="topCorners"><div><xsl:comment> page </xsl:comment></div></div>
            <div class="leftBorder">
              <div class="rightBorder">
                <div>
                  <xsl:attribute name="class">
                    <xsl:choose>
                      <xsl:when test="$hasNavigation and $hasAdditional">threeColumnLayout</xsl:when>
                      <xsl:when test="$hasNavigation">twoColumnLayoutLeft</xsl:when>
                      <xsl:when test="$hasAdditional">twoColumnLayoutRight</xsl:when>
                      <xsl:otherwise>singleColumnLayout</xsl:otherwise>
                    </xsl:choose>
                  </xsl:attribute>
                  <div class="pageBackground">
                    <xsl:if test="$hasNavigation">
                      <a name="jump-navigation"><xsl:text> </xsl:text></a>
                      <xsl:call-template name="box-group">
                        <xsl:with-param name="boxes" select="boxes/box[@group = 'navigation']"/>
                        <xsl:with-param name="groupId">pageNavigation</xsl:with-param>
                      </xsl:call-template>
                      <hr class="accessibilityElement" />
                    </xsl:if>
                    <div id="pageContent">
                      <a name="jump-content"><xsl:text> </xsl:text></a>
                      <xsl:call-template name="box-group">
                        <xsl:with-param name="boxes" select="boxes/box[@group = 'ariadne']"/>
                        <xsl:with-param name="groupId">ariadne</xsl:with-param>
                      </xsl:call-template>
                      <xsl:call-template name="box-group">
                        <xsl:with-param name="boxes" select="boxes/box[@group = 'before-content']"/>
                        <xsl:with-param name="groupId">beforeContent</xsl:with-param>
                      </xsl:call-template>

                      <div id="content">
                        <xsl:call-template name="content-area"/>
                        <xsl:call-template name="float-fix"/>
                      </div>

                      <xsl:call-template name="box-group">
                        <xsl:with-param name="boxes" select="boxes/box[@group = 'after-content']"/>
                        <xsl:with-param name="groupId">afterContent</xsl:with-param>
                      </xsl:call-template>

                      <xsl:call-template name="accessibility-jump-to-top" />
                      <xsl:call-template name="float-fix"/>
                    </div>
                    <xsl:if test="$hasAdditional">
                      <xsl:call-template name="box-group">
                        <xsl:with-param name="boxes" select="boxes/box[@group = 'additional']"/>
                        <xsl:with-param name="groupId">pageAdditional</xsl:with-param>
                      </xsl:call-template>
                      <xsl:call-template name="accessibility-jump-to-top" />
                    </xsl:if>
                    <xsl:call-template name="float-fix"/>
                  </div>
                </div>
              </div>
              <xsl:call-template name="float-fix"/>
            </div>
            <div class="bottomBorder"><div><xsl:comment> /page </xsl:comment></div></div>
          </div>
          <xsl:call-template name="footer" />
          <xsl:call-template name="float-fix"/>
          <xsl:call-template name="page-scripts-lazy" />
        </body>
      </html>
    </xsl:template>

    </xsl:stylesheet>
    ~~~~

Sie können nun für jedes andere Boxmodul so vorgehen, wie dies im obigen Beispiel beschrieben worden ist.

[Kategorie:Implementierungsphase: Webseitenvorlage erstellen](/Kategorie:Implementierungsphase:_Webseitenvorlage_erstellen )