
In this second part of the basic module development tutorial, we want to take a look at how to add database support to our module package. If you are new to papaya module programming, read the first part [here](Module_Development_1:_Content_Modules.md).

Please note that you should adhere to the [Papaya CMS Coding Standards](Papaya_CMS_Coding_Standards.md), especially if you plan to contribute your modules for the papaya Community.

Preparing and writing the database access class

Most web applications use a database to load content from or to store data in. papaya CMS includes its own database access interface that we are going to explore in this section of the tutorial. Our goal is to replace "World" in the "Hello World" headline by a selected planet loaded from a database table.

### Setting up the database table

The first step towards database support in a module package is to define the database table or tables you want to use. The table of planets we are using for our module only consists of two columns:

-------------|---------------|---------
`Column      | SQL data type | Remarks`
`planet_id   | int           | Primary key to identify a planet`
`planet_name | varchar(30)   | Name of the planet`

You can use your usual database administration tool to create this table. Just select the database of your papaya CMS installation and execute the following query to create the table (please note that this is a MySQL query that has to be adjusted for other databases supported by papaya):

~~~~ {.sql}
CREATE TABLE papaya_tutorial_planets (
  planet_id int AUTO_INCREMENT,
  planet_name varchar(30),
  PRIMARY KEY (planet_id)
)
~~~~

Please note that if you have chosen another table prefix than *papaya* for your papaya installation, you need to use this in the table name.

Next, you need to create the table information XML file. Add a line to the *tables* section in your *modules.xml* file (or add it in the first place, underneath the closing *</modules>* tag) to make it look like this:

~~~~ {.xml}
  <tables>
    <table name="tutorial_planets" />
  </tables>
~~~~

Go to the *Modules* section of the papaya backend and click the *Search modules* button. After the scan, click onto the *Hello World tutorial module package*. In the *Package content* box, there is a *Tables* section now. Click the plus sign to open it, and then select the *papaya_tutorial_planets* table. You get the error message *Could not load table structure file!* Click on *Export table* in the subtoolbar of the Modules sesction, and the XML file for the table is created and saved on your computer.

The contents of the file look like this (remember to *never* write or edit these files manually; always use the Export functionality explained above):

~~~~ {.xml}
<?xml version="1.0" encoding="ISO-8859-1" ?>
<table name="tutorial_planets" prefix="yes">
  <fields>
    <field name="planet_id" type="integer" size="4" null="no" autoinc="yes"/>
    <field name="planet_name" type="string" size="30" null="yes"/>
  </fields>
  <keys>
    <primary-key>
      <field>planet_id</field>
    </primary-key>
  </keys>
</table>
~~~~

You need to remove the date from the filename (before: something like *table_tutorial_planets_2010-04-30.xml*, after: *table_tutorial_planets.xml*). Then save this file in a subdirectory of *tutorial* called *DATA*. Click on the table again in the papaya backend, and the table structure should show up. You have to repeat this whole procedure whenever you add or change tables.

### Creating a class to delegate database access

Before we write the real low-level database access class, we are going to implement a class that models a Planet object. Following our top-down approach, the first implementation of this class is static. Later, when we implement the database access class, we are going to modify that class: the static implementation will be replaced by calls to the database class. This approach helps, once again, to implement the public interface before writing the internal logic.

Of course, you should follow the test-first approach again, implementing one test and the corresponding method at a time. But as you already know this concept, here's the complete test class (to be saved in *tutorial/PlanetTest.php* in your testing directory):

~~~~ {.php}
<?php

require_once(substr(dirname(__FILE__), 0, -49).'/Framework/PapayaTestCase.php');
require_once(PAPAYA_INCLUDE_PATH.'modules/special/myproject/tutorial/Planet.php');

class PlanetTest extends PapayaTestCase {
  /**
  * Get the Planet object to be tested
  *
  * @return Planet
  */
  private function getPlanetObjectFixture() {
    return new Planet();
  }

  /**
  * @covers Planet::getById {
  */
  public function testGetById() {
    $planetObject = $this->getPlanetObjectFixture();
    $expected = array('planet_id' => 1, 'planet_name' => 'Mars');
    $this->assertEquals($expected, $planetObject->getById(1));
  }

  /**
  * @covers Planet::getAll
  */
  public function testGetAll() {
    $planetObject = $this->getPlanetObjectFixture();
    $expected = array(1 => 'Mars', 2 => 'Jupiter', 3 => 'Saturn');
    $this->assertEquals($expected, $planetObject->getAll());
  }

  /**
  * @covers Planet::create {
  */
  public function testCreate() {
    $planetObject = $this->getPlanetObjectFixture();
    $data = array('planet_name' => 'Jupiter');
    $this->assertTrue($planetObject->create($data));
  }

  /**
  * @covers Planet::update
  */
  public function testUpdate() {
    $planetObject = $this->getPlanetObjectFixture();
    $data = array('planet_name' => 'Saturn');
    $this->assertTrue($planetObject->update(3, $data));
  }

  /**
  * @covers Planet::delete {
  */
  public function testDelete() {
    $planetObject = $this->getPlanetObjectFixture();
    $this->assertTrue($planetObject->delete(2));
  }
}

?>
~~~~

As you can see from the test methods, this class provides the public interface to classic database access methods (the so-called *CRUD* methods for *C*reate, *R*ead, *U*pdate, and *D*elete). Here's the static implementation of the class -- save it as *tutorial/Planet.php* in your module package directory:

~~~~ {.php}
<?php

/**
* Hello World tutorial, Planet class
*
* The Planet class provides the public interface for planets to be stored in
* and loaded from database.
*
* @package Papaya-Modules
* @subpackage tutorial
*/

/**
* Hello World tutorial, Planet class
*
* @package Papaya-Modules
* @subpackage tutorial
*/
class Planet {
  /**
  * Get a planet by id
  *
  * @param integer $planetId id of the planet to load
  * @return mixed array planet data if id exists, FALSE otherwise
  */
  public function getById($planetId) {
    return array('planet_id' => 1, 'planet_name' => 'Mars');
  }

  /**
  * Get all planets
  *
  * @return mixed array planet data if planets exist, FALSE otherwise
  */
  public function getAll() {
    return array(1 => 'Mars', 2 => 'Jupiter', 3 => 'Saturn');
  }

  /**
  * Add a new planet to the database
  *
  * @param array $data planet data
  * @return boolean TRUE on success, FALSE otherwise
  */
  public function create($data) {
    return TRUE;
  }

  /**
  * Modify an existing planet
  *
  * @param integer $planetId id of the planet to modify
  * @param array $data modified planet data
  * @return boolean TRUE on success, FALSE otherwise
  */
  public function update($planetId, $data) {
    return TRUE;
  }

  /**
  * Delete a specific planet by id
  *
  * @param integer $planetId
  * @return boolean TRUE on success, FALSE otherwise
  */
  public function delete($planetId) {
    return TRUE;
  }
}

