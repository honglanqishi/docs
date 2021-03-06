Adding Active Elements to Components
==========================

According to default Webix pattern, applications are made by putting pre-built meaningful components (like [grid](datatable/index.md), [tree](datatree/index.md), [controls](desktop/controls.md)) 
into [layouts](desktop/building_app_architecture.md). The areas with extra interactivity can be added to Webix components in two ways: 

##Defining Active Zones through Component templates

Clickable (or in some other way 'active') areas inside components are defined through HTML templates and CSS classes. More on [data templates](desktop/html_templates.md).

<img src="desktop/list_item_css.png"/>

{{sample 05_list/14_custom_handlers.html}}


In the [list](desktop/list.md) above the task is solved in the following way:

- each item has a template with HTML content in addition to list item data. 
- the **onClick** event of a list component is redefined for *info* CSS class. Clicking on this area (*'Details' button*) invokes showing a message while clicking any list item outside the button invokes default 
list behavior for clicking (here item is selected); 

{{snippet
CSS for active element
}}
~~~css
.info{
	width:100px;
	text-align: center;
	font-weight:bold;
	float:right;
	background-color:#444;
	color:white;
	border-radius:3px;
}
~~~
{{snippet
List config with active span element
}}
~~~js
webix.ui({
	view:"list", 
	template:"#votes# <span class='info'>Details</span>",
    select:true,
	onClick:{
		info:function(e, id){
			webix.message(this.getItem(id).title);
			return false;
		}
	}
});
~~~

##Using ActiveContent Module

[ActiveContent](api/refs/activecontent.md) module lets developers get rid of drawing interactive elements themselves and makes it possible to use existing Webix controls for it. 
In other words, it allows inserting controls into views. (Normally, it goes only for [layouts](desktop/building_app_architecture.md)). 

<img src="desktop/list_active_content.png"/>

{{sample 05_list/15_active_content.html}}

First of all, the **ActiveContent** module should be added to the needed component by extending its default functionality:

~~~js
webix.protoUI({
    name:"activeList" //give it some name you like
},webix.ui.list, webix.ActiveContent);
~~~

Now, you can use it. Note that the **name** of the newly created component is used as **view** property value: 

~~~js
webix.ui({
    view: "activeList",
    id:"mylist",
    activeContent:{
        deleteButton:{
            id:"deleteButtonId",
            view:"button",
            label:"Delete",
            width:120
        },
        editButton:{
			id:"editButtonId",
			view:"button",
			label:"Edit",
			width:80		
		},
        markCheckbox:{
        	view:"checkbox", 
            ...
        }
    },
  template: "<div class='rank'>#rank#.</div>"+
  			"<div class='title'>#title#<br>#year# year</div>"+
  			"<div class='buttons'>{common.deleteButton()}</div>"+
  			"<div class='buttons'>{common.editButton()}</div>"+
			"<div class='checkbox'>{common.markCheckbox()}</div>"
});
~~~

"Active" template can be also defined via a function:

~~~js
view:"activelist",
template: function (obj, common) {
   return obj.title+"<div>"+common.deleteButton(obj, common)+"</div>";
}
~~~

- the **activeContent** property contains an array of key-value pairs where: 
	- *key* is a used-defined **name** of an active area (here *deleteButton*, *editButton*);
    - *value* is its **configuration**. 
- config of the active element is stored in the **common** property of the housing component and can be derived by its *name*;
- template for each list item includes these configurations as {common.*name*};

If an active content element can have a **variable value** (like [text](desktop/text.md) field, or [checkbox](desktop/checkbox.md)) it can be passed to it within data item properties: 

{{snippet
Value for 'markCheckbox' active element
}}
~~~js
view:"activeList",
data:[
	{ id:5, title:"My Fair Lady", year:1964, rank:5, markCheckbox:1}
]
~~~

**Click** and other **events** are attached to the buttons according to Webix [Event handling](desktop/event_handling.md) pattern:

~~~js
view:"button", click:function(){...}
//or
button.attachEvent("onItemClick", function(){...});
~~~

###Getting master of an active content element

- using [locate](api/link/ui.proto_locate.md) function:
	- in any mouse event handler  of the active element (e.g. *onItemClick* event, or *click* property).

~~~js
$$('deleteButtonId').attachEvent("onItemClick", function(id, e){
	var item_id = $$('mylist').locate(e);
    //returns id of a list item
});
~~~

- through the active element configuration:
	- if  a handler is attached to non-mouse event (all component-specific Webix events);
    - if an active element is redrawn on a mouse event (like toggle, checkbox).


~~~js
on:{
	'onChange':function(newv, oldv){
		var item_id = this.config.$masterId;
	}
}
~~~

**Datatable and treetable specificity**

With [datatable](datatable/index.md) and [treetable](desktop/treetable.md) you can get a row object by **$masterId** property that includes: 

- **row** - row id;
- **rind** - row index;
- **column** - column id;
- **cind** - column index.

~~~js
on:{
	'onChange':function(newv, oldv){
		var item_id = this.config.$masterId.row;
	}
}
~~~

##Related articles

- [Adding a custom element to the datatable header](datatable/headers_footers.md#customheaderandfootercontent)

@complexity: 2