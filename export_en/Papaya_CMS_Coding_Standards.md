---
title: Papaya CMS Coding Standards
permalink: /Papaya_CMS_Coding_Standards/
---

<div id="navigation">
1.  <a href="#commitmessages">Commit Messages</a>
2.  <a href="#encoding">Encoding</a>
3.  <a href="#indent">Indent / Line Length</a>
4.  <a href="#structures">Control Structures</a>
5.  <a href="#functioncall">Function Calls</a>
6.  <a href="#functiondef">Function Definitions</a>
7.  <a href="#comments">Comments</a>
8.  <a href="#include">Including Code</a>
9.  <a href="#phpcodetags">PHP Code Tags</a>
10. <a href="#naming">Naming Conventions</a>
11. <a href="#defaultclass">Default Class Template</a>
12. <a href="#strings">Strings</a>
13. <a href="#sql">SQL</a>
14. <a href="#css">CSS</a>
15. <a href="#xslt">XSLT</a>

`       This document inherits from the `<a href="http://pear.php.net/manual/en/standards.php">`Pear Coding Standard`</a>`. Parts are copied and modified.`
`     `

</div>
<div id="content">
<h2>
Commit Messages<a name="commitmessages"></a>

</h2>
`       All messages should wrap at 79 characters per line. This means, if you are`
`       writing multiple lines after a message starting with a "- " each following`
`       line should be indented by exactly two spaces.`
`       Including descriptive text in your commit messages is generally important to`
`       offer a good overview on the commit when the issue tracker is not available`
`       (commit mails, history).`
`     `

`       All messages may include references to existing issues to add status updates`
`       to the issue, which should look like:`
`     `

`       ``- Refs MANTIS-<number>: <text>`
`     `

`       Where `<number>` references the ticket and the `<text>` describes what you did.`
`       The ticket numer has no leading zeros.`
`     `

<h3>
Keywords

</h3>
<table>
<thead>
<tr>
<th>
Keyword

</th>
<th>
Ticket

</th>
<th>
Usage

</th>
</tr>
<tr>
<td>
Added

</td>
<td>
no

</td>
<td>
Added file, class, method, ...

</td>
</tr>
<tr>
<td>
Documented

</td>
<td>
optional

</td>
<td>
Added or changed source documentation

</td>
</tr>
<tr>
<td>
Fixed

</td>
<td>
yes

</td>
<td>
Fixed a bug

</td>
</tr>
<tr>
<td>
Implemented

</td>
<td>
optional

</td>
<td>
Implemented some new logic/feature

</td>
</tr>
<tr>
<td>
Refs

</td>
<td>
yes

</td>
<td>
Reference to a bug ticket. This commit has dependencies to the ticket.

</td>
</tr>
<tr>
<td>
Tested

</td>
<td>
no

</td>
<td>
Added unit/integration test for classes/methods

</td>
</tr>
<tr>
<td>
Translated

</td>
<td>
no

</td>
<td>
Translated some text

</td>
</tr>
</thead>
</table>
<h3>
Sample

</h3>
    - Added: PapayaCommitMessageParser for commit message checks
    - Fixed MANTIS-10815: because it was here and should not

<h3>
Comments

</h3>
`       You may always append arbitrary comments in your commit messages, where each`
`       line should start with a number sign (#). Text in these lines won't be`
`       checked.`
`     `

<h3>
Bug fix

</h3>
`       A bug fix commit message should follow the following scheme::`
`     `

`       ``- Fixed MANTIS-<number>: <text>`
`     `

`       Where `<number>` references the closed bug and `<text>` is a description of the`
`       bug and the fix. Keep in mind that the texts will be used for the changelog,`
`       so please check the spelling before committing.`
`     `

`       The bug number is not optional, which means that there should be an open bug`
`       in the issue tracker for *each* bug you fix.`
`     `

`       For compatibility with other issue tracker you may also use "Closed" instead`
`       of "Fixed" in your message, but "Fixed" is highly preferred.`
`     `

<h3>
New features

</h3>
If you implemented a new feature, your commit message should look like::

`       ``- Implemented[ MANTIS-<number>]: <text>`
`     `

`       Where `<text>` is a short description of the feature you implemented, and`
`       `<number>` may optionally reference a feature request in the bug tracker. Keep`
`       in mind that the texts will be used for the changelog, so please check the`
`       spelling before committing.`
`     `

<h3>
Documentation

</h3>
`       If you extended your documentation, your commit message should look like::`
`     `

`       ``- Documented[ MANTIS-<number>]: <text>`
`     `