?>
~~~~

### A primer on papaya database access

Next, we are going to implement the database access class itself. Database access is provided by the *PapayaDatabaseObject* class (from the file *Object.php* in *papaya-lib/system/Papaya/Database*). You can extend this class for your own database access classes; amongst others, it provides the following methods:

-   *databaseGetTableName(\$name, \$usePrefix)* gets the name of a database table. Usually, the boolean *\$usePrefix* is set to TRUE, which adds the value of the *PAPAYA_DB_TABLEPREFIX* configuration constant to the base name. As the prefix can be modified in papaya's *conf.inc.php* configuration file, this is a safe way to always use the correct table names.
-   *databaseQueryFmt(\$sql, \$params, \$limit = NULL, \$offset = NULL)* executes the SQL query given in the string *\$sql*. *\$sql* is a printf-style format string, while \$params is an array of values you want to insert insert instead of the format placeholders. You should provide the database table names and all dynamic values using these placeholders. The optional *\$limit* and *\$offset* parameters limit the number of result records, starting at the given offset (which is 0 by default, i.e. the first record in the order defined by the query). This method is read-only, so it's only suitable for SELECT queries.
-   *databaseQueryFmtWrite(\$sql, \$params, \$limit = NULL, \$offset = NULL)* is the read-and-write version of *databaseQueryFmt(.md)*. You only need to use it for really complex modification queries; in most cases, you can get along with the methods described below.
-   *databaseInsertRecord(\$table, \$keyField = NULL, \$data)* is used to insert a record into a table. The first argument is the table; the second is the optional name of the primary key field (only use it in case of auto-increment, integer primary keys; it makes the method return the index of the new record in case of success). The third argument is an associative array of data you want to insert into the table; the keys are the field names. The return value is FALSE in case of an error, otherwise it depends on whether you've used a primary key name or not. To test for success, compare the value to FALSE using strict identity (===) instead of simple equality (==).
-   *databaseInsertRecords(\$table, \$data)* is a multi-record variant of *databaseInsertRecord*. The *\$table* argument is the table name, while *\$data* is an array of associative arrays, each one including a record as described for the previous method. The return value is FALSE on error or the number of affected rows on success.
-   *databaseUpdateRecord(\$table, \$data, \$field, \$value = NULL)* updates records matching a certain condition. You provide the table name, an array of data as described for *databaseInsertRecord(.md)* above, and then the condition, either as a single field and a single value, or as an associative array of field-value pairs. The return value is FALSE in case of an error or the number of affected rows on success (0 rows may still be success, so remember to check the return value using ===).
-   *databaseDeleteRecord(\$table, \$field, \$value = NULL)* deletes records matching a condition; the *\$field* parameter, the optional *\$value* parameter, and the return value work just like for *databaseUpdateRecord(.md)*.
-   *databaseGetSQLCondition(\$field, \$value = NULL)* creates a condition to be used in *databaseQueryFmt(.md)* or *databaseQueryFmtWrite(.md)*. Like with *databaseUpdateRecord(.md)*, you can either use a single field and a value (which might as well be an array of values resulting in a condition matching any of the provided values), or an associative array with field-value pairs. Usually, multiple field-value conditions are combined using the AND operator, but you can add the string 'OR' as an array value without a dedicated key between two field-value pairs to use OR instead.

### Unit-testing the database access class

In order to unit-test a class derived from *PapayaDatabaseObject*, you can use its *setDatabaseAccess(.md)* method to replace papaya's database implementation by a mock object. We have used this technique in the previous tutorial to mock our own base class. This time, though, we do not require any real class to model the database access mock object after, but we use a generic mock object and add the method names to use. In the *PapayaDatabaseAcess* class from the *papaya-lib/system/Papaya/Database* directory, these methods share the names of the above mentioned methods without the 'database' prefix, starting with a lowercase letter, for example *queryFmt(.md)* or *deleteRecord(.md)*.

Here's the unit test class for our database access class, to be saved in *tutorial/Planet/Database/AccessTest.php* in the unit test directory structure:

~~~~ {.php}
<?php
require_once(substr(dirname(__FILE__), 0, -65).'/Framework/PapayaTestCase.php');
PapayaTestCase::registerPapayaAutoloader();
require_once(PAPAYA_INCLUDE_PATH.'modules/special/myproject/tutorial/Planet/Database/Access.php');

class PlanetDatabaseAccessTest extends PapayaTestCase {
  /**
  * Get the PlanetDatabaseAccess object to be tested
  *
  * @return PlanetDatabaseAccess
  */
  private function getPlanetDatabaseAccessObjectFixture() {
    return new PlanetDatabaseAccess();
  }

  /**
  * Get a PapayaDatabaseAccess mock object
  *
  * @param array $methods the methods to be modeled
  * @return PapayaDatabaseAccess mock object
  */
  private function getPapayaDatabaseAccessObjectFixture($methods) {
    if (!in_array('getTableName', $methods)) {
      $methods[] = 'getTableName';
    }
    $databaseAccessObject = $this->getMock(
      'PapayaDatabaseAccess',
      $methods,
      array(),
      'Mock_'.md5(__CLASS__.microtime()),
      FALSE
   .md);
    $databaseAccessObject
      ->expects($this->once())
      ->method('getTableName')
      ->with($this->equalTo('tutorial_planets'), $this->isTrue())
      ->will($this->returnValue('papaya_tutorial_planets'));
    return $databaseAccessObject;
  }

  /**
  * Get a database result mock object
  *
  * @param array $methods List of methods to be mocked
  * @return dbresult_mysql mock object
  */
  private function getDatabaseResultObjectFixture($methods) {
    if (!defined('DB_FETCHMODE_ASSOC')) {
      define('DB_FETCHMODE_ASSOC', 0);
    }
    return $this->getMock(
      'dbresult_mysql',
      $methods,
      array(),
      'Mock_'.md5(__CLASS__.microtime()),
      FALSE
   .md);
  }

  /**
  * @covers PlanetDatabaseAccess::getById
  */
  public function testGetById() {
    $planetDatabaseAccessObject = $this->getPlanetDatabaseAccessObjectFixture();
    $databaseAccessObject = $this->getPapayaDatabaseAccessObjectFixture(
      array('queryFmt')
   .md);
    $databaseResultObject = $this->getDatabaseResultObjectFixture(
      array('fetchRow')
   .md);
    $expectedData = array('planet_id' => 1, 'planet_name' => 'Mars');
    $databaseResultObject
      ->expects($this->once())
      ->method('fetchRow')
      ->will($this->returnValue($expectedData));
    $databaseAccessObject
      ->expects($this->once())
      ->method('queryFmt')
      ->will($this->returnValue($databaseResultObject));
    $planetDatabaseAccessObject->setDatabaseAccess($databaseAccessObject);
    $this->assertEquals($expectedData, $planetDatabaseAccessObject->getById(1));
  }

