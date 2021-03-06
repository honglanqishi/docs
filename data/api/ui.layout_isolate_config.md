isolate
=======

@short: masks IDs of all inner elements 
	

@type: boolean
@example:
webix.ui({
   cols:[
     { id:"col1", isolate:true, rows:[
     	{ view:"list", id:"mylist" },
        { template:"Details", id:"details" }
     ]},
     { id:"col2", isolate:true, rows:[
     	{view:"list", id:"mylist" },
        { template:"Details", id:"details" }
     ]}
   ]
});



@template:	api_config
@descr:

This property allows reusing the same IDs.
For example, in the above code snippet there are two blocks which are using the same IDs, so there is no way to access the list by its ID directly.

Due to the *isolate* property two elements with the same ID can exist and can be addressed in the following way:

~~~js
//will not work
var list = $$("mylist"); // will return second list

//will work
var list1 = $$("col1").$$("mylist");
var list2 = $$("col2").$$("mylist");
~~~

@related:
desktop/building_app_architecture.md#isolatingidsinsidelayouts