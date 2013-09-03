# Learning AngularJS Concept from the Guide

This is a study notes for angularJS. This notes follow the [angularjs guide](http://docs.angularjs.org/guide/concepts) please refer origin for more details.

## Tools

- make sure there is angularjs plugin for editor.

You can edit the code and try it out in plunker or jsFiddle. Even though, concepts are un-related, I will periodically create branches for jumping back to certain spot for experiment more.
 
## step-0-init setup

  - start a basic HTML Hello World template, title and paragraph. No angularjs yet.

## step-1-startup

  - add ng-app to html tag to indicate angularJS context and set boundary of angularjs.
  - add reference to angularjs script library in `<head>`.
  - add ng-init in paragraph to initialize some expressions, in here, we set name equal 'World' [see doc for more details](http://docs.angularjs.org/api/ng.directive:ngInit)
  
=>>insert startup picture here
    
  - copy all code into HTML panel of jsFiddle, and run it, works, git add, commit and give a branch name. This can be done many different ways, we will set it properly later.

## step-2-runtime

The short code here is trying to show how model and view keep in sync:

- use the value from input tag as ng-model, the value is called "name"
- part of the p-tag value "name" is linked to the ng-model. It observes/syncs with the model. 

[copy from Angular doc](http://docs.angularjs.org/guide/concepts), reformated here for ease reading. 

=>>insert runtime picture here 

=- copy start here -==
    	
Here is the explanation of how the Hello world example achieves the data-binding effect when the user enters text into the text field. 
  	
 1. During the *compilation phase*: 
  	1. the ng-model and input directive set up a keydown listener on the `<input>` control. 
  	2. the {{name}} interpolation sets up a $watch to be notified of name changes. 
  	
 2. During the *runtime phase*: 
  	1. Pressing an 'X' key causes the browser to emit a keydown event on the input control. 
  	2. The input directive captures the change to the input's value and calls $apply("name = 'X';") to update the application model inside the Angular execution context. 
  	3. Angular applies the name = 'X'; to the model. 
  	4. The $digest loop begins 
  	5. The $watch list detects a change on the name property and notifies the {{name}} interpolation, which in turn updates the DOM. 
  	6. Angular exits the execution context, which in turn exits the keydown event and with it the JavaScript execution context. 
  	7. The browser re-renders the view with update text.
  	
=- copy up to here -== 	
  	
If the testing in jsFiddle does not run, make sure "Angularjs", and "No wrap - in <head>" in Framework & Extensions are selected.

## step-3-scope

Because the different scope, same variables called "name" may have different values.

- we added a js file called script.js, which defines two scopes.
- we have ng-controller="GreetCtrl" in one `<div>`, and ng-controller="ListCtrl" in the other `<div>`, they are different scopes, which refer to diiferent values.
- To make display/view more specific for different scopes, it uses red color border for different scopes. This is defined in CSS file.

Controller describes in js.

	function GreetCtrl($scope) {
		$scope.name = 'World';
		}	
	function ListCtrl($scope) {
		$scope.names = ['Igor', 'Misko', 'Vojta'];
	}

There are 4 names in 2 different scopes.

## step-4-controller

A controller construct model and publish it to view along with callback methods. The view is a projection of the scope onto the template. In the [later part of guide](http://docs.angularjs.org/guide/scope), it defines scope: "scope is an object that refers to the application model. It is an execution context for expressions. Scopes are arranged in hierarchical structure which mimic the DOM structure of the application. Scopes can watch expressions and propagate events." Scope is defined in js.

=>>insert controller picture here

HTML sets up the template of webpage, Controller provides the value and behavior of the elements. Scope provides the context and connects View and Controller. There are two different scopes: MyCtrl and action. 

Controller
	
	function MyCtrl($scope) {
		$scope.action = function() {
    		$scope.name = 'OK';
    	}
    
    	$scope.name = 'World';
    }

View
    
	<div ng-controller="MyCtrl">
      Hello {{name}}!
      <button ng-click="action()">
        OK
      </button>
    </div>

Tested in Chrome, works.

## step-5-model

Scope is a data-model, which can merge with HTML to create view. With different scopes, view can connect to different part of data-model.

=>>insert model picture here

## step-6-view

Angular extends HTML by adding some angularjs-directives, like an attributes to an element. The browser parses the HTML into the DOM with all these new "attributes", which becomes the input for angularjs's compiler. The compiler looks for specified-directives to set up watches on the specified model. With event model described above, the view will continue to observe/sync with the model.

=>>insert half size view picture here