  /**
  * @covers PlanetDatabaseAccess::getAll
  */
  public function testGetAll() {
    $planetDatabaseAccessObject = $this->getPlanetDatabaseAccessObjectFixture();
    $databaseAccessObject = $this->getPapayaDatabaseAccessObjectFixture(
      array('queryFmt')
   .md);
    $databaseResultObject = $this->getDatabaseResultObjectFixture(
      array('fetchRow')
   .md);
    $expectedData = array(
      array('planet_id' => 1, 'planet_name' => 'Mars'),
      array('planet_id' => 2, 'planet_name' => 'Jupiter'),
      array('planet_id' => 3, 'planet_name' => 'Saturn')
   .md);
    $databaseResultObject
      ->expects($this->atLeastOnce())
      ->method('fetchRow')
      ->will(
          $this->onConsecutiveCalls(
            $this->returnValue($expectedData[0]),
            $this->returnValue($expectedData[1]),
            $this->returnValue($expectedData[2]),
            FALSE
         .md)
       .md);
    $databaseAccessObject
      ->expects($this->once())
      ->method('queryFmt')
      ->will($this->returnValue($databaseResultObject));
    $planetDatabaseAccessObject->setDatabaseAccess($databaseAccessObject);
    $expectedResult = array();
    foreach ($expectedData as $row) {
      $expectedResult[$row['planet_id']] = $row['planet_name'];
    }
    $this->assertEquals($expectedResult, $planetDatabaseAccessObject->getAll());
  }

  /**
  * @covers PlanetDatabaseAccess::create
  */
  public function testCreate() {
    $planetDatabaseAccessObject = $this->getPlanetDatabaseAccessObjectFixture();
    $databaseAccessObject = $this->getPapayaDatabaseAccessObjectFixture(
      array('insertRecord')
   .md);
    $databaseAccessObject
      ->expects($this->once())
      ->method('insertRecord')
      ->will($this->returnValue(1));
    $planetDatabaseAccessObject->setDatabaseAccess($databaseAccessObject);
    $this->assertTrue($planetDatabaseAccessObject->create(array('planet_name' => 'Mars')));
  }

  /**
  * @covers PlanetDatabaseAccess::update
  */
  public function testUpdate() {
    $planetDatabaseAccessObject = $this->getPlanetDatabaseAccessObjectFixture();
    $databaseAccessObject = $this->getPapayaDatabaseAccessObjectFixture(
      array('updateRecord')
   .md);
    $databaseAccessObject
      ->expects($this->once())
      ->method('updateRecord')
      ->will($this->returnValue(1));
    $planetDatabaseAccessObject->setDatabaseAccess($databaseAccessObject);
    $this->assertTrue($planetDatabaseAccessObject->update(2, array('planet_name' => 'Jupiter')));
  }

  /**
  * @covers PlanetDatabaseAccess::delete
  */
  public function testDelete() {
    $planetDatabaseAccessObject = $this->getPlanetDatabaseAccessObjectFixture();
    $databaseAccessObject = $this->getPapayaDatabaseAccessObjectFixture(
      array('deleteRecord')
   .md);
    $databaseAccessObject
      ->expects($this->once())
      ->method('deleteRecord')
      ->will($this->returnValue(1));
    $planetDatabaseAccessObject->setDatabaseAccess($databaseAccessObject);
    $this->assertTrue($planetDatabaseAccessObject->delete(9));
  }
}

?>
~~~~

In this test, a bit more mocking has to be done than before, and it seems more complicated, but with a bit of explanation it's pretty straightforward. We need to mock two classes:

-   *PapayaDatabaseAccess* was already explained above. Please note the verbose *getMock(.md)* call using five arguments: The name of the original class to be mocked, an array of methods to be provided, an array of parameters for the constructor (an empty array in this case), a dynamic name for the mock class -- 'Mock_'.md5(__CLASS__.microtime()) is a good choice as *microtime(.md)* changes fast enough to provide a unique name for each class --, and a boolean that states whether the constructor of the original class should be called (TRUE) or not (FALSE, which we use here).
-   *dbresult_mysql* is what a successful SELECT query returns if you use the MySQL database interface (theoretically, you could mock any of the papaya database result classes, but this one works just as fine as any). See the *PapayaDatabaseAccess* explanation above for details on the *getMock(.md)* parameters. Last, we define the DB_FETCHMODE_ASSOC constant with an arbitrary value if it's not already defined. The implementation will use this constant as an argument for the database result object's *fetchRow(.md)* method that is used to read a record from the result set.

After requiring the *PapayaTestCase* class, there's a static call to its *registerPapayaAutoloader(.md)* method. This makes sure that papaya's autoloader mechanism also works for the unit tests.

The test for *getAll(.md)* is by far the longest and the most complex one. As we are going to read more than one record from the result set, we need to make sure, that our mock result object returns different values for each call. You can achieve this using the *atLeastOnce(.md)* expectation for the method call and *onConsecutiveCalls(.md)* for the return values, as shown above. The last return value is FALSE because we are going to call *fetchRow(.md)* as the condition of a while loop, thus making sure it stops when there are no more records left to read.

You should be able to understand the rest of the test class if you have been following along this tutorial series up to this point. In the *getPapayaDatabaseAccessObjectFixture(.md)* method, we add 'getTableNames' to the array of mocked methods if it's not already present, and we expect this method to return the table name *papaya_tutorial_planets* -- it's easier to do this in the fixture because we need it for each test that uses the *PapayaDatabaseAccess* mock object.

Please note the *with* clause in this usage of *expects* which requires the expected method call to use the arguments you ask for (in this case, the string 'tutorial_plantes' and the boolean TRUE). This provides even more exact unit test results as you don't only expect a certain method call to happen, but also expect it to use certain argument values.

### Implementing the database access class

Here's the database access class that should pass all of the above tests:

~~~~ {.php}
<?php
/**
* Hello World tutorial, Planet database access class
*
* The Planet database access class provides the internal functionality
* to store planets in and load them from the database
*
* @package Papaya-Modules
* @subpackage tutorial
*/

/**
* Hello World tutorial, Planet database access class
*
* @package Papaya-Modules
* @subpackage tutorial
*/
class PlanetDatabaseAccess extends PapayaDatabaseObject {
  /**
  * Database table planets
  * @var string
  */
  private $_tablePlanets = 'tutorial_planets';

  /**
  * Get a planet by id
  *
  * @param integer $planetId id of the planet to load
  * @return mixed array planet data if id exists, FALSE otherwise
  */
  public function getById($planetId) {
    $sql = "SELECT planet_id, planet_name
              FROM %s
             WHERE planet_id = %d";
    $params = array($this->databaseGetTableName($this->_tablePlanets, TRUE), $planetId);
    $result = FALSE;
    if ($res = $this->databaseQueryFmt($sql, $params)) {
      if ($row = $res->fetchRow(DB_FETCHMODE_ASSOC)) {
        $result = $row;
      }
    }
    return $result;
  }

