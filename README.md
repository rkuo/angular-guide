# Learning AngularJS for the guide

## Tools
- make sure there is angularjs plugin for editor.
- open jsFiddle or similar online playground for testing.

periodically create a branch for user to jump back. 
## step-0-init setup
  - start a basic HTML Hello World template, title and paragraph. No angularjs yet.
  - refer http://docs.angularjs.org/guide/concepts for note

## step-1-startup
  - add ng-app to html tag to set boundary of angularjs
  - add reference to angularjs script library in head tag
  - add ng-init in paragraph to initialize some expressions, in here, we set name variable with value 'World' [see doc for more details](http://docs.angularjs.org/api/ng.directive:ngInit)
  - copy all code into HTML panel of jsFiddle, and run it, works, git add, commit and give a branch name.

## step-2-runtime
  - [copy from doc, reformated here for ease reading](http://docs.angularjs.org/guide/concepts) Short code here is trying to show how model and view keep in sync:
	- use the value from input tag as ng-model, the value is called "name"
	- part of the p-tag value "name" is linked to the ng-model. It observes/syncs the model. 

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
  		1. the ng-model and input directive set up a keydown listener on the <input> control. 
  		2. the {{name}} interpolation sets up a $watch to be notified of name changes. 
  	
  	2. During the *runtime phase*: 
  		1. Pressing an 'X' key causes the browser to emit a keydown event on the input control. 
  		2. The input directive captures the change to the input's value and calls $apply("name = 'X';") to update the application model inside the Angular execution context. 
  		3. Angular applies the name = 'X'; to the model. 
  		4. The $digest loop begins 
  		5. The $watch list detects a change on the name property and notifies the {{name}} interpolation, which in turn updates the DOM. 
  		6. Angular exits the execution context, which in turn exits the keydown event and with it the JavaScript execution context. 
  		7. The browser re-renders the view with update text.
  		