`       Where `<number>` optionally specifies a documentation request, and the text`
`       describes what you documented.`
`     `

<h3>
Additional tests

</h3>
`       If you added tests for some feature, your commit message should look like::`
`     `

`       ``- Tested: <text>`
`     `

`       Where `<text>` describes the feature(s) you are testing.`
`     `

<h3>
Other commits

</h3>
`       If your commit does not match any of the above rules you should only include a`
`       comment in your commit message or extend this document with your commit`
`       message of desire.`
`     `

<h3>
General Grammar

</h3>
    Message       ::= Statement+ | Statement* Comment+
    Statement     ::= Reference | Fixed | Implemented | Documented | Tested
                      | Added | Translated
    Comment       ::= '# ' TextLine | '#\n'
    Reference     ::= '- Refs'         BugNr  ': ' TextLine Text?
    Fixed         ::= '- ' FixedString BugNr  ': ' TextLine Text?
    Implemented   ::= '- Implemented'  BugNr? ': ' TextLine Text?
    Documented    ::= '- Documented'   BugNr? ': ' TextLine Text?
    Tested        ::= '- Tested: '                 TextLine Text?
    Added         ::= '- Added: '                  TextLine Text?
    Translated    ::= '- Translated: '             TextLine Text?
    FixedString   ::= 'Fixed' | 'Closed'
    Text          ::= '  ' TextLine Text?
    BugNr         ::= ' MANTIS-' [1-9]+[0-9]*
    TextLine      ::= [\x20-\x7E]+ "\n"

<h2>
Encoding<a name="encoding"></a>

</h2>
`       Use only ASCII chars in your PHP source documents. If you need to write chars in other encodings use escaping (in PHP strings/constants) or translitaration (in comments or names).`
`     `

    define(
      'PAPAYA_CHECKIT_WORDCHARS',
      "\xc3\x80-\xc3\x8f\xc3\x91-\xc3\x96\xc3\x98-\xc3\xb6\xc3\xb8-\xc3\xbf"
    );

The language of the PHP source documents is English, do not use other languages for names or comments.

<h2>
Indent / Line Length<a name="indent"></a>

</h2>
`       Use an indent of 2 spaces, with no tabs.  It is recommended that you break lines at approximately 100 characters. There is no standard rule for the best way to break a line, use your judgment and, when in doubt, ASK.`
`     `

`       If your editor can be configured to remove spaces from the end of a line when you save a file, please do so. This prevents unnecessary differences in file versions.`
`     `

`     `<a href="#top" class="top">`Top`</a>
`     `

<h2>
Control Structures<a name="structures"></a>

</h2>
These include `if`, `for`, `while`, `switch`, etc. Here is an example `if` statement, since it is the most complicated of them:

    <?php
    if ((condition1) || (condition2)) {
      action1();
    } elseif ((condition3) && (condition4)) {
      action2();
    } else {
      defaultaction();
    }
    ?>

If a condition is too long, it is suggested to break it after any boolean operator (`||`, `&&`) for faster comprehension.

Use of abbreviated `if` statments is acceptable if really short, i.e. it doesn't exceed <a href="#indent">line limit</a>:

    $selected = ($foo == $bar) ? ' selected="selected"' : '';

`       Control statements should have one space between the control keyword and opening parenthesis, to distinguish them from function calls.`
`     `

You are strongly encouraged to always use curly braces even in situations where they are technically optional. Having them increases readability and decreases the likelihood of logic errors being introduced when new lines are added.

`       For switch statements:`
`     `

    <?php
    switch (condition) {
    case 1:
      action1;
      break;
    case 2:
      action2;
      break;
    default:
      defaultaction;
      break;
    }
    ?>

`     `<a href="#top" class="top">`Top`</a>
`     `

<h2>
Function Calls<a name="functioncall"></a>

</h2>
`       Functions should be called with no spaces between the function name, the opening parenthesis, and the first parameter; spaces between commas and each parameter, and no space between the last parameter, the closing parenthesis, and the semicolon. Here's an example:`
`     `


    <?php
    $var = foo($bar, $baz, $quux);
    ?>

`       As displayed above, there should be one space on either side of an equals sign used to assign the return value of a function to a variable. In the case of a block of related assignments, do not add more spaces!`
`     `

    <?php
    $short = foo($bar);
    $longVariable = foo($baz);
    ?>