  /**
  * Get all planets
  *
  * The return value is an array in which the keys are planet ids and the values are planet names.
  * If no planets exist, the return value will be FALSE.
  *
  * @return mixed array planet data if planets exist, FALSE otherwise
  */
  public function getAll() {
    $sql = "SELECT planet_id, planet_name
              FROM %s";
    $params = array($this->databaseGetTableName($this->_tablePlanets, TRUE));
    $result = FALSE;
    if ($res = $this->databaseQueryFmt($sql, $params)) {
      while ($row = $res->fetchRow(DB_FETCHMODE_ASSOC)) {
        if ($result === FALSE) {
          $result = array();
        }
        $result[$row['planet_id']] = $row['planet_name'];
      }
    }
    return $result;
  }

  /**
  * Add a new planet to the database
  *
  * @param array $data planet data
  * @return boolean TRUE on success, FALSE otherwise
  */
  public function create($data) {
    $result = FALSE;
    $success = $this->databaseInsertRecord(
      $this->databaseGetTableName($this->_tablePlanets, TRUE),
      'planet_id',
      $data
   .md);
    if ($success !== FALSE) {
      $result = TRUE;
    }
    return $result;
  }

  /**
  * Modify an existing planet
  *
  * @param integer $planetId id of the planet to modify
  * @param array $data modified planet data
  * @return boolean TRUE on success, FALSE otherwise
  */
  public function update($planetId, $data) {
    $result = FALSE;
    $success = $this->databaseUpdateRecord(
      $this->databaseGetTableName($this->_tablePlanets, TRUE),
      $data,
      'planet_id',
      $planetId
   .md);
    if ($success !== FALSE) {
      $result = TRUE;
    }
    return $result;
  }

  /**
  * Delete a specific planet by id
  *
  * @param integer $planetId
  * @return boolean TRUE on success, FALSE otherwise
  */
  public function delete($planetId) {
    $result = FALSE;
    $success = $this->databaseDeleteRecord(
      $this->databaseGetTableName($this->_tablePlanets, TRUE),
      'planet_id',
      $planetId
   .md);
    if ($success !== FALSE) {
      $result = TRUE;
    }
    return $result;
  }
}
?>
~~~~

