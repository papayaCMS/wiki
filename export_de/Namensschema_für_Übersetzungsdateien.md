---
title: Namensschema für Übersetzungsdateien
permalink: /Namensschema_für_Übersetzungsdateien/
---

Der Dateiname setzt sich aus einem Sprachkürzel sowie der Endung `.xml` zusammen. Das Sprachkürzel folgt dem Schema, das in ISO 639-1 festgelegt worden ist. ISO 639-1 geben dabei zwei Zeichen vor, die für die Sprache stehen.

Um zusätzlich noch die Region zu kodieren, in der eine spezielle Variante der Sprache gesprochen wird, kann das zwei Zeichen lange Sprachkürzel (Alpha-2) optional durch weitere zwei Zeichen erweitert werden, die die jeweilige Region kodieren. Diese regionalen oder geografischen Codes werden in ISO 3166 definiert. Die Kombination von Sprach- und Regionalcode wird darüber hinaus in RFC 4646 besprochen. Diesem RFC folgend werden Sprachcode und Regionalcode im Dateinamen durch Bindestrich voneinander getrennt.

Die folgende Liste stellt eine Reihe von Beispielen für Dateinamen nach dem oben genannten Schema vor:

1.  `de.xml`, `en.xml`
2.  `de-DE.xml`, `de-CH.xml`, `en-US.xml`

[export_de/Kategorie.md:Mit Übersetzungsdateien arbeiten](export_de/Kategorie.md:Mit_Übersetzungsdateien_arbeiten )