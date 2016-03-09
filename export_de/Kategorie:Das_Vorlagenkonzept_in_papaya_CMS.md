---
title: Kategorie:Das Vorlagenkonzept in papaya CMS
permalink: Kategorie:Das_Vorlagenkonzept_in_papaya_CMS/
---

Eine Webseitenvorlage in papaya CMS setzt sich aus Theme und Template zusammen. Ein Theme besteht aus CSS-Dateien und Layout-Grafiken, während ein Template aus mehreren XSLT-Dateien besteht. Das Template dient dazu, das von den Modulen generierte XML in das HTML-Format zu transformieren. Mit dem Theme erhält dieses HTML eine ansprechende grafische Gestaltung.

Template und Theme unterscheiden sich noch in einem anderen Bereich. Während das Template ausschließlich durch die Template-Engine von papaya CMS benutzt wird, muss der Browser auf die CSS-Dateien und Grafiken aus dem Theme zugreifen können. Das bedeutet, dass das Theme im DocumentRoot liegen muss, während das Template außerhalb des DocumentRoot untergebracht werden kann. Auch wenn dies technisch nicht notwendig ist, hat es aus Sicherheitsgründen Sinn, das Template außerhalb des DocumentRoot zu legen.

Des Weiteren kann die Webseitenvorlage auch noch durch eine clientseitige Scriptbibliothek ergänzt werden. Eine Webseitenvorlage ist also immer ein Template/Theme-Set, in dem ein Template mit einem dazu passenden Theme und einer optionalen Scriptbibliothek zusammengestellt wird.

[Kategorie:Templates und Themes entwickeln](export_de/Kategorie:Templates_und_Themes_entwickeln.md)