All methods in this class deals with the database table *papaya_tutorial_planets*: *getById(.md)* tries to read a record with a given value for the *planet_id* field, *getAll(.md)* retrieves an array of all planets, *create(.md)* inserts a new record, *update(.md)* modifies an existing one, and *delete(.md)* removes a record, again based on its id. The technique used to set the return value in each method prevents you from writing too many tests: As soon as you add more than one *return* statement to a method, or use *else* for an *if* statement, you need an additional test for that method (naming them something like *testDeleteExpectingSuccess(.md)* and *testDeleteExpectingFailure(.md)*, or the like). Or you may use *data providers* as explained [here](http://www.phpunit.de/manual/3.4/en/phpunit-book.html#writing-tests-for-phpunit.data-providers) in the PHPUnit documentation to run the same test with more than one set of values, expecting different results.

A final word on return values: In this simple example, we just return FALSE if one of the methods does not operate successfully. For larger projects, you should consider throwing, catching, and unit-testing distinct exceptions for invalid arguments, database connection errors, and other problems.

### Refactoring the public access class

Now that the database access class is up and running, we can refactor the *Planet* class to make use of it. This is really straightforward, so here's the implementation of the rebuilt unit test, followed by the class itself, without any further explanations:

~~~~ {.php}
<?php
require_once(substr(dirname(__FILE__), 0, -46).'/Framework/PapayaTestCase.php');
PapayaTestCase::registerPapayaAutoloader();
require_once(PAPAYA_INCLUDE_PATH.'modules/special/myproject/tutorial/Planet.php');
require_once(PAPAYA_INCLUDE_PATH.'modules/special/myproject/tutorial/Planet/Database/Access.php');

class PlanetTest extends PapayaTestCase {
  /**
  * Get the Planet object to be tested
  *
  * @return Planet
  */
  private function getPlanetObjectFixture() {
    return new Planet();
  }

  /**
  * Get a PlanetDatabaseAccess mock object fixture
  *
  * @return PlanetDatabaseAccess mock object
  */
  private function getDatabaseAccessObjectFixture() {
    return $this->getMock('PlanetDatabaseAccess');
  }

  /**
  * @covers Planet::setDatabaseAccessObject
  */
  public function testSetDatabaseAccessObject() {
    $planetObject = $this->getPlanetObjectFixture();
    $databaseAccessObject = $this->getDatabaseAccessObjectFixture();
    $planetObject->setDatabaseAccessObject($databaseAccessObject);
    $this->assertAttributeSame($databaseAccessObject, '_databaseAccessObject', $planetObject);
  }

  /**
  * @covers Planet::getDatabaseAccessObject
  */
  public function testGetDatabaseAccessObject() {
    $planetObject = $this->getPlanetObjectFixture();
    $databaseAccessObject = $planetObject->getDatabaseAccessObject();
    $this->assertAttributeType('PlanetDatabaseAccess', '_databaseAccessObject', $planetObject);
  }

  /**
  * @covers Planet::getById {
  */
  public function testGetById() {
    $planetObject = $this->getPlanetObjectFixture();
    $databaseAccessObject = $this->getDatabaseAccessObjectFixture();
    $expected = array('planet_id' => 1, 'planet_name' => 'Mars');
    $databaseAccessObject
      ->expects($this->once())
      ->method('getById')
      ->will($this->returnValue($expected));
    $planetObject->setDatabaseAccessObject($databaseAccessObject);
    $this->assertEquals($expected, $planetObject->getById(1));
  }

  /**
  * @covers Planet::getAll
  */
  public function testGetAll() {
    $planetObject = $this->getPlanetObjectFixture();
    $databaseAccessObject = $this->getDatabaseAccessObjectFixture();
    $expected = array(1 => 'Mars', 2 => 'Jupiter', 3 => 'Saturn');
    $databaseAccessObject
      ->expects($this->once())
      ->method('getAll')
      ->will($this->returnValue($expected));
    $planetObject->setDatabaseAccessObject($databaseAccessObject);
    $this->assertEquals($expected, $planetObject->getAll());
  }

  /**
  * @covers Planet::create {
  */
  public function testCreate() {
    $planetObject = $this->getPlanetObjectFixture();
    $databaseAccessObject = $this->getDatabaseAccessObjectFixture();
    $databaseAccessObject
      ->expects($this->once())
      ->method('create')
      ->will($this->returnValue(TRUE));
    $planetObject->setDatabaseAccessObject($databaseAccessObject);
    $data = array('planet_name' => 'Jupiter');
    $this->assertTrue($planetObject->create($data));
  }

  /**
  * @covers Planet::update
  */
  public function testUpdate() {
    $planetObject = $this->getPlanetObjectFixture();
    $databaseAccessObject = $this->getDatabaseAccessObjectFixture();
    $databaseAccessObject
      ->expects($this->once())
      ->method('update')
      ->will($this->returnValue(TRUE));
    $planetObject->setDatabaseAccessObject($databaseAccessObject);
    $data = array('planet_name' => 'Saturn');
    $this->assertTrue($planetObject->update(3, $data));
  }

  /**
  * @covers Planet::delete {
  */
  public function testDelete() {
    $planetObject = $this->getPlanetObjectFixture();
    $databaseAccessObject = $this->getDatabaseAccessObjectFixture();
    $databaseAccessObject
      ->expects($this->once())
      ->method('delete')
      ->will($this->returnValue(TRUE));
    $planetObject->setDatabaseAccessObject($databaseAccessObject);
    $this->assertTrue($planetObject->delete(2));
  }
}

?>
~~~~

And here's the rebuilt *Planet* class:

~~~~ {.php}
<?php
/**
* Hello World tutorial, Planet class
*
* The Planet class provides the public interface for planets to be stored in
* and loaded from database.
*
* @package Papaya-Modules
* @subpackage tutorial
*/

/**
* Hello World tutorial, Planet class
*
* @package Papaya-Modules
* @subpackage tutorial
*/
class Planet {
  /**
  * The PlanetDatabaseAccess object to be used
  * @var PlanetDatabaseAccess
  */
  private $_databaseAccessObject = NULL;

  /**
  * Set the PlanetDatabaseAccess object to be used
  *
  * @param PlanetDatabaseAccess $databaseAccessObject
  */
  public function setDatabaseAccessObject($databaseAccessObject) {
    $this->_databaseAccessObject = $databaseAccessObject;
  }

  /**
  * Get (and, if necessary, initialize) the PlanetDatabaseAccess object
  *
  * @return PlanetDatabaseAccess
  */
  public function getDatabaseAccessObject() {
    if (!is_object($this->_databaseAccessObject)) {
      include_once(dirname(__FILE__).'/Planet/Database/Access.php');
      $this->_databaseAccessObject = new PlanetDatabaseAccess();
    }
    return $this->_databaseAccessObject;
  }

  /**
  * Get a planet by id
  *
  * @param integer $planetId id of the planet to load
  * @return mixed array planet data if id exists, FALSE otherwise
  */
  public function getById($planetId) {
    $databaseAccessObject = $this->getDatabaseAccessObject();
    return $databaseAccessObject->getById($planetId);
  }

  /**
  * Get all planets
  *
  * @return mixed array planet data if planets exist, FALSE otherwise
  */
  public function getAll() {
    $databaseAccessObject = $this->getDatabaseAccessObject();
    return $databaseAccessObject->getAll();
  }

  /**
  * Add a new planet to the database
  *
  * @param array $data planet data
  * @return boolean TRUE on success, FALSE otherwise
  */
  public function create($data) {
    $databaseAccessObject = $this->getDatabaseAccessObject();
    return $databaseAccessObject->create($data);
  }

  /**
  * Modify an existing planet
  *
  * @param integer $planetId id of the planet to modify
  * @param array $data modified planet data
  * @return boolean TRUE on success, FALSE otherwise
  */
  public function update($planetId, $data) {
    $databaseAccessObject = $this->getDatabaseAccessObject();
    return $databaseAccessObject->update($planetId, $data);
  }

  /**
  * Delete a specific planet by id
  *
  * @param integer $planetId
  * @return boolean TRUE on success, FALSE otherwise
  */
  public function delete($planetId) {
    $databaseAccessObject = $this->getDatabaseAccessObject();
    return $databaseAccessObject->delete($planetId);
  }
}

?>
~~~~

All necessary concepts, especially dependency injection as in *Planet::setDatabaseAccessObject(.md)* and lazy initialization as in *Planet::getDatabaseAccessObject(.md)*, have already been explained in the [the first tutorial](Module_Development_1:_Content_Modules.md).

Creating a connector module

To make the methods of the *Planet* class available to the content module, we are going to write a *connector* module. Connectors can be included regardless of directory structures. This makes them the best choice to use functionality from base classes in content or administration modules, both within the same package and in other packages.

### Registering the connector

As the connector class is a module, it has to be registered in the package's *modules.xml* file. This has been explained in detail in the [first tutorial](Module_Programming_1:_Content_Modules.md). Add the following XML block to the *<modules>* section of *modules.xml*:

~~~~ {.xml}
    <module type="connector"
            guid=""
            name="Hello World Connector"
            class="HelloConnector"
            file="Hello/Connector.php">
      The Hello World Connector provides basic functionality for content and administration modules
    </module>
~~~~

Add a GUID as value of the guid attribute; the previous tutorial describes how to get one. We will need the GUID to use the connector, so here is the one I came up with: eeb42aad2491cd607c7c64bc57eae455

### Testing and implementing the connector

Usually, a connector collects the methods of all base classes in a package to make them accessible for modules. In our package, we have only got one base class, *Planet*, but nevertheless we add *Planet* or *Planets* to the method names, for example, *getPlanetById(.md)* instead of *getById(.md)* or *getAllPlanets(.md)* instead of *getAll(.md)*. This helps to easily extend the connector with other bases classes' funcionality later.

It's easy to write the unit test class and the connector class. As usual, let's look at the test first; it will be saved as *ConnectorTest.php* in the *tutorial/Hello* directory in the unit test directory structure:

~~~~ {.php}
<?php
require_once(substr(dirname(__FILE__), 0, -52).'/Framework/PapayaTestCase.php');
PapayaTestCase::registerPapayaAutoloader();
require_once(PAPAYA_INCLUDE_PATH.'modules/special/myproject/tutorial/Hello/Connector.php');
require_once(PAPAYA_INCLUDE_PATH.'modules/special/myproject/tutorial/Planet.php');

class HelloConnectorTest extends PapayaTestCase {
  /**
  * Get the HelloConnector object to test
  *
  * @return HelloConnector
  */
  private function getHelloConnectorObjectFixture() {
    return new HelloConnectorProxy();
  }

  /**
  * Get a Planet mock object
  *
  * @return Planet mock object
  */
  private function getPlanetObjectFixture() {
    return $this->getMock('Planet');
  }

  /**
  * @covers HelloConnector::setPlanetObject
  */
  public function testSetPlanetObject() {
    $helloConnectorObject = $this->getHelloConnectorObjectFixture();
    $planetObject = $this->getPlanetObjectFixture();
    $helloConnectorObject->setPlanetObject($planetObject);
    $this->assertAttributeSame($planetObject, '_planetObject', $helloConnectorObject);
  }

  /**
  * @covers HelloConnector::getPlanetObject
  */
  public function testGetPlanetObject() {
    $helloConnectorObject = $this->getHelloConnectorObjectFixture();
    $planetObject = $helloConnectorObject->getPlanetObject();
    $this->assertTrue($planetObject instanceof Planet);
  }

  /**
  * @covers HelloConnector::getPlanetById
  */
  public function testGetPlanetById() {
    $helloConnectorObject = $this->getHelloConnectorObjectFixture();
    $planetObject = $this->getPlanetObjectFixture();
    $expected = array('planet_id' => 1, 'planet_name' => 'Mars');
    $planetObject
      ->expects($this->once())
      ->method('getById')
      ->will($this->returnValue($expected));
    $helloConnectorObject->setPlanetObject($planetObject);
    $this->assertEquals($expected, $helloConnectorObject->getPlanetById(1));
  }

  /**
  * @covers HelloConnector::getAllPlanets
  */
  public function testGetAllPlanets() {
    $helloConnectorObject = $this->getHelloConnectorObjectFixture();
    $planetObject = $this->getPlanetObjectFixture();
    $expected = array(1 => 'Mars', 2 => 'Jupiter', 3 => 'Saturn');
    $planetObject
      ->expects($this->once())
      ->method('getAll')
      ->will($this->returnValue($expected));
    $helloConnectorObject->setPlanetObject($planetObject);
    $this->assertEquals($expected, $helloConnectorObject->getAllPlanets());
  }

  /**
  * @covers HelloConnector::createPlanet
  */
  public function testCreatePlanet() {
    $helloConnectorObject = $this->getHelloConnectorObjectFixture();
    $planetObject = $this->getPlanetObjectFixture();
    $planetObject
      ->expects($this->once())
      ->method('create')
      ->will($this->returnValue(TRUE));
    $helloConnectorObject->setPlanetObject($planetObject);
    $this->assertTrue(
      $helloConnectorObject->createPlanet(array('planet_name' => 'Neptune'))
   .md);
  }

  /**
  * @covers HelloConnector::updatePlanet
  */
  public function testUpdatePlanet() {
    $helloConnectorObject = $this->getHelloConnectorObjectFixture();
    $planetObject = $this->getPlanetObjectFixture();
    $planetObject
      ->expects($this->once())
      ->method('update')
      ->will($this->returnValue(TRUE));
    $helloConnectorObject->setPlanetObject($planetObject);
    $this->assertTrue(
      $helloConnectorObject->updatePlanet(1, array('planet_name' => 'Mars'))
   .md);
  }

  /**
  * @covers HelloConnector::deletePlanet
  */
  public function testDeletePlanet() {
    $helloConnectorObject = $this->getHelloConnectorObjectFixture();
    $planetObject = $this->getPlanetObjectFixture();
    $planetObject
      ->expects($this->once())
      ->method('delete')
      ->will($this->returnValue(TRUE));
    $helloConnectorObject->setPlanetObject($planetObject);
    $this->assertTrue($helloConnectorObject->deletePlanet(2));
  }
}

class HelloConnectorProxy extends HelloConnector {
  public function __construct() {
    // Nothing to do here; just provide a constructor without parameters
  }
}

?>
~~~~

The connector class itself (*tutorial/Hello/Connector.php*) looks like this:

~~~~ {.php}
<?php
/**
* Hello World tutorial, connector class
*
* The connector class provides the base methods to be used by content and admin modules.
*
* @package Papaya-Modules
* @subpackage tutorial
*/

/**
* Base class base_connector
*/
require_once(PAPAYA_INCLUDE_PATH.'system/base_connector.php');

/**
* Hello World tutorial, connector class
*
* @package Papaya-Modules
* @subpackage tutorial
*/
class HelloConnector extends base_connector {
  /**
  * The Planet object to be used
  * @var Planet
  */
  private $_planetObject = NULL;

  /**
  * Set the Planet object to be used
  *
  * @param Planet $planetObject
  */
  public function setPlanetObject($planetObject) {
    $this->_planetObject = $planetObject;
  }

  /**
  * Get (and, if necessary, initialize) the Planet object
  *
  * @return Planet
  */
  public function getPlanetObject() {
    if (!is_object($this->_planetObject)) {
      include_once(substr(dirname(__FILE__), 0, -6).'/Planet.php');
      $this->_planetObject = new Planet();
    }
    return $this->_planetObject;
  }

  /**
  * Get a planet by id
  *
  * @param integer $planetId
  * @return mixed array if the planet exists, FALSE otherwise
  */
  public function getPlanetById($planetId) {
    $planetObject = $this->getPlanetObject();
    return $planetObject->getById($planetId);
  }

  /**
  * Get all planets
  *
  * @return mixed array if planets exist, FALSE otherwise
  */
  public function getAllPlanets() {
    $planetObject = $this->getPlanetObject();
    return $planetObject->getAll();
  }

  /**
  * Insert a new planet
  *
  * @param array $planetData
  * @return boolean TRUE on success, FALSE otherwise
  */
  public function createPlanet($data) {
    $planetObject = $this->getPlanetObject();
    return $planetObject->create($data);
  }

  /**
  * Update a planet
  *
  * @param integer $planetId
  * @param array $data
  * @return boolean TRUE on success, FALSE otherwise
  */
  public function updatePlanet($planetId, $data) {
    $planetObject = $this->getPlanetObject();
    return $planetObject->update($planetId, $data);
  }

  /**
  * Delete a planet
  *
  * @param integer $planetId
  * @return boolean TRUE on success, FALSE otherwise
  */
  public function deletePlanet($planetId) {
    $planetObject = $this->getPlanetObject();
    return $planetObject->delete($planetId);
  }
}

?>
~~~~

### Getting a connector object in a module

If you want to use a connector in your module, you can load it using an instance of papaya's *base_pluginloader* class. The code you need to do this is as simple as this:

~~~~ {.php}
  $pluginloader = new base_pluginloader();
  $connectorObject = $pluginloader->getPluginInstance($connectorGuid, $owner);
~~~~

where *\$owner* is the current content module and *\$connectorGuid* is a string containing the GUID of the connector. As we want to mock the connector class in order to unit-test the module using it, we need getter and setter methods for both the *base_pluginloader* and the connector instance, and additionally a *setOwner(.md)* method to be called by the content class. This is the modified *HelloPageBaseTest* class (we just left out the *testSetPageData(.md)* and *testGetPageXml(.md)* methods because they stay the same for now):

~~~~ {.php}
<?php

require_once(substr(__FILE__, 0, -57).'/Framework/PapayaTestCase.php');
PapayaTestCase::registerPapayaAutoloader();
require_once(PAPAYA_INCLUDE_PATH.'modules/beta/tutorial/Hello/Page/Base.php');
require_once(PAPAYA_INCLUDE_PATH.'modules/beta/tutorial/Hello/Connector.php');

class HelloPageTest extends PapayaTestCase {
  /**
  * Instantiate the HelloPageBase object to be tested
  *
  * @return HelloPageBase
  */
  private function getHelloPageBaseObjectFixture() {
    $this->defineConstantDefaults('PAPAYA_DB_TBL_MODULES');
    return new HelloPageBase();
  }

  /**
  * Get an owner mock object (it doesn't need a specific class)
  *
  * @return object
  */
  private function getOwnerObjectFixture() {
    return $this->getMock('GenericContentClass');
  }

  /**
  * Get a base_pluginloader mock object
  *
  * @return base_pluginloader mock object
  */
  private function getPluginloaderObjectFixture() {
    return $this->getMock('base_pluginloader');
  }

  /**
  * Get a HelloConnector mock object
  *
  * @return HelloConnector mock object
  */
  private function getHelloConnectorObjectFixture() {
    return $this->getMock(
      'HelloConnector',
      array(),
      array(),
      'Mock_'.md5(__CLASS__.microtime()),
      FALSE
   .md);
  }

  /**
  * @covers HelloPageBase::setOwner
  */
  public function testSetOwner() {
    $helloPageBaseObject = $this->getHelloPageBaseObjectFixture();
    $owner = $this->getOwnerObjectFixture();
    $helloPageBaseObject->setOwner($owner);
    $this->assertAttributeSame($owner, '_owner', $helloPageBaseObject);
  }

  /**
  * @covers HelloPageBase::setPluginloaderObject
  */
  public function testSetPluginloaderObject() {
    $helloPageBaseObject = $this->getHelloPageBaseObjectFixture();
    $pluginloaderObject = $this->getPluginloaderObjectFixture();
    $helloPageBaseObject->setPluginloaderObject($pluginloaderObject);
    $this->assertAttributeSame(
      $pluginloaderObject,
      '_pluginloaderObject',
      $helloPageBaseObject
   .md);
  }

  /**
  * @covers HelloPageBase::getPluginloaderObject
  */
  public function testGetPluginloaderObject() {
    $helloPageBaseObject = $this->getHelloPageBaseObjectFixture();
    $pluginloaderObject = $helloPageBaseObject->getPluginloaderObject();
    $this->assertTrue($pluginloaderObject instanceof base_pluginloader);
  }

  /**
  * @covers HelloPageBase::setHelloConnectorObject
  */
  public function testSetHelloConnectorObject() {
    $helloPageBaseObject = $this->getHelloPageBaseObjectFixture();
    $helloConnectorObject = $this->getHelloConnectorObjectFixture();
    $helloPageBaseObject->setHelloConnectorObject($helloConnectorObject);
    $this->assertAttributeSame(
      $helloConnectorObject,
      '_helloConnectorObject',
      $helloPageBaseObject
   .md);
  }

  /**
  * @covers HelloPageBase::getHelloConnectorObject
  */
  public function testGetHelloConnectorObject() {
    $helloPageBaseObject = $this->getHelloPageBaseObjectFixture();
    $owner = $this->getOwnerObjectFixture();
    $pluginloaderObject = $this->getPluginloaderObjectFixture();
    $helloConnectorObject = $this->getHelloConnectorObjectFixture();
    $pluginloaderObject
      ->expects($this->once())
      ->method('getPluginInstance')
      ->will($this->returnValue($helloConnectorObject));
    $helloPageBaseObject->setOwner($owner);
    $helloPageBaseObject->setPluginloaderObject($pluginloaderObject);
    $this->assertSame(
      $helloConnectorObject,
      $helloPageBaseObject->getHelloConnectorObject()
   .md);
  }

  // testSetPageData() and testGetPageXml() stay the same
}
~~~~

*defineConstantDefaults(.md)* is a *PHPUnit_TestCase* method that defines a necessary constant using a default value. The *PAPAYA_DB_TBL_MODULES* is a constant containing the name of the papaya modules database table. This table is used by *base_pluginloader* to read the module by its GUID. You should understand the rest of the new tests without any trouble, so here's the new code for the *HelloPageBase* class (add it at the beginning of the class body, keeping in mind that the declaration of the *\$_data* property was already there before):

~~~~ {.php}
  /**
  * Owner object
  * @var HelloPage
  */
  private $_owner = NULL;

  /**
  * base_pluginloader object
  * @var base_pluginloader
  */
  private $_pluginloaderObject = NULL;

  /**
  * HelloConnector object
  * @var HelloConnector
  */
  private $_helloConnectorObject = NULL;

  /**
  * Page configuration data
  * @var array
  */
  private $_data = array();

  /**
  * Set owner object
  *
  * @param HelloPage $owner
  */
  public function setOwner($owner) {
    $this->_owner = $owner;
  }

  /**
  * Set the base_pluginloader object to use
  *
  * @param base_pluginloader $pluginloaderObject
  */
  public function setPluginloaderObject($pluginloaderObject) {
    $this->_pluginloaderObject = $pluginloaderObject;
  }

  /**
  * Get (and, if necessary, initialize) the base_pluginloader object
  *
  * @return base_pluginloader
  */
  public function getPluginloaderObject() {
    if (!is_object($this->_pluginloaderObject)) {
      $this->_pluginloaderObject = new base_pluginloader();
    }
    return $this->_pluginloaderObject;
  }

  /**
  * Set the HelloConnector object to use
  *
  * @param HelloConnector $helloConnectorObject
  */
  public function setHelloConnectorObject($helloConnectorObject) {
    $this->_helloConnectorObject = $helloConnectorObject;
  }

  /**
  * Get (and, if necessary, initialize) the HelloConnector object
  *
  * @return HelloConnector
  */
  public function getHelloConnectorObject() {
    if (!is_object($this->_helloConnectorObject)) {
      $pluginloaderObject = $this->getPluginloaderObject();
      $this->_helloConnectorObject = $pluginloaderObject->getPluginInstance(
        'eeb42aad2491cd607c7c64bc57eae455', $this->owner
     .md);
    }
    return $this->_helloConnectorObject;
  }
~~~~

If you have used another GUID for your connector module, replace it. As soon as the unit tests work, we have successfully added code to initialize the connector object, so we can use its methods in the next step.

Using the database functionality in the content module

In the base class of our content module, we are going to use two methods from the connector: *getAllPlanets(.md)* to load the list of planets and *getPlanetById(.md)* to load a single planet. The list is used to select a planet during page configuration. Its ID is saved in page data, and when the page is displayed, the name will be retrieved by id again.

### Extending the base class

First, we are going to modify one method and to add another one in the *HelloBase* class. Make the following change to *testGetPageXml(.md)* in the *HelloPageBaseTest* class:

~~~~ {.php}
  /**
  * @covers HelloPageBase::getPageXML()
  */
  public function testGetPageXML() {
    $helloPageBaseObject = $this->getHelloPageBaseObjectFixture();
    $helloConnectorObject = $this->getHelloConnectorObjectFixture();
    $helloConnectorObject
      ->expects($this->once())
      ->method('getPlanetById')
      ->will($this->returnValue(array('planet_id' => 1, 'planet_name' => 'Mars')));
    $helloPageBaseObject->setHelloConnectorObject($helloConnectorObject);
    $helloPageBaseObject->setPageData(array('planet_id' => 1, 'text' => 'Hello'));
    $xml = '<title>Hello Mars!</title>
<text>Hello</text>';
    $this->assertEquals($xml, $helloPageBaseObject->getPageXML());
  }
~~~~

Now replace the existing *getPageXml(.md)* method in *HelloPageBase* by this one:

~~~~ {.php}
  /**
  * Get the page's XML output
  *
  * @return string XML
  */
  public function getPageXML() {
    $helloConnectorObject = $this->getHelloConnectorObject();
    $planet = '';
    $planetData = $helloConnectorObject->getPlanetById($this->_data['planet_id']);
    if (is_array($planetData) && !empty($planetData)) {
      $planet = $planetData['planet_name'];
    }
    $result = sprintf('<title>Hello %s!</title>'.LF, $planet);
    $result .= sprintf('<text>%s</text>', papaya_strings::escapeHTMLChars($this->_data['text']));
    return $result;
  }
~~~~

As you can see, we initialize the planet name as an empty string and only replace it by the *planet_name* element from the result if it's a non-empty array. This prevents errors, for example when a planet id stored in the page data no longer exists in the planets table.

Next up is our method to load the list of planets for the selector in the page configuration dialog. The unit test looks like this (add it to the end of *HelloPageBaseTest*'s class body):

~~~~ {.php}
  /**
  * @covers HelloPageBase::getPlanetSelector
  */
  public function testGetPlanetSelector() {
    $helloPageBaseObject = $this->getHelloPageBaseObjectFixture();
    $owner = $this->getOwnerObjectFixture();
    $owner->paramName = 'tut';
    $helloConnectorObject = $this->getHelloConnectorObjectFixture();
    $helloConnectorObject
      ->expects($this->once())
      ->method('getAllPlanets')
      ->will($this->returnValue(array(1 => 'Mars', 2 => 'Jupiter')));
    $helloPageBaseObject->setOwner($owner);
    $helloPageBaseObject->setHelloConnectorObject($helloConnectorObject);
    $expectedXml = '<select name="tut[planet_id]" class="dialogSelect dialogScale">
<option value="1" selected="selected">Mars</option>
<option value="2">Jupiter</option>
</select>';
    $this->assertXmlStringEqualsXmlString(
      $expectedXml,
      $helloPageBaseObject->getPlanetSelector('planet_id', 1)
   .md);
  }
~~~~

Now we can implement the *HelloPageBase* method that will pass this test; again, add it to the end of the class body:

~~~~ {.php}
  /**
  * Get a select box for planets
  *
  * @param string $name
  * @param integer $data
  */
  public function getPlanetSelector($name, $data) {
    $result = '';
    $helloConnectorObject = $this->getHelloConnectorObject();
    $planetList = $helloConnectorObject->getAllPlanets();
    if (is_array($planetList) && !empty($planetList)) {
      $result = sprintf(
        '<select name="%s[%s]" class="dialogSelect dialogScale">'.LF,
        $this->_owner->paramName,
        $name
     .md);
      foreach ($planetList as $planetId => $planetName) {
        $selected = ($planetId == $data) ? ' selected="selected"' : '';
        $result .= sprintf(
          '<option value="%d"%s>%s</option>'.LF,
          $planetId,
          $selected,
          $planetName
       .md);
      }
      $result .= '</select>';
    }
    return $result;
  }
~~~~

There's nothing really new in this method to be discussed. Once again, we save ourselves the need to write more than one test for this method by choosing the correct processing order: We don't use any *else* block for an *if* statement and we've only got one *return* statement. The *dialogSelect dialogScale* CSS classes are used by the papaya CMS backend to make the select box identical to the other, autogenerated form elements. As *\$data* contains the currently selected planet id, we can set the corresponding option as selected.

### Adjusting the content class

The last thing we need to do is make our *HelloPage* content class match the changes in the base class. Nothing substantial needs to be changed in the *getParsedData(.md)* method -- just add the following line behind the *\$baseObject = \$this-\>getBaseObject();* line:

~~~~ {.php}
    $baseObject->setOwner($this);
~~~~

You don't even need to modify the unit test for this one as the method is mocked automatically, and it doesn't have a return value we need to expect.

Now add the *planet_id* field to the definition of *\$editFields*; the complete definition should look like this:

~~~~ {.php}
  /**
  * Edit fields for page configuration
  * @var array
  */
  public $editFields = array(
    'text' => array(
      'Text',
      'isNoHTML',
      TRUE,
      'textarea',
      5,
      '',
      'Greetings from the new module'
   .md),
    'planet_id' => array(
      'Planet',
      'isNum',
      FALSE,
      'function',
      'callbackPlanetSelector'
   .md)
 .md);
~~~~

The *planet_id* field uses the check type *isNum* that only allows numeric values. The field is not mandatory (*FALSE*). The field type is *function* -- this means that a callback method will be called whenever the field is encountered; papaya CMS will pass it the field name, the complete field definition array, and its current value. The next argument must be the name of this method. Before we implement it, take a look at its unit test (to be added to the end of *HelloPageTest*'s class body):

~~~~ {.php}
  /**
  * @covers HelloPage::callbackPlanetSelector
  */
  public function testGetCallbackPlanetSelector() {
    $helloPageObject = $this->getHelloPageObjectFixture();
    $helloPageBaseObject = $this->getMock('HelloPageBase');
    $helloPageBaseObject
      ->expects($this->once())
      ->method('getPlanetSelector')
      ->will($this->returnValue('<select />'));
    $helloPageObject->setBaseObject($helloPageBaseObject);
    $this->assertEquals(
      '<select />',
      $helloPageObject->callbackPlanetSelector('planet_id', array(), 1)
   .md);
  }
~~~~

We're not interested in implementation details of the base class on this level, so it's enough to expect an empty XML node as a return value. And we don't need anything from the field definition array (it's not even a parameter for *HelloPageBase::getPlanetSelector(.md)*), so we simply use an empty array here. The method's implementation at the end of the *HelloPage* class looks like this:

~~~~ {.php}
  /**
  * Callback method for a planet select field
  *
  * @param string $name
  * @param array $field
  * @param integer $data
  * @return string XML
  */
  public function callbackPlanetSelector($name, $field, $data) {
    $baseObject = $this->getBaseObject();
    $baseObject->setOwner($this);
    return $baseObject->getPlanetSelector($name, $data);
  }
~~~~

And that's it. Before you try out the content module practically, don't forget to search for new modules in the papaya backend (because otherwise, the new connector module won't be found), and add some planets to the database table using your database administration tool. In the next tutorial, however, we're going to implement an administration class for this package in which we can add, delete, and modify planets.
