---
title: Base simpletemplate
permalink: /Base_simpletemplate/
---

The **base_simpletemplate** class is a simple template class, mainly provided for use with the *sys_mail.php* object. The class implements simple variable substitution and block conditionals.

Public Methods
--------------

=== <string> = \$this-\>parse(<string>, <array>) === The parse method is the only public method avalable in the base_simpletemplate class. It takes a template, as a string, and a list of values, as a hash array, and then substitutes the variables into the template, which is returned as a string.

Example Usage
-------------

The following example :

    <?php
     echo $myTemplate->parse(
       'Good morning {%user%}. I am ready for my first lesson today.',
       array(
         'user' => 'Dr. Chandra'
       )
     );
    ?>

Will produce the following output

    Good morning Dr. Chandra. I am ready for my first lesson today.

Template Syntax
---------------

The simple template class recognizes everything within '{%...%}' tags as being template variables which will be substituted. The default tags can be changed by addressing the relevant variables in the class as follows:

     $myTemplate->tagOpen = '[!';
     $myTemplate->tagClose = '!]';

which would change the class to accept everything within '[!...!]' tags as variables.

### Conditional Blocks

As well as simple replacement of variables, the template class is also capable of conditional selection, using the reserved words IF and ENDIF.

    {% IF language == de %}
      Schwein gehabt!
    {% ENDIF %}
    {% IF language == en %}
      Lucky you!
    {% ENDIF %}

Depending on the value of 'language', parsing the template will output "Schwein gehabt!", "Lucky you!" or an empty string.

Caveats
-------

The simple template class is not for use on main output by Papaya. XML - XSL template translation should be used in preference.

[Category:papaya Core System](/Category:papaya_Core_System "wikilink")