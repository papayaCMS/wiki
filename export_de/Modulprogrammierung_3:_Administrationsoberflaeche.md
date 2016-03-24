
In this third part of the basic module development tutorial, we are going to create a custom backend administration interface for our module package. It will provide a convenient way to add, modify, and delete planets using the database access methods we implemented in the [previous part](Module_Development_2:_Adding_Database_Support.md).

Please note that you should adhere to the [Papaya CMS Coding Standards](Papaya_CMS_Coding_Standards.md), especially if you plan to contribute your modules for the papaya Community.

This tutorial makes use of unit tests which is a highly recommended way to build software. In order to have a papaya CMS version that includes the PHPUnit framework and the PapayaTestCase class, you need to check out papaya CMS from the SVN repository. Instructions on how to obtain it can be found [here](http://www.papaya-cms.com/download.990.en.html#svn).

Setting up the Module

Administration modules consist of at least two files: The module file itself that initializes the environment and a class file that delivers the actual contents. We are going to create the module file first. To match papaya CMS's rewrite rules, you have to name an administration module file using the prefix *edmodule_*, followed by an all-lowercase name similar to the package name. In this case, we are going to use *edmodule_tutorial_hello.php*, so add the following block to the *modules.xml* file:

~~~~ {.xml}
  <module type="admin"
          guid=""
          name="Hello World Tutorial"
          class="edmodule_tutorial_hello"
          file="edmodule_tutorial_hello.php">
    The administration interface allows you to add, delete, and modify planets.
  </module>
~~~~

As usual, you need to create a GUID and add it to the *guid* attribute as a value (see section [Preparing the modules.xml file](Module_Development_1:_Content_Modules#Preparing_the_modules.xml_file.md) in the first part of this tutorial series for details).

Writing the Module File

Module files are pretty straightforward and short, and they are the only part of the current module implementation that is not covered by unit tests. Here is the full code of *edmodule_tutorial_hello.php*:

~~~~ {.PHP}
<?php
/**
* Hello World tutorial modification module
*
* @package Papaya-Modules
* @subpackage Tutorial-HelloWorld
*/

/**
* Basic class modification module
*/
require_once(PAPAYA_INCLUDE_PATH.'system/base_module.php');

/**
* Hello World modification module
*
* Planet administration
*
* @package Papaya-Modules
* @subpackage Tutorial-HelloWorld
*/
class edmodule_tutorial_hello extends base_module {
  /**
  * Permissions
  * @var array
  */
  public $permissions = array(
    1 => 'Manage'
 .md);

  /**
  * Execute module
  */
  public function execModule() {
    if ($this->hasPerm(1, TRUE)) {
      include_once(dirname(__FILE__)."/Planet/Admin.php");
      $planetAdmin = new PlanetAdmin();
      $planetAdmin->module = &$this;
      $planetAdmin->msgs = &$this->msgs;
      $planetAdmin->images = &$this->images;
      $planetAdmin->layout = &$this->layout;
      $planetAdmin->execute();
      $planetAdmin->getXml($this->layout);
      $planetAdmin->getButtons();
    }
  }
}

?>
~~~~

As you can see, the module file extends *base_module*. This class defines the common features for backend administration modules. The class itself only defines one attribute and one method.

The *\$permissions* attribute contains an array of permissions for backend users. Complex administration modules like the papaya Community often have up to ten different permissions, allowing or prohibiting different sets of actions for different user roles. This simple module only contains a single permission. If a specific user has this permission, she has access to the complete administration interface of this module.

The *execModule(.md)* method is automatically called by *papaya/module.php* when a module editing URL matches papaya's rewrite rules. It checks the basic permission 1 ('Manage') first. Users without this permission will not be able to access the admin module at all. Next, an instance of the *PlanetAdmin* class is created. This class will be described in the next section; it contains the actual module editor GUI and functionality.

Next, you need to set some of the admin class's public attributes: *module* is the current file, i.e. *\$this*. *msgs* is an instance of the *papaya_errors* class; it's used to handle error messages. Simply pass a reference to *\$this-\>msgs* from the module file's context to provide the current error messages to the admin class. *images* is an array of default icons that can be used for menu and list items. You can see all of these icons in the papaya backend: Click on *Settings* in the *Administration* group, and then click *View icons*. *layout* is the environment in which the GUI is rendered using special, predefined XML structures. Using icons and GUI XML will be described later in this tutorial.

Eventually, the main methods of the admin class instance are called: *execute(.md)* is used to execute the module's commands, invoked by buttons in menu bars or list views; *getXml(.md)* returns the XML for the current layout; and *getButtons(.md)* returns the XML for the main toolbar.

Writing the Admin Class File

The admin class file will be unit-tested again, so let's start with the stub for the unit test class. Create a file called tutorial/Planet/AdminTest.php in your package's unit test directory. Add the following content:

~~~~ {.PHP}
<?php
require_once(substr(dirname(__FILE__), 0, -56).'/Framework/PapayaTestCase.php');
PapayaTestCase::registerPapayaAutoloader();
require_once(PAPAYA_INCLUDE_PATH.'modules/special/myproject/tutorial/Planet/Admin.php');

class PlanetAdminTest extends PapayaTestCase {
  /**
  * Get the PlanetAdmin object to be tested
  *
  * @return PlanetAdmin
  */
  private function getPlanetAdminObjectFixture() {
    $this->defineConstantDefaults(array('PAPAYA_DB_TBL_MODULES'));
    return new PlanetAdmin();
  }
}

?>
~~~~

The *PAPAYA_DB_TBL_MODULES* constant which we define with a default value is used by the current implementation of the *base_pluginloader* class. This class will be used later to load a connector instance, and the creation of the mock object we use for the unit test would make the test fail because of the missing constant definition.

Now add the corresponding implementation stub as tutorial/Planet/Admin.php to your tutorial module directory:

~~~~ {.PHP}
<?php
/**
* Hello World tutorial, Planet administration class
*
* The Planet class provides the public interface for planets to be stored in
* and loaded from database.
*
* @package Papaya-Modules
* @subpackage tutorial
*/

/**
* Basic class base_object
*/
require_once(PAPAYA_INCLUDE_PATH.'system/sys_base_object.php');

/**
* Hello World tutorial, Planet class
*
* @package Papaya-Modules
* @subpackage tutorial
*/
class PlanetAdmin extends base_object {
  /**
  * Parameter namespace
  * @var string
  */
  public $paramName = 'tut';
}

?>
~~~~

The public *\$this-\>paramName* property will be used for link and form field parameters, just like for content modules.

### Providing a Connector instance

The first thing we need in the admin class is an instance of the *HelloConnector* class. The concept of getting a connector instance using a *base_pluginloader* instance has already been explained in the previous tutorial. Add the following code to the unit test class, right under the *getPlanetAdminObjectFixture()* method, to provide tests for setting and getting the base_pluginloader and connector objects:

~~~~ {.PHP}
  /**
  * @covers PlanetAdmin::setPluginloaderObject
  */
  public function testSetPluginloaderObject() {
    $adminObject = $this->getPlanetAdminObjectFixture();
    $pluginloaderObject = $this->getMock('base_pluginloader');
    $adminObject->setPluginloaderObject($pluginloaderObject);
    $this->assertAttributeSame($pluginloaderObject, '_pluginloaderObject', $adminObject);
  }

  /**
  * @covers PlanetAdmin::getPluginloaderObject
  */
  public function testGetPluginloaderObject() {
    $adminObject = $this->getPlanetAdminObjectFixture();
    $pluginloaderObject = $adminObject->getPluginloaderObject();
    $this->assertTrue($pluginloaderObject instanceof base_pluginloader);
  }

  /**
  * @covers PlanetAdmin::setConnectorObject
  */
  public function testSetConnectorObject() {
    $adminObject = $this->getPlanetAdminObjectFixture();
    $connectorObject = $this->getMock('HelloConnector');
    $adminObject->setConnectorObject($connectorObject);
    $this->assertAttributeSame($connectorObject, '_connectorObject', $adminObject);
  }

  /**
  * @covers PlanetAdmin::getConnectorObject
  */
  public function testGetConnectorObject() {
    $adminObject = $this->getPlanetAdminObjectFixture();
    $connectorObject = $this->getMock('HelloConnector');
    $pluginloaderObject = $this->getMock('base_pluginloader');
    $pluginloaderObject
      ->expects($this->once())
      ->method('getPluginInstance')
      ->will($this->returnValue($connectorObject));
    $adminObject->setPluginloaderObject($pluginloaderObject);
    $this->assertSame($connectorObject, $adminObject->getConnectorObject());
  }
~~~~

Now, you can add the corresponding implementation code as the body of the PlanetAdmin class and run the unit tests afterwards:

~~~~ {.PHP}
  /**
  * Plugin loader object
  * @var base_pluginloader
  */
  private $_pluginloaderObject = NULL;

  /**
  * Hello world connector object
  * @var HelloConnector
  */
  private $_connectorObject = NULL;

  /**
  * Set the plugin loader object to be used
  *
  * @param base_pluginloader $pluginloaderObject
  */
  public function setPluginloaderObject($pluginloaderObject) {
    $this->_pluginloaderObject = $pluginloaderObject;
  }

  /**
  * Get (and, if necessary, initialize) the plugin loader object
  *
  * @return base_pluginloader
  */
  public function getPluginloaderObject() {
    if (!is_object($this->_pluginloaderObject)) {
      include_once(PAPAYA_INCLUDE_PATH.'system/base_pluginloader.php');
      $this->_pluginloaderObject = new base_pluginloader();
    }
    return $this->_pluginloaderObject;
  }

  /**
  * Set the Hello world connector object to be used
  *
  * @param HelloConnector $connectorObject
  */
  public function setConnectorObject($connectorObject) {
    $this->_connectorObject = $connectorObject;
  }

  /**
  * Get (and, if necessary, initialize) the Hello world connector object
  *
  * @return HelloConnector
  */
  public function getConnectorObject() {
    if (!is_object($this->_connectorObject)) {
      $pluginloaderObject = $this->getPluginloaderObject();
      $this->_connectorObject =
        $pluginloaderObject->getPluginInstance('eeb42aad2491cd607c7c64bc57eae455', $this);
    }
    return $this->_connectorObject;
  }
~~~~

### Showing a list of existing planets

The first task of the GUI implementation is to show the list of existing planets. This list is always present, and the method responsible for this functionality is called using the *getXml()* method. In order to write a unit test for the new *getPlanetsList()* method, our test class needs a little refactoring: we are going to use a proxy class, this time not for the constructor's sake but to override a few test-inconvenient papaya API methods. To provide the proxy class, add the following code underneath the closing curly brace } of the *PlanetAdminTest* class:

~~~~ {.PHP}
class PlanetAdminProxy extends PlanetAdmin {
  public function getLink() {
    return 'module_edmodule_tutorial_hello.php';
  }

  public function _gt($str) {
    return $str;
  }
}
~~~~

The *getLink()* method can create web links for backend admin classes using just an array of parameters as a single argument (and optional additional arguments). The *_gt()* method is used to provide translations for GUI elements for the multi-language support in the papaya backend.

To make sure the proxy class is used instead of the original PlanetAdmin class, change the *getPlanetAdminObjectFixture()* method, like so (i.e. use *PlanetAdminProxy* instead of *PlanetAdmin* for both the *return* statement and the *@return* PHPDoc annotation):

~~~~ {.PHP}
  /**
  * Get the PlanetAdmin object to be tested
  *
  * @return PlanetAdminProxy
  */
  private function getPlanetAdminObjectFixture() {
    $this->defineConstantDefaults(array('PAPAYA_DB_TBL_MODULES'));
    return new PlanetAdminProxy();
  }
~~~~

One last thing you need to add to the unit test file is an include statement for the original *HelloConnector* class. This will make sure that mock objects for this class are created providing the correct methods. Add the following line above the *class* statement, below the other *require_once()* statements:

~~~~ {.PHP}
require_once(PAPAYA_INCLUDE_PATH.'modules/beta/tutorial/Hello/Connector.php');
~~~~

Now, you can add the test method for the *getPlanetsList()* method:

~~~~ {.PHP}
  /**
  * @covers PlanetAdmin::getPlanetsList
  */
  public function testGetPlanetsList() {
    $adminObject = $this->getPlanetAdminObjectFixture();
    $connectorObject = $this->getMock('HelloConnector');
    $connectorObject
      ->expects($this->once())
      ->method('getAllPlanets')
      ->will($this->returnValue(array(1 => 'Mars', 2 => 'Jupiter')));
    $adminObject->setConnectorObject($connectorObject);
    $expectedXml = '<listview title="Planets">
<cols>
<col>Planet</col>
</cols>
<items>
<listitem href="module_edmodule_tutorial_hello.php" title="Mars" />
<listitem href="module_edmodule_tutorial_hello.php" title="Jupiter" selected="selected" />
</items>
</listview>
';
    $adminObject->params = array('id' => 2);
    $this->assertEquals($expectedXml, $adminObject->getPlanetsList());
  }
~~~~

You can place this method right below the *getPlanetAdminObjectFixture()* method as the *getPlanetsList()* method will be at the same position in the *PlanetAdmin* class -- above the service methods for the plugin loader and connector objects.

Now add the *getPlanetsList()* method to the admin file, above the *setPluginloaderObject()* method:

~~~~ {.PHP}
  /**
  * Get the list of existing planets
  *
  * @return string XML
  */
  public function getPlanetsList() {
    $result = '';
    $connectorObject = $this->getConnectorObject();
    $planets = $connectorObject->getAllPlanets();
    if (!empty($planets)) {
      $result = sprintf('<listview title="%s">'.LF, $this->_gt('Planets'));
      $result .= '<cols>'.LF;
      $result .= sprintf('<col>%s</col>'.LF, $this->_gt('Planet'));
      $result .= '</cols>'.LF;
      $result .= '<items>'.LF;
      foreach ($planets as $id => $name) {
        $selected = '';
        if (isset($this->params['id']) && $id == $this->params['id']) {
          $selected = ' selected="selected"';
        }
        $link = $this->getLink(array('cmd' => 'edit_planet', 'id' => $id));
        $result .= sprintf(
          '<listitem href="%s" title="%s"%s />'.LF,
          $link,
          papaya_strings::escapeHTMLChars($name),
          $selected
       .md);
      }
      $result .= '</items>'.LF;
      $result .= '</listview>'.LF;
    }
    return $result;
  }
~~~~

Now execute the unit tests, they should work without problems.

The *<listview>...</listview>* XML structure is a standard output element for backend classes. The structure is as follows:

~~~~ {.XML}
  <listview title="{Title}">
    <cols>  <!-- Define the columns and their headlines -->
      <col>{Headline}</col>
      <!-- Add more col elements if you need more than one column -->
    </cols>
    <items>  <!-- Start the lines of the list, i.e. list items -->
      <listitem href="{Link}" title="{Content}"[ selected="selected"]>
        <!-- Add the optional selected="selected" attribute for the current item -->
        <subitem>{Text}</subitem>
        <!-- Add more subitem elements for more than two columns,
             or leave the subitems out if there's only one column -->
      </listitem>
    </items>
  </listview>
~~~~

</source>
Next, we are going to add the first implementation of the *getXml()* method. For now, it will only invoke the *getPlanetsList()* method and add its result to the module's layout object.

In the *getPlanetAdminObjectFixture()* method, modify the *defineConstantDefaults()* invocation to include *PAPAYA_XSLT_EXTENSION* as another constant:

~~~~ {.PHP}
    $this->defineConstantDefaults(
      array('PAPAYA_DB_TBL_MODULES', 'PAPAYA_XSLT_EXTENSION')
   .md);
~~~~

This constant is used by the *papaya_xsl* class of which we are going to create a mock object as the module's layout object.

Now add the following unit test method above the *testGetPlanetsList()* method:

~~~~ {.PHP}
  /**
  * @covers PlanetAdmin::getXml
  */
  public function testGetXml() {
    $adminObject = $this->getPlanetAdminObjectFixture();
    $layout = $this->getMock('papaya_xsl', array('addLeft'));
    $layout
      ->expects($this->once())
      ->method('addLeft');
    $connectorObject = $this->getMock('HelloConnector');
    $connectorObject
      ->expects($this->once())
      ->method('getAllPlanets')
      ->will($this->returnValue(array()));
    $adminObject->setConnectorObject($connectorObject);
    $adminObject->getXml($layout);
  }
~~~~

Note that this test does not contain any *assert...()* call. This is perfectly legitimate as the *getXml()* method does not return a value. There are two assertions in this test, though, as each *expects()* statement is an assertion as well. The assertion for the layout object does not have a *will()* clause because the *addLeft()* method expected here does not return a value either.

The corresponding implementation which can be added above the *getPlanetsList()* method in the admin class reads as follows:

~~~~ {.PHP}
  /**
  * Get the module's GUI XML
  *
  * @param papaya_xsl $layout
  */
  public function getXml($layout) {
    $layout->addLeft($this->getPlanetsList());
  }
~~~~

The layout object's *addLeft()* method adds the content to the left, fixed-width column of the content area.

At this stage, you can even perform your first browser test of the admin module. To do this, you just need to add stubs for the *execute()* and *getButtons()* methods that are invoked by the *edmodule_...* class. Put *execute()* above the *getXml()* method, and *getButtons()* above *setPluginloaderObject()*:

~~~~ {.PHP}
  /**
  * Execute the module's commands
  */
  public function execute() {
    $this->initializeParams();
    // TO DO: Add implementation
  }

  /**
  * Get the main toolbar
  */
  public function getButtons() {
    // TO DO: Add implementation
  }
~~~~

And make sure you add some planets to the database table using your favorite database administration tool if you haven't already done so during the previous tutorial.

Then you can reload the module list in the papaya backend and start the application by choosing *Applications \> Hello World Planet Administration*. You will see your list of planets, and if you click on one, the page will reload and the selected planet will remain highlighted. This is done using the *initializeParams()* call in *execute()* to load the request parameters, and the *selected="selected"* attribute for the matching entry of the list view.

### Adding a form to create/edit planets

Next up we need a dialog, or web form, to add new or edit existing planets. The only interactive element in this form is a simple text field to enter or modify a planet's name. Web forms in papaya CMS can be built using the *base_dialog* class, although new, improved, and fully unit-tested dialog classes are on the way and will be featured in an updated version of this tutorial.

A *base_dialog* object is built using three arrays: the interactive fields, the default data for the fields' contents, and the hidden fields. The structure for the fields is identical with the edit fields for a content module as described in the previous tutorial. Default data and hidden fields are just plain associative arrays in which the keys are the field names and the values are the fields' values. The constructor of *base_dialog* takes in two more arguments: A reference to the caller class (*\$this*) and the current parameter namespace (usually *\$this-\>paramName* or *\$this-\>_owner-\>paramName* for content worker classes like *HelloPageBase*).

We are using one more pair of setter/getter methods for the dialog object. Add the following code to the unit test class for these methods (below all other test methods):

~~~~ {.PHP}
  /**
  * @covers PlanetAdmin::setDialogObject
  */
  public function testSetDialogObject() {
    $adminObject = $this->getPlanetAdminObjectFixture();
    $dialogObject = $this->getMock(
      'base_dialog',
      array(),
      array(),
      'Mock_'.md5(__CLASS__.microtime()),
      FALSE
   .md);
    $adminObject->setDialogObject($dialogObject, TRUE);
    $this->assertAttributeSame($dialogObject, '_dialogObject', $adminObject);
    $this->assertAttributeEquals(TRUE, '_dialogInitialized', $adminObject);
  }

  /**
  * @covers PlanetAdmin::getDialogObject
  */
  public function testGetDialogObject() {
    $this->markTestSkipped();
  }
~~~~

Yes, that's right: We write a test for *getDialogObject()* in which we officially declare the test skipped. This is to make perfectly clear that we did not leave this method untested by accident but because it cannot be tested with the current *base_dialog* implementation.

In the admin class, we need to declare two more private attributes (add the code below the declaration of the connector object):

~~~~ {.PHP}
  /**
  * Dialog object
  * @var base_dialog
  */
  private $_dialogObject = NULL;

  /**
  * Has the dialog already been initialized?
  * @var boolean
  */
  private $_dialogInitialized = FALSE;
~~~~

The *\$this-\>_dialogInitialized* attribute is used to make sure the dialog is not accidentally initialized twice (once for saving data and once for displaying the dialog). It would also be possible to simply check whether the *\$this-\>_dialogObject* attribute is already an object, but as we need to set it from exterior for the unit test, a check for this would prevent most of the method from being executed by the test.

Now add the following methods below any other method in the admin class:

~~~~ {.PHP}
  /**
  * Set the dialog object to be used
  *
  * @param base_dialog $dialogObject
  * @param boolean $initialized optional, default FALSE
  */
  public function setDialogObject($dialogObject, $initialized = FALSE) {
    $this->_dialogObject = $dialogObject;
    $this->_dialogInitialized = $initialized;
  }

  /**
  * Get (and, if necessary, initialize) the dialog object
  *
  * @param array $fields interactive form fields
  * @param array $data default data for the form fields
  * @param array $hidden hidden fields
  * @return base_dialog
  */
  public function getDialogObject($fields, $data, $hidden) {
    if (!is_object($this->_dialogObject)) {
      include_once(PAPAYA_INCLUDE_PATH.'system/base_dialog.php');
      $this->_dialogObject = new base_dialog($this, $this->paramName, $fields, $data, $hidden);
    }
    return $this->_dialogObject;
  }
~~~~

The contents of the dialog will be set using a method called *initializeDialog()*. This method can be called prepare the dialog for both input checks before saving data and displaying the dialog. Add the following unit test under *testGetPlanetsList()*:

~~~~ {.PHP}
  /**
  * @covers PlanetAmdin::initializeDialog
  */
  public function testInitializeDialog() {
    $adminObject = $this->getPlanetAdminObjectFixture();
    $dialogObject = $this->getMock(
      'base_dialog',
      array('loadParams'),
      array(),
      'Mock_'.md5(__CLASS__.microtime()),
      FALSE
   .md);
    $adminObject->setDialogObject($dialogObject);
    $connectorObject = $this->getMock('HelloConnector');
    $connectorObject
      ->expects($this->once())
      ->method('getPlanetById')
      ->will($this->returnValue(array('planet_id' => 2, 'planet_name' => 'Mars')));
    $adminObject->setConnectorObject($connectorObject);
    $adminObject->params = array('id' => 2);
    $adminObject->initializeDialog(FALSE);
    $this->assertAttributeEquals(TRUE, '_dialogInitialized', $adminObject);
  }
~~~~

Here's the *initializeDialog()* method you need to add to the admin class:

~~~~ {.PHP}
  /**
  * Initialze the dialog
  *
  * @param boolean $new TRUE => new planet, FALSE => edit existing planet
  */
  public function initializeDialog($new) {
    if (!$this->_dialogInitialized) {
      $fields = array('planet_name' => array('Name', 'isNoHTML', TRUE, 'input', 100));
      $data = array();
      $hidden = array('cmd' => 'save_planet');
      if (!$new && isset($this->params['id'])) {
        $connectorObject = $this->getConnectorObject();
        $planetData = $connectorObject->getPlanetById($this->params['id']);
        if (!empty($planetData)) {
          $hidden['id'] = $this->params['id'];
          $data = array('planet_name' => $planetData['planet_name']);
        }
      }
      $dialogObject = $this->getDialogObject($fields, $data, $hidden);
      if (is_object($dialogObject)) {
        $this->_dialogInitialized = TRUE;
        $dialogObject->buttonTitle = $this->_gt('Save');
        $dialogObject->dialogTitle = $this->_gt('Add planet');
        if (!$new && isset($this->params['id'])) {
          $dialogObject->dialogTitle = $this->_gt('Edit planet');
        }
        $dialogObject->loadParams();
      }
    }
  }
~~~~

The *\$new* parameter determines whether we use the dialog to add a new planet or to edit an existing planet. By setting it to FALSE in the unit test we make sure all of the method's lines of code are covered by it.

Next up is the method to display the dialog's XML output. Add the following test method to the unit test class:

~~~~ {.PHP}
  /**
  * @covers PlanetAdmin::getDialog
  */
  public function testGetDialog() {
    $adminObject = $this->getPlanetAdminObjectFixture();
    $dialogObject = $this->getMock(
      'base_dialog',
      array('getDialogXML'),
      array(),
      'Mock_'.md5(__CLASS__.  microtime()),
      FALSE
   .md);
    $dialogObject
      ->expects($this->once())
      ->method('getDialogXML')
      ->will($this->returnValue('<dialog />'));
    $adminObject->setDialogObject($dialogObject, TRUE);
    $this->assertEquals('<dialog />', $adminObject->getDialog(FALSE));
  }
~~~~

Note how setting the *\$this-\>_dialogInitialized* attribute to TRUE (by passing the value via *setDialogObject()*) prevents us from having to mock all the necessary objects and methods for *initializeDialog()* -- the working body of this method simply won't be executed.

The *getDialog()* implementation itself is pretty straightforward:

~~~~ {.PHP}
  /**
  * Get the dialog's XML output
  *
  * @param boolean $new TRUE => new planet, FALSE => edit existing planet
  */
  public function getDialog($new) {
    $result = '';
    $this->initializeDialog($new);
    if (is_object($this->_dialogObject)) {
      $result = $this->_dialogObject->getDialogXML();
    }
    return $result;
  }
~~~~

Our dialog is ready to be used, so we can add the invocations to *getXml()*. As usual, expand the unit test first. Replace the existing *testGetXml()* implementation by this one:

~~~~ {.PHP}
  /**
  * @covers PlanetAdmin::getXml
  * @dataProvider cmdProvider
  */
  public function testGetXml($cmd) {
    $adminObject = $this->getPlanetAdminObjectFixture();
    $layout = $this->getMock('papaya_xsl', array('addLeft', 'add'));
    $layout
      ->expects($this->once())
      ->method('addLeft');
    $layout
      ->expects($this->once())
      ->method('add');
    $connectorObject = $this->getMock('HelloConnector');
    $connectorObject
      ->expects($this->once())
      ->method('getAllPlanets')
      ->will($this->returnValue(array()));
    $adminObject->setConnectorObject($connectorObject);
    $dialogObject = $this->getMock(
      'base_dialog',
      array('getDialogXML'),
      array(),
      'Mock_'.md5(__CLASS__.microtime()),
      FALSE
   .md);
    $dialogObject
      ->expects($this->once())
      ->method('getDialogXML')
      ->will($this->returnValue('<dialog />'));
    $adminObject->setDialogObject($dialogObject, TRUE);
    $adminObject->params = array('cmd' => $cmd);
    $adminObject->getXml($layout);
  }
~~~~

The *@dataProvider* annotation determines the name of a data provider method. This method can be used to set different values for the argument(s) of the test class, resulting in more than one run of the test for different value sets. The data provider itself is a public, static method that returns an array of value arrays to be used in a row. Add it to the end of the test class body:

~~~~ {.PHP}
  /**
  * Data provider for commands
  */
  public static function cmdProvider() {
    return array(
      array('add_planet'),
      array('edit_planet')
   .md);
  }
~~~~

The *testGetXml()* method will be executed two times, using the *add_planet* and *edit_planet* commands, respectively.

*getXml()* itself needs to be expanded to look like this:

~~~~ {.PHP}
  /**
  * Get the module's GUI XML
  *
  * @param papaya_xsl $layout
  */
  public function getXml($layout) {
    $layout->addLeft($this->getPlanetsList());
    if (isset($this->params['cmd'])) {
      switch ($this->params['cmd']) {
      case 'add_planet':
        $layout->add($this->getDialog(TRUE));
        break;
      case 'edit_planet':
        $layout->add($this->getDialog(FALSE));
        break;
      }
    }
  }
~~~~

Now click on a planet in the planets list of the application. You will see a form entitled *Edit planet* with the planet's name in the text field. If you change the planet's name and click on *Save*, though, nothing will happen. We are going to implement the saving functionality in the next subsection.

### Executing the save command

When the form is submitted, the cmd parameter has the value 'save_planet'. The *execute()* method needs to check the cmd parameter and call the *savePlanet()* method when this value is set. In this subsection, we implement *savePlanet()* and its invocations for both new and existing planets.

There are three different test methods for *savePlanet()* to cover its different behaviors. Before adding these tests, we need to alter the testing environment. First things first, add the following include statement below the other *require_once()* calls:

~~~~ {.PHP}
require_once(PAPAYA_INCLUDE_PATH.'system/sys_error.php');
~~~~

This file defines some error level constants. *defineConstantDefaults()* won't help in this case because the class is required by other dependencies, resulting in a nasty *Constant already defined* error message when running test test suite.

Next up, add the following code at the beginning of the *PlanetAdminProxy* class definition:

~~~~ {.PHP}
  public $msgs = array();

  public function addMsg($level, $msg) {
    $this->msgs[] = $msg;
  }
~~~~

The *addMsg()* method is used by backend admin classes to issue success, warning, and error messages to the editor or administrator. In the proxy class, we use a public array to collect these messages. This makes them easily accessible for our assertions.

Next, you can add the following three test methods above *testGetPlanetsList()*:

~~~~ {.PHP}
  /**
  * @covers PlanetAdmin::savePlanet
  */
  public function testSavePlanetWithDialogError() {
    $adminObject = $this->getPlanetAdminObjectFixture();
    $dialogObject = $this->getMock(
      'base_dialog',
      array('checkDialogInput', 'getDialogXML'),
      array(),
      'Mock_'.md5(__CLASS__.microtime()),
      FALSE
   .md);
    $dialogObject
      ->expects($this->once())
      ->method('checkDialogInput')
      ->will($this->returnValue(FALSE));
    $dialogObject
      ->expects($this->once())
      ->method('getDialogXML')
      ->will($this->returnValue('<dialog />'));
    $adminObject->setDialogObject($dialogObject, TRUE);
    $layout = $this->getMock('papaya_xsl');
    $adminObject->layout = $layout;
    $adminObject->params = array('invalid_field' => 'invalid data');
    $adminObject->savePlanet();
    $this->assertEquals(
      'Input errors.',
      $adminObject->msgs[0]
   .md);
  }

  /**
  * @covers PlanetAdmin::savePlanet
  */
  public function testSavePlanetNewSuccess() {
    $adminObject = $this->getPlanetAdminObjectFixture();
    $dialogObject = $this->getMock(
      'base_dialog',
      array('checkDialogInput'),
      array(),
      'Mock_'.md5(__CLASS__.microtime()),
      FALSE
   .md);
    $dialogObject
      ->expects($this->once())
      ->method('checkDialogInput')
      ->will($this->returnValue(TRUE));
    $adminObject->setDialogObject($dialogObject, TRUE);
    $connectorObject = $this->getMock('HelloConnector');
    $connectorObject
      ->expects($this->once())
      ->method('createPlanet')
      ->will($this->returnValue(TRUE));
    $adminObject->setConnectorObject($connectorObject);
    $adminObject->params = array('planet_name' => 'Neptune');
    $adminObject->savePlanet();
    $this->assertEquals(
      'Planet successfully saved.',
      $adminObject->msgs[0]
   .md);
  }

  /**
  * @covers PlanetAdmin::savePlanet
  */
  public function testSavePlanetExistingFailure() {
    $adminObject = $this->getPlanetAdminObjectFixture();
    $dialogObject = $this->getMock(
      'base_dialog',
      array('checkDialogInput'),
      array(),
      'Mock_'.md5(__CLASS__.microtime()),
      FALSE
   .md);
    $dialogObject
      ->expects($this->once())
      ->method('checkDialogInput')
      ->will($this->returnValue(TRUE));
    $adminObject->setDialogObject($dialogObject, TRUE);
    $connectorObject = $this->getMock('HelloConnector');
    $connectorObject
      ->expects($this->once())
      ->method('updatePlanet')
      ->will($this->returnValue(FALSE));
    $adminObject->setConnectorObject($connectorObject);
    $adminObject->params = array(
      'cmd' => 'save_planet',
      'id' => 1,
      'planet_name' => 'Earth'
   .md);
    $adminObject->savePlanet();

    $this->assertEquals(
      'Planet could not be saved: database error.',
      $adminObject->msgs[0]
   .md);
  }
~~~~

The corresponding implementation of the *savePlanet()* method (to be inserted above *getPlanetsList()*) looks like this:

~~~~ {.PHP}
  /**
  * Save a planet
  */
  public function savePlanet() {
    $new = TRUE;
    if (isset($this->params['id'])) {
      $new = FALSE;
    }
    $this->initializeDialog($new);
    if ($this->_dialogObject->checkDialogInput()) {
      $connectorObject = $this->getConnectorObject();
      if ($new) {
        $success = $connectorObject->createPlanet(
          array('planet_name' => $this->params['planet_name'])
       .md);
      } else {
        $success = $connectorObject->updatePlanet(
          $this->params['id'],
          array('planet_name' => $this->params['planet_name'])
       .md);
      }
      if ($success) {
        $this->addMsg(MSG_INFO, $this->_gt('Planet successfully saved.'));
      } else {
        $this->addMsg(MSG_ERROR, $this->_gt('Planet could not be saved: database error.'));
      }
    } else {
      $this->addMsg(MSG_ERROR, $this->_gt('Input errors.'));
      $this->layout->add($this->getDialog($new));
    }
  }
~~~~

Now that the saving method is implemented, we want *execute()* to invoke it whenever the *save_planet* command is set. First, add the following lines to the *PlanetAdminProxy* class definition to circumvent the side effects of the original *initializeParams()* method for unit tests:

~~~~ {.PHP}
  public function initializeParams() {
    // Nothing to do here; just override the original
  }
~~~~

*execute()* will have several unit test methods, one for each command. Here's the first one that covers the *save_planet* command:

~~~~ {.PHP}
  /**
  * @covers PlanetAdmin::execute
  */
  public function testExecuteSave() {
    $adminObject = $this->getPlanetAdminObjectFixture();
    $dialogObject = $this->getMock(
      'base_dialog',
      array('checkDialogInput', 'getDialogXML'),
      array(),
      'Mock_'.md5(__CLASS__.microtime()),
      FALSE
   .md);
    $dialogObject
      ->expects($this->once())
      ->method('checkDialogInput')
      ->will($this->returnValue(FALSE));
    $dialogObject
      ->expects($this->once())
      ->method('getDialogXML')
      ->will($this->returnValue('<dialog />'));
    $adminObject->setDialogObject($dialogObject, TRUE);
    $layout = $this->getMock('papaya_xsl');
    $adminObject->layout = $layout;
    $adminObject->params = array(
      'cmd' => 'save_planet',
      'invalid_field' => 'invalid data'
   .md);
    $adminObject->execute();
    $this->assertEquals('Input errors.', $adminObject->msgs[0]);
  }
~~~~

As you can see, the largest part of the method provides the environment for the *savePlanet()* call. As the case with the invalid dialog input is the shortest, I have chosen this one, but any of the cases covered by the *testSavePlanet...()* methods would work.

The implementation of *execute()* itself is pretty short at this point:

~~~~ {.PHP}
  /**
  * Execute the module's commands
  */
  public function execute() {
    $this->initializeParams();
    if (isset($this->params['cmd'])) {
      switch ($this->params['cmd']) {
      case 'save_planet':
        $this->savePlanet();
        break;
      }
    }
  }
~~~~

When you click on a planet in the administration interface, you can successfully change its name now. But we don't have a button to create a new planet yet. So next up is the toolbar.

### Building the main toolbar

The *getButtons()* method is used to provide the main toolbar which is displayed above the application's main content area. It uses an instance of the *base_btnbuilder* class from *papaya-lib/system*. This class provides the *addButton()* and *addSeparator()* methods to add buttons and vertical separators, respectively. *addButton()* has the following syntax:

~~~~ {.PHP}
$btnbuilderObject->addButton($caption, $href, $img, $hint, $down, $noTranslation);
~~~~

There are up to six arguments of which the last four are optional:

-   *string \$caption* -- the caption to be displayed on the button
-   *string \$href* -- the link for the page to be displayed when the button is clicked, usually built using the *getLink()* method
-   *string \$img* -- the icon or glyph to be displayed on the button, an index of the *\$this-\>images* attribute. You can see all available images in the backend if you click on *Settings \> View icons*.
-   *string \$hint* -- the tooltip to be displayed on mouseover
-   *boolean \$down* -- whether the button should be displayed as clicked down (*TRUE*) or not (*FALSE*, which is the default). Usually, this depends on whether the command represented by a button is currently active.
-   *boolean \$noTranslation* -- whether the caption should be automatically translated (*FALSE*, default) or not (*TRUE*)

*addSeparator()* does not take in any arguments. It should be used to separate logical groups within the toolbar.

After adding all necessary buttons and separators, call the *base_btnbuilder* object's *getXML()* method to get the XML representation of the toolbar's contents. Once the XML is fetched, you can add it to the layout object using the *addMenu()* method. Here's the unit test method for *getButtons()*:

~~~~ {.PHP}
  /**
  * @covers PlanetAdmin::getButtons
  */
  public function testGetButtons() {
    $adminObject = $this->getPlanetAdminObjectFixture();
    $layout = $this->getMock('papaya_xsl', array('addMenu'));
    $layout
      ->expects($this->once())
      ->method('addMenu');
    $adminObject->layout = $layout;
    $adminObject->params = array('cmd' => 'del_planet', 'id' => 4);
    $adminObject->images = array(
      'actions-generic-add' => 'actions/generic-add.png',
      'actions-generic-delete' => 'actions/generic-delete.png'
   .md);
    $adminObject->getButtons();
  }
~~~~

And this is the implementation itself:

~~~~ {.PHP}
  /**
  * Get the main toolbar
  */
  public function getButtons() {
    include_once(PAPAYA_INCLUDE_PATH.'system/base_btnbuilder.php');
    $toolbar = new base_btnbuilder();
    $toolbar->images = &$this->images;
    $cmd = (isset($this->params['cmd'])) ? $this->params['cmd'] : '';
    $pushed = ($cmd == 'add_planet') ? TRUE : FALSE;
    $toolbar->addButton(
      'Add planet',
      $this->getLink(array('cmd' => 'add_planet')),
      'actions-generic-add',
      'Add a new planet',
      $pushed
   .md);
    if (isset($this->params['id'])) {
      $toolbar->addSeparator();
      $pushed = ($cmd == 'del_planet') ? TRUE : FALSE;
      $toolbar->addButton(
        'Delete planet',
        $this->getLink(array('cmd' => 'del_planet', 'id' => $this->params['id'])),
        'actions-generic-delete',
        'Delete current planet',
        $pushed
     .md);
    }
    if ($str = $toolbar->getXML()) {
      $this->layout->addMenu(sprintf('<menu ident="%s">%s</menu>'.LF, 'edit', $str));
    }
  }
~~~~

As you can see, the *Delete planet* button is only displayed when there's an *id* parameter for a current planet.

### Executing the delete command

The last feature we need to implement is the deletion of a planet. We don't want users to delete data with a single click, so we add a security question. It is provided by the *base_msgdialog* class from *papaya-lib/system*. Much like *base_dialog*, this class cannot be unit-tested and is currently being replaced by a better, test-friendly implementation.

So you just need to add a very short unit test (or rather a non-test) for the *getDeleteDialog()* method:

~~~~ {.PHP}
  /**
  * @covers PlanetAdmin::getDeleteDialog
  */
  public function testGetDeleteDialog() {
    $this->markTestSkipped();
  }
~~~~

Here's the *getDeleteDialog()* method; place it above *getPlanetsList()* method:

~~~~ {.PHP}
  /**
  * Get the delete confirmation dialog
  */
  public function getDeleteDialog() {
    if (isset($this->params['id']) && !isset($this->params['confirm_delete'])) {
      $dialog = new base_msgdialog(
        $this,
        $this->paramName,
        array('cmd' => 'del_planet', 'id' => $this->params['id'], 'confirm_delete' => 1),
        'Really delete current planet?',
        'question'
     .md);
      $this->layout->add($dialog->getMsgDialog());
    }
  }
~~~~

The method will be called by *getXml()* when the *del_planet* command is set. It will only display the dialog if the *confirm_delete* parameter that it provides a link for is not present.

The delete method itself uses the connector to delete the current planet. Here are two unit tests to cover both possible outcomes:

~~~~ {.PHP}
  /**
  * @covers PlanetAdmin::deletePlanet
  */
  public function testDeletePlanetSuccess() {
    $adminObject = $this->getPlanetAdminObjectFixture();
    $connectorObject = $this->getMock('HelloConnector');
    $connectorObject
      ->expects($this->once())
      ->method('deletePlanet')
      ->will($this->returnValue(TRUE));
    $adminObject->setConnectorObject($connectorObject);
    $adminObject->params = array('id' => 4, 'confirm_delete' => 1);
    $adminObject->deletePlanet();
    $this->assertEquals(
      'Planet successfully deleted.',
      $adminObject->msgs[0]
   .md);
  }

  /**
  * @covers PlanetAdmin::deletePlanet
  */
  public function testDeletePlanetError() {
    $adminObject = $this->getPlanetAdminObjectFixture();
    $connectorObject = $this->getMock('HelloConnector');
    $connectorObject
      ->expects($this->once())
      ->method('deletePlanet')
      ->will($this->returnValue(FALSE));
    $adminObject->setConnectorObject($connectorObject);
    $adminObject->params = array('id' => 7, 'confirm_delete' => 1);
    $adminObject->deletePlanet();
    $this->assertEquals(
      'Could not delete planet.',
      $adminObject->msgs[0]
   .md);
  }
~~~~

Next, add the *deletePlanet()* method above *getDeleteDialog()*:

~~~~ {.PHP}
  /**
  * Delete the current planet
  */
  public function deletePlanet() {
    if (isset($this->params['id']) && isset($this->params['confirm_delete']) &&
        $this->params['confirm_delete'] == 1) {
      $connectorObject = $this->getConnectorObject();
      $success = $connectorObject->deletePlanet($this->params['id']);
      if ($success) {
        $this->addMsg(MSG_INFO, $this->_gt('Planet successfully deleted.'));
      } else {
        $this->addMsg(MSG_ERROR, $this->_gt('Could not delete planet.'));
      }
    }
  }
~~~~

Now we need to make sure that *deletePlanet()* is called by *execute()* and *getDeleteDialog()* by *getXml()*. For the former, add another *execute()* unit test below the first one:

~~~~ {.PHP}
  /**
  * @covers PlanetAdmin::execute
  */
  public function testExecuteDelete() {
    $adminObject = $this->getPlanetAdminObjectFixture();
    $connectorObject = $this->getMock('HelloConnector');
    $connectorObject
      ->expects($this->once())
      ->method('deletePlanet')
      ->will($this->returnValue(TRUE));
    $adminObject->setConnectorObject($connectorObject);
    $adminObject->params = array('cmd' => 'del_planet', 'id' => 5, 'confirm_delete' => 1);
    $adminObject->execute();
    $this->assertEquals('Planet successfully deleted.', $adminObject->msgs[0]);
  }
~~~~

Expand the *execute()* method to look like this:

~~~~ {.PHP}
  /**
  * Execute the module's commands
  */
  public function execute() {
    $this->initializeParams();
    if (isset($this->params['cmd'])) {
      switch ($this->params['cmd']) {
      case 'save_planet':
        $this->savePlanet();
        break;
      case 'del_planet':
        $this->deletePlanet();
        break;
      }
    }
  }
~~~~

To test the *getXml()* extension without hitting the untestable *base_msgdialog* object, we simply set the *confirm_delete* parameter. As *del_planet* behaves unlike the other commands, we do not expand the existing test using the data provider but write an additional test:

~~~~ {.PHP}
  /**
  * @covers PlanetAdmin::getXml
  */
  public function testGetXmlDelete() {
    $adminObject = $this->getPlanetAdminObjectFixture();
    $layout = $this->getMock('papaya_xsl', array('addLeft'));
    $layout
      ->expects($this->once())
      ->method('addLeft');
    $connectorObject = $this->getMock('HelloConnector');
    $connectorObject
      ->expects($this->once())
      ->method('getAllPlanets')
      ->will($this->returnValue(array()));
    $adminObject->setConnectorObject($connectorObject);
    $adminObject->params = array('cmd' => 'del_planet', 'id' => 5, 'confirm_delete' => 1);
    $adminObject->getXml($layout);
  }
~~~~

As a final step, we expand *getXml()*:

~~~~ {.PHP}
  /**
  * Get the module's GUI XML
  *
  * @param papaya_xsl $layout
  */
  public function getXml($layout) {
    $layout->addLeft($this->getPlanetsList());
    if (isset($this->params['cmd'])) {
      switch ($this->params['cmd']) {
      case 'add_planet':
        $layout->add($this->getDialog(TRUE));
        break;
      case 'edit_planet':
        $layout->add($this->getDialog(FALSE));
        break;
      case 'del_planet':
        $this->getDeleteDialog();
        break;
      }
    }
  }
~~~~