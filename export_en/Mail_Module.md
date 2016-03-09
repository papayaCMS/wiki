---
title: Mail Module
permalink: /Mail_Module/
---

Location
--------

The mail module is a system module, and is found in the system/sys_email.php file. It can be included with the following line :

include_once(PAPAYA_INCLUDE_PATH.'system/sys_email.php');

Creator Method
--------------

An email object is created in the standard way, with the

\$myObject = new email();

Public Methods
--------------

### setSender(<Email address string>, <name string>)

Set sender allows the setting of who is sending the message, both their name and email address.

### addAddress(<Email address string>, <name string>, \<'TO'|'CC'|'BCC'\>)

add address allows the addition of a user to a send envelope. The destination (to, carbon-copy and blind carbon copy) can be specified.

### setSubject()

### setBody(<message string> [, <template array>])

Set Body is used to set the main message data of the email. This method also uses simpleTemplate class to allow the replacement of tokens in the message string - tokens being of the type {%<TOKEN STRING>%}.

As an example, the following

    <?php
    $my->setBody("Good Morning {%user%}. I am ready for my first lesson.",
      array('user' => 'Dr. Chandra')
    );
    ?>

will result in the output

Good Morning Dr. Chandra. I am ready for my first lesson.

This feature allows fairly complex templates to be built up. Simple IF conditions are also allowed, due to the [base_simpletemplate](/base_simpletemplate.md) class.

[Category:papaya Core System](export_en/Category:Papaya_Core_System.md)
