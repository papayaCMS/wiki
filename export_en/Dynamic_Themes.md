
Dynamic Themes
==============

Implemented: papaya CMS \>= 5.5

The dynamic themes are a feature introduced in papaya CMS 5.5. They allow to change css values using the administration interface.

Value Definition

The value definition is done inside the themes.xml inside a theme directory. Here is a new section "dynamic-values" with page, group and value definitions.

Example:
```
 <papaya-theme>
   <name>Default Papaya Theme</name>
   ...
   <dynamic-values>
     <page name="boxes" title="Boxes">
       <group name="borders" title="Borders">
         <value name="color" title="Color" type="color" default="#FFFFFF"/> 
         <value name="size" title="Size" type="select_checkboxes">
           <type-parameter>1px</type-parameter>
           <type-parameter>2px</type-parameter>
         </value> 
       </group>
     </page>
   </dynamic-values>
 </papaya-theme>
```

Pages can contain several groups with multiple values. Each page will be one edit dialog in the administration interface. The "name"-Attributes need to contain valid xml element names.

The "type" Attribute is a profile used by PapayaUiDialogFieldFactory. Parameter can be provided as an attribute (if it is only one) or child elements if an array is needed. The "default"-Attribute are optional but highly suggested.

Template Preparation

To replace the dynamic values in the delivered css, the css wrapper has to be used. The urls of your css files should look like "<http://www.yourdomain.tld/papaya-themes/themename/css.php?files=foo.css&set=3>" if here is an set active.

Theme Preparation

After defining the values in the themes.xml, the css files need to be prepared. They will be still valid css. No special tools, scripts or applications are used to edit your css files.

The placeholder is an comment like "/\*\$pagename.groupname.valuename\*/". The \$ marks the following css value as replaceable. If now value for a placeholder is found, the placeholder is removed.

Example:

 div.box {
   border-color: /*$boxes.borders.color*/ #FFFFFF;
   border-size: /*$boxes.borders.size*/ 1px;
 }
