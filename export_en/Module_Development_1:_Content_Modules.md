This tutorial describes how to write a simple module or plugin for [papaya CMS](/papaya_CMS.md).

Getting started


The first module we want to write is a simple page module that outputs the famous "Hello World" message. It does not need database access and only consists of a single file.

### Terminology: module vs. module package

If you explore the *modules* subdirectory in the *papaya-lib* directory, you will discover a few subdirectories (which are explained in detail in the next subsection). Each of these subdirectories contains either files or other subdirectories. A directory like *modules/_base/community* is called a **module package**: Most of the files in it share the same database tables and cooperate to provide a certain service; the community package, e.g., handles frontend user-logins, user profiles, contacts, etc.

A single file within a package is called a **module**. It provides one aspect of the package's purpose. The community package, for instance, contains a file called *content_login.php* which is a standard login page, or *box_login.php*, a login box to be included into other pages.

You can explore all available packages and their modules by clicking on *Modules* in the main toolbar of the papaya CMS backend. A sidebar on the left contains all packages. When you click on one of them, the middle column of the page will show the package's modules (and its database tables). If you click on a module, the right column will show information about the module and allow you to control some of its settings.

### Selecting your directory

If you look around in the papaya-lib/modules directory of your papaya installation, you will discover *at least* the following subdirectories:

-   *_base* -- basic modules like simple HTML box or all of the community stuff
-   *free* -- free modules under the GNU GPL
-   *gpl* -- free modules depending on third-party GPL software
-   *sample* -- a very simple sample module similar to the one we are going to develop in this section

If you happen to buy one or more commercial papaya modules, they will be stored in an additional subdirectory called *commercial*.

If you write a module that only suits the purposes of your specific project, create a new subdirectory called *special*, and within it another subdirectory whose name reflects your project. Modules for a less specific purpose should be stored in the *free* directory (or in a *beta* directory while in development), and you might eventually decide to give those modules to the papaya community.

After you have selected or created the directory you want your module to be stored in, create a subdirectory of this one called *tutorial*.

### Preparing the *modules.xml* file

Each module package needs a module information file called *modules.xml*. This file lists the modules and the database tables within this package, and by scanning the module directories recursively for these files, papaya CMS will know about new, changed, or deleted modules.

Here is a short sample for a *modules.xml* file, taken from the *_base/countries* module package:

~~~~ {.xml}
<?xml version="1.0"  encoding="ISO-8859-1" ?>
<modulegroup>
  <name>Countries</name>
  <description>
    The country package provides backend functionality to manage continents and
    countries as well as a connector with form callback functions.
  </description>
  <modules>
    <module type="page"
            guid="fd53aef2d8bb7cb4637a64dabaf7b424"
            name="State List XML"
            class="content_statelist"
            file="content_statelist.php">
      Returns an XML list of states for a specific country, to be used for Ajax
    </module>
    <module type="admin"
            guid="bf6e40b71d3cfb0e80682c64b11d33af"
            name="Countries"
            class="edmodule_countries"
            file="edmodule_countries.php"
            glyph="countries.png">
      The administration module provides the facility to manage continents,
      countries, and their localized names.
    </module>
    <module type="connector"
            guid="99db2c2898403880e1ddeeebf7ee726c"
            name="Country Connector"
            class="connector_countries"
            file="connector_countries.php">
      Country Connector
    </module>
  </modules>
  <tables>
    <table name="continents"/>
    <table name="countries"/>
    <table name="countrynames"/>
    <table name="states"/>
    <table name="countries_old"/>
    <table name="states_old"/>
  </tables>
</modulegroup>
~~~~

The *modulegroup* root element encloses all the content. The *name* and *description* elements only contain plain text -- the package's name and abstract, respectively. The *modules* section contains a *module* element for each of the package's modules, while *tables* has got a *table* entry for each database table.

A *module* element consists of five or more attributes and an enclosed description in plain text. The attributes are defined as follows:

