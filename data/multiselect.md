 Multiselect 
==============
A set of check boxes.

<img src="multiselect_editor.png"/>

{{note
Include the **ext/dhtmlxscheduler_multiselect.js** file to use the control in the lightbox
}}

~~~js
scheduler.locale.labels.section_userselect = "Participants";
 
scheduler.config.lightbox.sections=[	
	{ name:"description", height:50, map_to:"text", type:"textarea", focus:true },
	{ name:"userselect", height:22, map_to:"user_id", type:"multiselect", 
    options: scheduler.serverList("user_id"), vertical:"false" },
	{ name:"time", height:72, type:"time", map_to:"auto"}	
];
~~~

{{sample
	03_extensions/21_multiselect_options.html
}}


Initialization
-----------------------
To add the Multiselect control to the lightbox, follow these steps:

<ol>
	<li><b>Include the 'ext/dhtmlxscheduler_multiselect.js' extension file on the page:</b>
~~~js
<script src="../codebase/ext/dhtmlxscheduler_multiselect.js"></script>

~~~
	</li>
    <li><b>Add the section to the lightbox configuration:</b>
~~~js
scheduler.config.lightbox.sections = 
	{ name:"description", ... },
	{ name:"userselect", height:22, map_to:"user_id", type:"multiselect", 
    options: scheduler.serverList("user_id"), vertical:"false" },
    { name:"time", ...}
];
~~~
	</li>
    <li><b>Set the label for the section:</b>
~~~js
scheduler.locale.labels.section_userselect = "Participants";
~~~
	</li>
</ol>
     
{{sample
	03_extensions/21_multiselect_options.html
}}

Properties
---------------------------------------------
The following properties are mostly important and commonly set for the 'multiselect' control (see the full list <a href="api/scheduler_lightbox_config.md">here</a>):

<table class="webixdoc_links">
	<tbody>
    	<tr>
			<td class="webixdoc_links0"><b>name</b></td>
			<td>(<i>string</i>) section's name </td>
		</tr>
        <tr>
			<td class="webixdoc_links0"><b>height</b></td>
			<td>(<i>number</i>) section's height</td>
		</tr>
        <tr>
			<td class="webixdoc_links0"><b>map_to</b></td>
			<td>(<i>string</i>) the name of a data property that will be mapped to the section</td>
		</tr>
        <tr>
			<td class="webixdoc_links0"><b>type</b></td>
			<td>(<i>textarea,time,select,template,multiselect,radio,checkbox</i>) the type of the section's control</td>
		</tr>
        <tr>
			<td class="webixdoc_links0"  style="vertical-align: top;"><b>options</b></td>
			<td>(<i>array of objects</i>) defines select options of the control (<b>for 'select', 'multiselect, 'radio' controls</b>).<br> Each object in the array specifies a single option and takes these properties:
            	<ul>
					<li><b>key</b> -   (<i>string</i>) the option's id. This attribute is compared with the event's data property to assign options to events</li>
					<li><b>label</b> -   (<i>string</i>) the option's label</li>
			</ul>
             </td>
		</tr>
        <tr>
			<td class="webixdoc_links0"><b>script_url</b></td>
			<td>(<i>string</i>) the path to the server-side script that will provide loading multiselect's options. Used in the dynamic mode only. Optional</td>
		</tr>
		<tr>
			<td class="webixdoc_links0"><b>vertical</b></td>
			<td>(<i>boolean</i>) specifies, whether multiselect buttons should be placed vertically(<i>true</i>) or horizontally  (<b>for the 'select' control only</b>)</td>
		</tr>
        <tr>
			<td class="webixdoc_links0"><b>delimiter</b></td>
			<td>(<i>string</i>) specifies the delimeter that will be used to separate values of the multiselect. If this property isn't set, the api/scheduler_section_delimiter_config.md
            config will be applied</td>
		</tr>
    </tbody>
</table>

Populating the control with data
-------------------------------------------

Generally, to set values for the multiselect buttons you should use the [options](api/scheduler_lightbox_config.md) parameter:

~~~js
scheduler.config.lightbox.sections = 
	{   name:"userselect", type:"multiselect", 
        ...
    	options:[
			{ key: 1, label: 'George' },
			{ key: 2, label: 'Nataly' },
			{ key: 3, label: 'Diana' },
            { key: 4, label: 'Adam' }
	]},
    ...
];
~~~

