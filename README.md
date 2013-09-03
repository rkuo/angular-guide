# Learning AngularJS Concept from the Guide

This is the study notes for angularJS. This notes follow the [angularjs guide](http://docs.angularjs.org/guide/concepts) please refer origin for more details.

## Tools

- make sure there is angularjs plugin for editor.
- open jsFiddle or similar online playground for testing.

We will periodically create branches for jumping back for experiment.
 
## step-0-init setup

  - start a basic HTML Hello World template, title and paragraph. No angularjs yet.

## step-1-startup

  - add ng-app to html tag to set boundary of angularjs
  - add reference to angularjs script library in head tag
  - add ng-init in paragraph to initialize some expressions, in here, we set name variable with value 'World' [see doc for more details](http://docs.angularjs.org/api/ng.directive:ngInit)
  - copy all code into HTML panel of jsFiddle, and run it, works, git add, commit and give a branch name.

## step-2-runtime

The short code here is trying to show how model and view keep in sync:

- use the value from input tag as ng-model, the value is called "name"
- part of the p-tag value "name" is linked to the ng-model. It observes/syncs the model. 

[copy from Angular doc](http://docs.angularjs.org/guide/concepts), reformated here for ease reading. 

=- copy start here -==

The diagram tries to describe how Angular interacts with the browser's event loop. 

1. The browser's event-loop waits for an event to arrive. An event is a user interaction, timer event, or network event (response from a server). 

2. The event's callback gets executed. This enters the JavaScript context. The callback can modify the DOM structure. 

3. Once the callback executes, the browser leaves the JavaScript context and re-renders the view based on DOM changes. Angular modifies the normal JavaScript flow by providing its own event processing loop. This splits the JavaScript into classical and Angular execution context. 
  
Only operations which are applied in Angular execution context will benefit from Angular data-binding, exception handling, property watching, etc... You can also use $apply() to enter Angular execution context from JavaScript. Keep in mind that in most places (controllers, services) $apply has already been called for you by the directive which is handling the event. An explicit call to $apply is needed only when implementing custom event callbacks, or when working with third-party library callbacks. 

 1. Enter Angular execution context by calling scope.$apply(stimulusFn). Where stimulusFn is the work you wish to do in Angular execution context. 
  	
 2. Angular executes the stimulusFn(), which typically modifies application state. 
  	
 3. Angular enters the $digest loop. The loop is made up of two smaller loops which process $evalAsync queue and the $watch list. The $digest loop keeps iterating until the model stabilizes, which means that the $evalAsync queue is empty and the $watch list does not detect any changes. 
  	
 4. The $evalAsync queue is used to schedule work which needs to occur outside of current stack frame, but before the browser's view render. This is usually done with setTimeout(0), but the setTimeout(0) approach suffers from slowness and may cause view flickering since the browser renders the view after each event. 
  	
 5. The $watch list is a set of expressions which may have changed since last iteration. If a change is detected then the $watch function is called which typically updates the DOM with the new value. 
  	
 6. Once the Angular $digest loop finishes the execution leaves the Angular and JavaScript context. 
  	
This is followed by the browser re-rendering the DOM to reflect any changes. Here is the explanation of how the Hello world example achieves the data-binding effect when the user enters text into the text field. 
  	
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
  	
=- copy to here -==  	
  	
If the testing in jsFiddle does not run, make sure Angularjs in Framework & Extensions, and No wrap - in <head> are selected.

## step-3-scope

Because the different scope, same variables called "name" may have different values.

- we added a js file called script.js, which defines two scopes.
- we have ng-controller="GreetCtrl" in one <div>, and ng-controller="ListCtrl" in the other `<div>`, they are different scopes, which refer to diiferent values.
- To make display/view more specific for different scopes, we use red color border for different scopes. This is defined in CSS file.

Controller is in js

	function GreetCtrl($scope) {
		$scope.name = 'World';
		}	
	function ListCtrl($scope) {
		$scope.names = ['Igor', 'Misko', 'Vojta'];
	}

There are 4 names in 2 different scopes.

## step-4-controller

A controller construct model and publish it to view along with callback methods. The view is a projection of the scope onto the template. In the [later part of guide](http://docs.angularjs.org/guide/scope), it defines scope. "scope is an object that refers to the application model. It is an execution context for expressions. Scopes are arranged in hierarchical structure which mimic the DOM structure of the application. Scopes can watch expressions and propagate events." scope is defined in js.

HTML sets up the template of webpage, Controller provides the value and behavior of the elements. Scope provides the context and connects two. There are two different scopes: MyCtrl and action function. 

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

There is no code change in document. Model is data, which can merge with HTML to create view. 