-   *type* -- the module type, e.g. "page" for a page module or "box" for a box module
-   *guid* -- a unique identifier for the module: a string containing a hexadecimal 128-bit number
-   *name* -- the module's name to be displayed in the papaya backend
-   *class* -- real name of the PHP class that declares the module
-   *file* -- the file containing the module (optionally, you can provide relative paths for files in subdirectories)
-   *glyph* -- the name of an icon file for the module (only for *admin* type modules as they are used to manage the whole package)
-   *outputfilter* -- an optional attribute for content modules (types *page* or *box*): If you set this attribute to the value "no", it will not use an output filter to create its final output (the concept of output filters is explained more detailed in \#\#papaya_architecture\#\#)

The *table* elements in the *tables* section only contain a *name* attribute. The table structures themselves are stored in the *DATA* subdirectory of each module package directory. These files should *never* be written manually; the next part of this tutorial will show you how to generate them from the papaya backend.

For now, we need a fresh *modules.xml* file for our intended package (that will only contain a single module and no database tables). Open your preferred text or XML editor and type (or copy and paste) the following:

~~~~ {.xml}
<?xml version="1.0" encoding="utf-8" ?>
<modulegroup>
  <modules>
    <module type="page"
            guid=""
            name="Hello World Page"
            class="HelloPage"
            file="Hello/Page.php">
      This simple page module displays a Hello World message
    </module>
  </modules>
</modulegroup>
~~~~

Please note that we have left the *guid* attribute blank for now. But obviously we need a value because the module will be registered using this identifier. You can either use the [GUID Generator](http://community.papaya-cms.com/guid), or write your own simple PHP code (most other languages would do, as well) such as the following:

~~~~ {.php}
<?php

echo md5(microtime());

?>
~~~~

Now paste your new hash value in between the quotes of the *guid* attribute and save the file.

Writing the PHP code

There is a difference in the recommended directory and file name structure for papaya modules between older, PHP-4-compatible parts and new PHP-5-only packages. This is because we want to use *unit testing* for the new modules. If you have never heard about unit testing in general or PHPUnit in particular, never mind -- everything necessary will be explained along the way. One of the best resources for getting started with unit testing, though, is the article [Test Infected: Programmers Love Writing Tests](http://junit.sourceforge.net/doc/testinfected/testing.htm) by *Erich Gamma* and *Kent Beck*. For detailed information about PHPUnit, visit the official [PHPUnit site](http://www.phpunit.de/).

For this project, we are even going to use the *test-first* approach: Write a unit test for each part of your code before you write the actual code.

Before we start writing the actual code, we are going to create the basic directory structure.

Just create the following directories in your selected location: + tutorial [you already created this one, containing the modules.xml file] | +--+ Hello | +--+ Page Now find the *testing/test-unittests* directory of your papaya installation. It should already contain a *papaya-lib/modules* subpath (if not, just create it). Next up, create the same nested structure of *tutorial/Hello/Page* directories in the subdirectories of the testing environment that reflects the structure's location in the module hierarchy.

### Top-down and test-driven: a static content module

Test-driven, object-oriented code should be written using a top-down approach: You start with a static implementation of what the user will see on screen and implement the underlying logic later. This approach guarantees that each part of the code is independent and can be flexibly exchanged by another implementation at any given time.

The first part to be implemented is the page module itself. Create an empty PHP file called *Hello.php* in the *tutorial* directory, and another empty PHP file called *HelloTest.php* in the *tutorial* directory in the unit testing directory tree.

Following the test-first approach, you should prepare the unit test class file and write the first test before writing any implementation code. A PHPUnit test case class extends the *PHPUnit_Framework_TestCase* base class. For the specific needs of papaya CMS unit tests, there's already a *PapayaTestCase* class you can extend, it's located in the *Framework* subdirectory of the *testing/tests-unittests* path.

