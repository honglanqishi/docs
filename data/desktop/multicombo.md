Multicombo
============

{{pronote
The control is available only in **Webix Pro** edition.
}}

##API Reference

- [Methods, properties and events](api/refs/ui.multicombo.md)
- [Shared functionality](desktop/controls_guide.md)

##Overview

Multicombo is an editable control that allows for choosing several options at a time from a dedicated popup list, 
[Checksuggest](api/refs/ui.checksuggest.md). 

<img src="desktop/multicombo_message.png">

Multicombo inherits from a standard [combo](desktop/combo.md) control and has the following features:

- Each option is equipped with a checkbox;
- Each time you enter symbols in the input field the list of options is filtered;
- To select an option, check it or click the corresponding list item. A selected option will be added to the input;
- To remove an option from selection, click either the "cross" sign or use the "backspace" key in the input filed or uncheck the option in the list. 

##Initialization

~~~js
var names = [
	{"id":1,"value":"Ray M. Parra"},
  	{"id":2,"value":"Sabrina N. Hermann"},
  	{"id":3,"value":"Lane E. Dion"}
];
    
webix.ui({
  	view:"multicombo", 
  	label:"To", 
  	value:"2,3", 
  	suggest: names // or e.g. suggest: "data/names.js"
});
~~~

{{sample 60_pro/02_form/06_multicombo.html}}

**Main properties**

- **label** (string) - specifies text label of a control;
- **labelPosition** (string) - sets the position of the label in relation to the control container;
- **value** (array|string) - defines the initially selected items of the control;
- **separator** (string) - defines a delimiter for separating options in a multicombo, comma by default;
- **options** or **suggest** (array|object|string) - defines a set of items to select from, or the data source, or [advanced popup configuration](desktop/advanced_combo.md);
- **optionWidth** (number) - defines the width of a popup list. By default, it is adjusted to the control width;
- **button** (boolean) - defines whether the "Select" button will be displayed in the options list;
- **keepText** (boolean) - defines whether the entered text should be kept in the input after an option is selected.


##Configuring Suggest List

You can read about advanced configuration options for popup selectors in the [corresponding documentation article](desktop/advanced_combo.md).

##Accessing Suggest List

Multiselect API allows getting popup and list objects for further usage: 

~~~js
var popup = multicombo.getPopup();
var list = popup.getBody();
//or
var list = popup.getList();
~~~

##Manipulating Tags

In some cases, multicombo can not be shown as a multi-line input and information about selected items should be displayed only as a number of these items.

<img src="desktop/multicombo_tags.png">

To disable multiple tags, you need to set the api/ui.multicombo_tagmode_config.md property to *false*:

~~~js
var names = [
  	{"id":1,"value":"Ray M. Parra"},
  	{"id":2,"value":"Sabrina N. Hermann"},
  	{"id":3,"value":"Lane E. Dion"}
];
    
webix.ui({
	view:"multicombo", 
    label:"Name", 
   	labelPosition: "top",
   	value:"2,3",
  	tagMode: false,
   	suggest:names
});
~~~

To specify a template for the common tag, you need to use the api/ui.multicombo_tagtemplate_config.md property.
By default the tagTemplate is set like this:

~~~js
tagTemplate: function(values){
   return (values.length? values.length+" item(s)":"");
}
~~~

- values - an array of the selected items' ids 

You can customize the template the way you like:

~~~js
var names = [
  	{"id":1,"value":"Ray M. Parra"},
  	{"id":2,"value":"Sabrina N. Hermann"},
  	{"id":3,"value":"Lane E. Dion"}
];

webix.ui({
   view:"multicombo", 
    label:"Name", 
    labelPosition: "top",
    value:"2,3",
    tagMode: false,
    suggest:names,
  	tagTemplate: function(values){
        return (values.length? values.length+" item(s) selected":"");
    }
});
~~~

{{sample
	60_pro/02_form/07_multicombo_tag.html
}}


@edition:pro