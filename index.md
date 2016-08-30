---
layout: page
title: iVXjs
id: home
---

# iVXjs 

A library to make data-driven interactive web-experiences with support for 
video, sound, data collection, decision-trees and animations.

## Note about this version

iVXjs is built to be used with different rendering libraries to create the 
experience. Currently, this repository utilizes Angular to build and render the 
experience and this "Getting Started" will focus on that implementation. 

## Getting Started

### Downloading the library

* Clone this repository or download zip
* Via npm `npm install ivx-js-angular --save`

### iVXjs Angular's Dependencies

The Angular iVXjs instance--iVXjs for this documentation--has the following script dependencies:

* [Angular 1.5](https://angularjs.org/)
* [Angular UI-Router](https://github.com/angular-ui/ui-router)
* [ngSanitize](https://docs.angularjs.org/api/ngSanitize)

### Setting up the html

To get the iVXjs experience working, you can add the following HTML to your project:

```
	<!-- Angular application for this page -->
	<div ng-app="[MODULE-NAME]"></div>
	<!-- Container for an iVXjs Experience -->
	<div id='ivx'></div>
	<!-- iVXjs Angular Library Dependencies  -->
	<script src='[PATH-TO-JS]/angular.min.js'></script>
	<script src='[PATH-TO-JS]/angular-ui-router.min.js'></script>
	<script src='[PATH-TO-JS]/angular-sanitize.min.js'></script>
	<script src='[PATH-TO-JS]/angular.ivx.js'></script>
    <script>
        angular
        .module('[MODULE-NAME]', ['ivx-js'])
        .run(['iVXjs', function(iVXjs){
            iVXjs.init({
                "config" : {
						"defaultState": [{
							"stateId": "hello-world"
						}],
						"states": [{
							"id": "hello-world",
							"name": "Hello World",
							"url": "/hello-world",
							"type": "input",
							"header": {
								"html" : "<h1>Welcome to iVXjs!</h1><h2>Write in your name below so we can get to know you.</h2>"
							},
							"footer": {
								"html" : ""
							},
							"onEnter":[],
							"onExit": [],
							"next": [{
								"stateId" : "welcome-screen"
							}],
							"onSubmit": [],
							"inputs": [{
								"id": "name",
								"type": "text",
								"name" : "name",
								"label" : "Your Name:"           
							}]
						},{
							"id" : "welcome-screen",
							"name" : "Welcome Screent",
							"url" : "/welcome-screen",
							"type" : "html",
							"html" : "<div class=\"welcome\"><h1>Congratulations {{experience.name}}!</h1><h2>You made your first iVXjs Experience!</h2></div>"

						}]
					}	
            	});
        	}])     
    </script>
```

### Breaking down the html

Let's take a moment to explain what the above HTML is doing:

__Angular Module__

We have to add an angular module to a child element of the page so that 
iVXjs can bootstrap itself to the correct element.

So, to make sure it loads properly, add your `ng-app` directive to a child node. For this example,
replace the `[MODULE-NAME]` with the name of your app:

```
<div ng-app="app"></div>
```

__Adding the dependency scripts__

These add the scripts to the page. Just replace `[PATH-TO-JS]` with the
location of these scripts relative to your page.

```
<script src='[PATH-TO-JS]/angular.min.js'></script>	
<script src='[PATH-TO-JS]/angular-ui-router.min.js'></script>
<script src='[PATH-TO-JS]/angular-sanitize.min.js'></script>
<script src='[PATH-TO-JS]/angular.ivx.js'></script>
```

__Angular Code__

The actual way we begin iVXjs is by running the init function during
the child app's run function.

First we will need to inject 'ivx-js' into your angular app:

```
angular
        .module('[MODULE-NAME]', ['ivx-js'])
```

Next we need to tell the angular module on run to initialize by running the `iVXjs.init`:

```
angular
    .module('[MODULE-NAME]', ['ivx-js'])
    .run(['iVXjs', 
        function(iVXjs){
        iVXjs.init({
            config : {
			    "defaultState": [{
			        "stateId": "hello-world"
			    }],
			    "states": [{
			        "id": "hello-world",
			        "name": "Hello World",
			        "url": "/hello-world",
			        "type": "input",
			        "header": {
			            "html" : "<h1>Welcome to iVXjs!</h1><h2>Write in your name below so we can get to know you.</h2>"
			        },
			        "footer": {
			            "html" : ""
			        },
			        "onEnter":[],
			        "onExit": [],
			        "next": [{
			            "stateId" : "welcome-screen"
			        }],
			        "onSubmit": [],
			        "inputs": [{
			            "id": "name",
			            "type": "text",
			            "name" : "name",
			            "label" : "Your Name:"           
			        }]
			    },{
			        "id" : "welcome-screen",
			        "name" : "Welcome Screen",
			        "url" : "/welcome-screen",
			        "type" : "html",
			        "html" : "<div class=\"welcome\"><h1>Congratulations {{experience.name}}!</h1><h2>You made your first iVXjs Experience!</h2></div>"

			    }]
			}
        });
    }])     
```

__Configuration__

The iVXjs init takes one argument, an object with various settings you can add to customize the experience, but for 
now we will using the following config. The definition for this spec is [here](/docs/esdocs/manual/configuration.html#json). But,
for this _Getting Started_ we will explain a little what this config is instructing iVXjs to do.

_Default State_

The default state array tells where is the starting point for this experience. In this case, we want this experience to start 
at the state "hello-world":

```
"defaultState": [{
        "stateId": "hello-world"
}]
```

_States_

The states array is the list of all states in this experience. States are segments of an experience where
a user can interact with in varied of ways. 

The first state in the array is an input state. The input state is a state that has input elements that typically captures
user information and adds it to the experience.data object. In this case, the state's data is indicating that it wants 
to make a text input that records the user's name. So, the spec here:

```

{
    "id": "hello-world",
    "name": "Hello World",
    "url": "/hello-world",
    "type": "input",
    "header": {
        "html" : "<h1>Welcome to iVXjs!</h1><h2>Write in your name below so we can get to know you.</h2>"
    },
    "footer": {
        "html" : ""
    },
    "onEnter":[],
    "onExit": [],
    "next": [{
        "stateId" : "welcome-screen"
    }],
    "onSubmit": [],
    "inputs": [{
        "id": "name",
        "type": "text",
        "name" : "name",
        "label" : "Your Name:"           
    }]
}
```

Will appear like this here:

![Hellow World State](http://e8ddcf8725663d605209-8d8cc7c733bcfce1ecd11bbb8349e503.r95.cf2.rackcdn.com/tutorial/Hello-World-State.png) 

The next and final state is a HTML state. The HTML state is a state where a user can put in any HTML they want and 
have it render. The HTML state for this set up shows a congratulatory message. So, the spec is as follows:

```
{
	"id" : "welcome-screen",
	"name" : "Welcome Screen",
	"url" : "/welcome-screen",
	"type" : "html",
	"html" : "<div class='welcome'><h1>Congratulations {{experience.name}}!</h1><h2>You made your first iVXjs Experience!</h2></div>"
}
			   
```

Will render:

![Congratulations State](http://e8ddcf8725663d605209-8d8cc7c733bcfce1ecd11bbb8349e503.r95.cf2.rackcdn.com/tutorial/Congratulations%20State.png)

For this state, a special note. Look at this line of the config:

```
"html" : "<div class='welcome'><h1>Congratulations {{experience.name}}!</h1><h2>You made your first iVXjs Experience!</h2></div>"
```

The `{{experience.name}}` indicates to iVXjs to replace the piece of code with the name the user 
provided in the last state. In this case, the "User's Name" was provided in the state before and 
now shows here.