Items in the  [options](api/scheduler_lightbox_config.md) parameter must have 2 mandatory properties:

- **key** - the option's id
- **label** - the option's label

Populating check boxes from the server
------------------------------------------------------
To retrieve checkbox values from the server, you need to use the api/scheduler_serverlist.md method:

~~~js
scheduler.config.lightbox.sections = [
	{name:"description", ...},
	{ name:"userselect", height:22, map_to:"user_id", type:"multiselect", 
    options: scheduler.serverList("user_id"), vertical:"false" },
	{name:"time", ...}
];

scheduler.load("./data/data.php");
~~~

where the **data.php** is a server-side script, retrieving events loaded to the scheduler, and a collection of multiselect buttons values:

~~~php
//data.php
<?php

	include ('../../../../connector-php/codebase/scheduler_connector.php');
	include ('../../../../connector-php/codebase/crosslink_connector.php');
	require_once ('../../common/config.php');

	$res=mysql_connect($server, $user, $pass);
	mysql_select_db($db_name);

	$cross = new CrossOptionsConnector($res, $dbtype);
	$cross->options->render_table(
          "user",
          "user_id",
          "user_id(value),username(label)"
    );
	$cross->link->render_table("event_user","event_id", "user_id,event_id");
	
	$scheduler = new SchedulerConnector($res, $dbtype);
	
	$scheduler->set_options("user_id", $cross->options);
	$scheduler->render_table(
          "events_ms",
          "event_id",
          "start_date,end_date,event_name,details"
    );
?>
~~~

{{note
Note, you can use the api/scheduler_updatecollection.md method to update the list of retrieving options
}}


- CrossOptionsConnector connector takes two tables: one, where options are stored and the second where relations are stored. 
- The set_options() method is used to link the cross-link connector to the scheduler

Dynamic loading
----------------------------------------------
 
In the static mode all the event parameter options are stored as an individual field in the database, and you can use this field for building your own logic later. 
It gives additional possibilities, but forces to make more queries for loading all the options.
  
  
In the dynamic mode nothing additional is stored. Options are loaded as necessary. It decreases the amount of queries, but disables building of any logic.  

On server side you need to have code similar to the next

To enable the dynamic mode, you should use the **script_url** property in addition to **options**:

~~~js
scheduler.config.lightbox.sections = [
	{name:"userselect", height:22, map_to:"user_id", type:"multiselect", 
	options: scheduler.serverList("user_id"),
    script_url:'data/events_multiselect_options.php'},
    ...
];
~~~

where the server-side is specified as in:
~~~php
<?php
	include ('../../../../connector-php/codebase/scheduler_connector.php');
	include ('../../../../connector-php/codebase/crosslink_connector.php');
	require_once ('../../common/config.php');

	$res=mysql_connect($server, $user, $pass);
	mysql_select_db($db_name);
    
	$cross = new CrossOptionsConnector($res, $dbtype);
	$cross->dynamic_loading(true);
	$cross->options->render_table(
         "user",
         "user_id",
         "user_id(value),username(label)"
    );
	$cross->link->render_table("event_user","event_id", "user_id,event_id");

	$scheduler = new SchedulerConnector($res, $dbtype);
		
	$scheduler->set_options("user_id", $cross->options);

	$scheduler->render_table(
         "events_ms",
         "event_id",
         "start_date,end_date,event_name,details"
    );
?>
~~~