If parameter string of a function call is to long for a single line break after the first ( and before the last. If it is still to long break after each parameter.


    <?php
    $singleLineBreak = foo(
      $parameterOne, $parameterTwo, $parameterTree
    );
    $parameterLineBreak = foo(
      $parameterOne,
      $parameterTwo,
      $parameterTree,
      $parameterFour,
      $parameterFive
    );
    $nestedFunctionsBreak = fooBar(
      bar(
        foo(
          $parameterOne, $parameterTwo, $parameterTree
        )
      ),
      $fooBarParameter
    );
    ?>


`     `<a href="#top" class="top">`Top`</a>
`     `

<h2>
Function Definitions<a name="functiondef"></a>

</h2>
`       Function definitions are formatted like control stuctures:`
`     `


    <?php
    function fooFunction($arg1, $arg2 = '') {
      if (condition) {
        statement;
      }
      return $val;
    }
    ?>

Arguments with default values go at the end of the argument list. Always attempt to return a meaningful value from a function if one is appropriate.

`       See also: `<a href="#naming">`Naming Conventions`</a>
`     `

`     `<a href="#top" class="top">`Top`</a>

<h2>
Comments<a name="comments"></a>

</h2>
`       Complete inline documentation (in English) comment blocks (docblocks, see `<a href="http://manual.phpdoc.org/HTMLSmartyConverter/HandS/phpDocumentor/tutorial_tags.pkg.html">`phpdocumentor`</a>`) must be provided.`
`     `

`       Mandatory elements for classes/files are @package, @version $Id$, for functions/methods @param, @return. Consider using @example, inline {@link}, @internal, @link, @see, @since, @todo if suitable.`
`     `

`       The @author element must not be included in the docbook comment. We use the version control system to identifify the authors.`
`     `

`       Non-documentation comments are strongly encouraged. A general rule of thumb is that if you look at a section of code and think "Wow, I don't want to try and describe that", you need to comment it before you forget how it works.`
`     `

C style comments (/\* \*/) and standard C++ comments (//) are both fine. Use of Perl/shell style comments (\#) is discouraged.

`     `<a href="#top" class="top">`Top`</a>
`     `

<h2>
Including Code<a name="include"></a>

</h2>
`       Anywhere you are unconditionally including a class file, use require_once. Anywhere you are conditionally including a class file (for example, factory methods), use include_once. Either of these will ensure that class files are included only once. They share the same file list, so you don't need to worry about mixing them - a file included with require_once will not be included again by include_once.`
`     `

`       If the included file is a part of the base system use the ``PAPAYA_INLUDE_PATH``. If the File is a part of the same module use ``dirname(__FILE__)``. In all other cases ASK.`
`     `

`       `<b>`Note:`</b>` include_once and require_once are statements, not functions. It is not nessesary, but please always surround the subject filename with parentheses.`
`     `

    <?php
    require_once(PAPAYA_INCLUDE_PATH.'system/sys_base_object.php');
    include(dirname(__FILE__).'/foobar.php');
    ?>

`     `<a href="#top" class="top">`Top`</a>
`     `

<h2>
PHP Code Tags<a name="phpcodetags"></a>

</h2>
`       `<b>`Always`</b>` use `<?php ?>` to delimit PHP code, not the `<? ?>` shorthand and not the <% %> ASP style.`
`     `

`     `<a href="#top" class="top">`Top`</a>
`     `

<h2>
Naming Conventions <a name="naming"></a>

</h2>
`       Only use English and ASCII letters (no digits or special chars please). Avoid using abbreviations where possible.`
`     `

<h3>
Classes

</h3>
`       Classes should be given descriptive names. Class names should always be lowercase. Parts are seperated using the underscore.`
`     `

    <?php
    class foo_bar {
    }
    ?>

<h3>
Functions and Methods

</h3>
`       Functions and methods should be named using the "studly caps" style (also referred to as "bumpy case" or "camel caps"). Functions (not methods) should in addition have the module name as a prefix, to avoid name collisions between modules. The initial letter of the name (after the prefix) is lowercase, and each letter that starts a new "word" is capitalized. Some examples:`
`     `

1.  \$this-\>insertRecord()
2.  \$this-\>databaseQueryFmt()

Private class members (meaning class members that are intended to be used only from within the same class in which they are declared; PHP does not yet support truly-enforceable private namespaces) are preceded by a single underscore.

<h3>
Properties and Local Variables

</h3>
`       Functions and methods should be named using the "studly caps" style (also referred to as "bumpy case" or "camel caps"). The initial letter of the name (after the prefix) is lowercase, and each letter that starts a new "word" is capitalized. Some examples:`
`     `

1.  \$fooBar
2.  \$this-\>fooBar

Private class members (meaning class members that are intended to be used only from within the same class in which they are declared; PHP does not yet support truly-enforceable private namespaces) are preceded by a single underscore. Some examples:

1.  var \$_fooBar
2.  \$this-\>_fooBar

This rule does not apply to global variables.

<h3>
Global Variables

</h3>
Global Variables should be all-uppercase, with underscores to separate words. Please ASK before defining global variables.

<h3>
Constants

</h3>
Constants should always be all-uppercase, with underscores to separate words. Prefix constant names with the uppercased name of the module/package they are used in. Please ASK before defining constants.

1.  TRUE
2.  FALSE
3.  NULL
4.  PAPAYA_INCLUDE_PATH

<h3>
Database tables and fields

</h3>
`       Always use lowercase ascii letters seperated with underscores.`
`     `

1.  table_name
2.  field_name

<h3>
Array indices

</h3>
`       Use the same convention as for database fields, i.e. lowercase with underscores.`
`     `


    array(
      'some_value' => 10,
      'some_other_value' => 'id',
    );

<h3>
Dynamic array keys added to a database record array

</h3>
`       Always use uppercase ascii letters seperated with underscores.`
`     `

1.  \$this-\>records[\$id]['TRANSLATION']
2.  \$this-\>records[\$id]['PARENT_IDS']

<h3>
GET/POST/COOKIE parameters

</h3>
`       Always use lowercase ascii letters seperated with underscores.`
`     `

1.  paramname
2.  paramname_sub

<h3>
XML

</h3>
`       Seperate words of elements (tag names and attribute names) by a dash '-'. Exception: xml that goes into flash. Use an underscore in that case, since the flash xml parser is allergic to tagnames with a dash, it would die if you used it.`
`     `

    <code><my-element-name my-attribute-name="somevalue" /></code>

<h3>
Variable to use for a function/method return

</h3>
`       If you have a variable returned at the end of a function/method, name this variable ``$result``.`
`     `

`     `<a href="#top" class="top">`Top`</a>

<h2>
Default Class Template<a name="defaultclass"></a>

</h2>
`       Please use the following default class template. A constructor is only`
`       necessary if the class is not derived or something has to be initialized at`
`       construction of the object.`
`     `

    <code><?php
    /**
    * {%SHORT_CLASS_DESCRIPTION%}
    *
    * @copyright 2002-2007 by papaya Software GmbH - All rights reserved.
    * @link http://www.papaya-cms.com/
    * @licence   GNU General Public Licence (GPL) 2 http://www.gnu.org/copyleft/gpl.html
    *
    * You can redistribute and/or modify this script under the terms of the GNU General Public
    * License (GPL) version 2, provided that the copyright and license notes, including these
    * lines, remain unmodified. papaya is distributed in the hope that it will be useful, but
    * WITHOUT ANY WARRANTY; without even the implied warranty of MERCHANTABILITY or FITNESS
    * FOR A PARTICULAR PURPOSE.
    *
    [*
    * {%LONGER_DESCRIPTION_IF_NECESSARY%} ]
    *
    * @package {%PACKAGE_NAME%}
    * @version $Id$
    */

    {%NECESSARY_INCLUDES_AND_DEFINES_GO_HERE%}

    /**
    * {%SHORT_CLASS_DESCRIPTION%}
    *
    * @package {%PACKAGE_NAME%}
    */
    class {%CLASSNAME%} [extends {%PARENTCLASS%}] {

      /**
      * @var {%ATTRIBUTE_TYPE%} ${%ATTRIBUTE_NAME%} {%ATTRIBUTE_DESCRIPTION%}
      */
      var ${%ATTRIBUTE_NAME%} [= {%DEFAULT_VALUE%}];

      /**
      * {%METHOD_DESCRIPTION%}
    [ *
      * {%LONGER_DESCRIPTION_IF_NECESSARY%} ]
      *
      * @since {%FIRST_PAPAYA_CMS_VERSION_CONTAINING_THIS_METHOD%}
    [ * @param {%PARAMETER_TYPE%} $parameter {%DESCRIPTION_OF_PARAMETER%} ]
    [ * @return {%RESULT_TYPE%} {%DESCRIPTION_OF_RESULT%} ]
      */
      function __construct([$parameter [= {%DEFAULT_VALUE%}]]) {
        parent::__construct([$parameter]);
        // HERE GOES ALL THE INITIALIZATION STUFF
      }

      function {%CLASSNAME%}([$parameter [= {%DEFAULT_VALUE%}]]) {
        $this->__construct([$parameter [= {%DEFAULT_VALUE%}]]);
      }

      {%MORE_METHODS%}

    }
    ?></code>

`     `<a href="#top" class="top">`Top`</a>
`     `

<h2>
Strings<a name="strings"></a>

</h2>
`       Enclose strings using ' except the string contains many ' - as in SQL - or char codes (\n , \t, ...)`
`     `

<h3>
Regular Expressions (Regex)

</h3>
`       Enclose regular expressions with brackets: '(EXPR)'.`
`     `

`     `<a href="#top" class="top">`Top`</a>

<h2>
SQL<a name="sql"></a>

</h2>
`       Enclose SQL-Strings in double quotes ("). The single quote (') is used for strings inside SQL. Write reserved words and SQL functions uppercase. Insert spaces to align the first word of each line. Never use a star (*) to fetch all fields, state each field explicitely.`
`     `

    SELECT t1.field1, t2.field2
           COUNT(t2.field3) fieldalias
      FROM table1 AS t1
      LEFT JOIN table2 AS t2 ON (t1.field1 = t1.field2)
     WHERE field2 = 'value'
     GROUP BY t1.field1, t2.field2
     ORDER BY t1.field1 DESC, t2.field2 ASC

Escape all variables inserted in SQL queries. Use \$db-\>databaseGetSQLCondition() for conditions, use \$db-\>databaseQueryFmt(\$sql, \$params) for all other values and use \$db-\>escapeStr() if the previous methods are not possible.

Example:

    $condition = $this->databaseGetSQLCondition('item_type', $this->params['item_type']);
    $sql = "SELECT item_id, item_name, item_type
              FROM %s
             WHERE $condition
               AND item_id <= %s";
    $params = array($this->tableItems, (int)$this->params['max_id']);
    if ($this->databaseQueryFmt($sql, $params)) {
      while ($row = $res->fetchRow(DB_FETCHMODE_ASSOC)) {
        $result[$row['item_id']] = $row;
      }
    }

`       If you have to build your own UPDATE or INSERT queries and cannot use databaseInsertRecord or databaseUpdateRecord, make sure you call databaseQueryFmtWrite OR databaseQueryWrite to provide compatibility on multi-database installations.`
`     `

<h3>
Aliases

</h3>
`       If a query uses more than one table or database, use aliases. All field and table names must be prefixed by the correct alias.`
`     `

`     `<a href="#top" class="top">`Top`</a>
`     `

<h2>
CSS<a name="css"></a>

</h2>
`       CSS class names should be named using the "studly caps" style (also referred to as "bumpy case" or "camel caps"). The initial letter of the name (after the prefix) is lowercase, and each letter that starts a new "word" is capitalized. The class names should be semantic and without any "layout information".`
`     `

`       Each attribute must be on a seperate line. After the colon a space is required. The semicolon after the last attribute may not be left out.`
`     `

`       For consistency using hexadecimal RGB value (#3399FF) for colors is suggested. Most designers won't ever pick a named color anyway. The shorthand #RGB (e.g. #FFF for #FFFFFF or #900 for #990000) may be used as well.`
`     `

`       Some examples:`
`     `

    additionalContent {
      width: 100px;
      color: #333333;
    }
    contentSubTitle {

    }

Sizes should be in em and px. Do not use pt, except in print stylesheets.

`     `<a href="#top" class="top">`Top`</a>
`     `

<h2>
XSLT<a name="xslt"></a>

</h2>
`       Named templates are preferred, avoid match templates if possible.`
`     `

`       XSLT template names should be named using only lowercase letters separated by minus.`
`       Standard XSLT functions/structures are always lowercase seperated by minus.`
`       Our custom template names and function names match that style.`
`     `

`       XSLT `<b>`local`</b>` variable and parameters names should be named using the "studly caps" style`
`       (also referred to as "bumpy case" or "camel caps"). The initial letter of the name`
`       (after the prefix) is lowercase, and each letter that starts a new "word" is capitalized.`
`     `

`       `<b>`Global`</b>` XSLT variable names should be named using uppercase letters seperated by underscores.`
`     `

    <xsl:template name="<span title="Template name" class="quickinfo">format-date</span>">
      <xsl:param name="<span title="Local parameter" class="quickinfo">date</span>" />
      <xsl:param name="format" select="<span title="Global variable" class="quickinfo">$DATETIME_DEFAULT_FORMAT</span>" />
      <xsl:call-template name="format-date-time">

        <xsl:with-param name="dateTime" select="$date" />
        <xsl:with-param name="format" select="$format" />
        <xsl:with-param name="outputTime" select="<span title="Standard XPath function" class="quickinfo">false()</span>" />
      </xsl:call-template>
    </xsl:template>

</div>
