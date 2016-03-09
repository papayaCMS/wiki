---
title: Module Development 5: Refactoring and Miscellania
permalink: /Module_Development_5:_Refactoring_and_Miscellania/
---

In this final part of the tutorial series, we will do some refactoring to get rid of double implementations. After everything is in its place, we will add test suite files to control execution of all unit tests and to measure code coverage.

Please note that you should adhere to the [Papaya CMS Coding Standards](/Papaya_CMS_Coding_Standards.md), especially if you plan to contribute your modules for the papaya Community.

This tutorial makes use of unit tests which is a highly recommended way to build software. In order to have a papaya CMS version that includes the PHPUnit framework and the PapayaTestCase class, you need to check out papaya CMS from the SVN repository. Instructions on how to obtain it can be found [here](http://www.papaya-cms.com/download.990.en.html#svn).

Overview of files and directories
---------------------------------

After finishing this tutorial, you will have created the following files and subdirectories in *papaya-lib/modules/special/myproject/tutorial* (new files from the current part are bold):

`+ [DATA]`
`|  |`
`|  + table_tutorial_planets.xml`
`|`
`+ edmodule_tutorial_hello.php`
`|`
`+ [Hello]`
`|  |`
`|  + Connector.php`
`|  |`
`|  + `**`Output.php`**
`|  |`
`|  + [Page]`
`|  |  |`
`|  |  + Base.php`
`|  |`
`|  + Page.php`
`|  |`
`|  + [Planet]`
`|  |  |`
`|  |  + Admin.php`
`|  |  |`
`|  |  + [Database]`
`|  |  |  |`
`|  |  |  + Access.php`
`|  |  |`
`|  |  + [Rating]`
`|  |  |  |`
`|  |  |  + [Box]`
`|  |  |  |  |`
`|  |  |  |  + Base.php`
`|  |  |  |`
`|  |  |  + Box.php`
`|  |  |`
`|  |  + Rating.php`
`|  |`
`|  + Planet.php`
`|`
`+ modules.xml`
`---------------------------------------`
`papaya-data/templates/default-xhtml/html/box_planet_rating.xsl`
` `***`Note`**`:` `Names` `in` `square` `brackets` `indicate` `directories`*

In the *testing/tests-unittests/papaya-lib/modules/special/myproject/tutorial* directory, you will have created the following structure (new files from the current part are bold):

`+ `**`AllTests.php`**
`|`
`+ [Hello]`
`   |`
`   + `**`AllTests.php`**
`   |`
`   + ConnectorTest.php`
`   |`
`   + `**`OutputTest.php`**
`   |`
`   + [Page]`
`   |  |`
`   |  + `*`AllTests.php`*
`   |  |`
`   |  + BaseTest.php`
`   |`
`   + [Planet]`
`   |  |`
`   |  + AdminTest.php`
`   |  |`
`   |  + `**`AllTests.php`**
`   |  |`
`   |  + [Database]`
`   |  |  |`
`   |  |  + AccessTest.php`
`   |  |  |`
`   |  |  + `**`AllTests.php`**
`   |  |`
`   |  + [Rating]`
`   |  |  |`
`   |  |  + `**`AllTests.php`**
`   |  |  |`
`   |  |  + [Box]`
`   |  |  |  |`
`   |  |  |  + `**`AllTests.php`**
`   |  |  |  |`
`   |  |  |  + BaseTest.php`
`   |  |  |`
`   |  |  + BoxTest.php`
`   |  |`
`   |  + RatingTest.php`
`   |`
`   + PlanetTest.php`
`   |`
`   + BaseTest.php`
` `***`Note`**`:` `Names` `in` `square` `brackets` `indicate` `directories`*