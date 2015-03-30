#AngularJS is a JavaScript Framework
* AngularJS is a JavaScript framework. It is a library written in JavaScript.
* AngularJS is distributed as a JavaScript file, and can be added to a web page with a script tag:
<script src="//ajax.googleapis.com/ajax/libs/angularjs/1.2.15/angular.min.js"></script>

It is a good idea to place scripts at the bottom of the `<body>` element.
This improves page loading, because HTML loading is not blocked by scripts loading.


##AngularJS Extends HTML

AngularJS extends HTML with ng-directives.
The `ng-app` directive defines an AngularJS application.
The `ng-model` directive binds element values (like the value of an input field) to the application.
The `ng-bind` directive binds application data to the HTML view.

````
<!DOCTYPE html>
<html>
<body>
<div ng-app="">
<p>Name: <input type="text" ng-model="name"></p>
<p ng-bind="name"></p>
</div>
<script src="//ajax.googleapis.com/ajax/libs/angularjs/1.2.15/angular.min.js"></script>
</body>
</html>
````

AngularJS starts automatically when the web page has loaded.

The `ng-app` directive tells AngularJS that the `<div>` element is the "owner" of an AngularJS application.
The` ng-model` directive binds the value of the input field to the application variable name.
The `ng-bind` directive binds application variable name to the innerHTML of a paragraph.
If you remove the ng-app directive, HTML will display the expression as it is, without solving it.


##AngularJS Directives

As you have already seen, AngularJS directives are HTML attributes with an `ng` prefix.
The ng-init directive initialize AngularJS application variables.
````
<div ng-app="" ng-init="firstName='John'">
<p>The name is <span ng-bind="firstName"></span></p>
</div>
```
HTML5 allows extended (home made) attributes, starting with `data-`.
AngularJS attributes start with `ng-`, but you can use `data-ng-`, to make your page HTML5 valid.
Alternatively with valid HTML5:
````
<div data-ng-app="" data-ng-init="firstName='John'">
<p>The name is <span data-ng-bind="firstName"></span></p>
</div>
````

##AngularJS Expressions

AngularJS expressions are written inside double braces: `{{ expression }}`.
AngularJS expressions binds data to HTML the same way as the ng-bind directive.
AngularJS will "output" data exactly where the expression is written.
AngularJS expressions are much like JavaScript expressions: They can contain literals, operators, and variables.
Example 
````
{{ 5 + 5 }} or {{ firstName + " " + lastName }}

<!DOCTYPE html>
<html>
<body>
<div ng-app="">
<p>My first expression: {{ 5 + 5 }}</p>
</div>
<script src="//ajax.googleapis.com/ajax/libs/angularjs/1.2.15/angular.min.js"></script>
</body>
</html>
````

##AngularJS Numbers
AngularJS numbers are like JavaScript numbers:
````
<div ng-app="" ng-init="quantity=1;cost=5">
<p>Total in dollar: {{ quantity * cost }}</p>
</div>
````
Same example using ng-bind:
````
<div ng-app="" ng-init="quantity=1;cost=5">
<p>Total in dollar: <span ng-bind="quantity * cost"></span></p>
</div>
````
Using `ng-init` is not very common. You will learn a better way to initialize data in the chapter about controllers.


##AngularJS Strings
AngularJS strings are like JavaScript strings:
````
<div ng-app="" ng-init="firstName='John';lastName='Doe'">
<p>The name is {{ firstName + " " + lastName }}</p>
</div>
````
Same example using ng-bind:
````
<div ng-app="" ng-init="firstName='John';lastName='Doe'">
<p>The name is <span ng-bind="firstName + ' ' + lastName"></span></p>
</div>
````

##AngularJS Objects
AngularJS objects are like JavaScript objects:
````
<div ng-app="" ng-init="person={firstName:'John',lastName:'Doe'}">
<p>The name is {{ person.lastName }}</p>
</div>
````
Same example using ng-bind:
````
<div ng-app="" ng-init="person={firstName:'John',lastName:'Doe'}">
<p>The name is <span ng-bind="person.lastName"></span></p>
</div>
````

##AngularJS Arrays
AngularJS arrays are like JavaScript arrays:
````
<div ng-app="" ng-init="points=[1,15,19,2,40]">
<p>The points are {{ points[2] }}</p>
</div>
````
Same example using ng-bind:
````
<div ng-app="" ng-init="points=[1,15,19,2,40]">
<p>The points are <span ng-bind="points[2]"></span></p>
</div>
````
AngularJS lets you extend HTML with new attributes called Directives.


##AngularJS Directives
AngularJS directives are extended HTML attributes with the prefix `ng-`.
The `ng-app` directive initializes an AngularJS application.
The `g-init` directive initialize application data.
The `ng-model` directive binds application data to HTML elements.
````
<div ng-app="" ng-init="firstName='John'">
<p>Name: <input type="text" ng-model="firstName"></p>
<p>You wrote: {{ firstName }}</p>
</div>
````
The `ng-app` directive also tells AngularJS that the `<div>` element is the "owner" of the AngularJS application.
 	A web page can contain many AngularJS applications, running in different elements.


##Data Binding
The `{{ firstName }}` expression, in the example above, is an AngularJS data binding expression.
Data binding in AngularJS, synchronizes AngularJS expressions with AngularJS data.
`{{ firstName }}` is synchronized with ng-model="firstName".
In the next example two text fields are synchronized with two ng-model directives:
````
<div ng-app="" ng-init="quantity=1;price=5">
Quantity: <input type="number" ng-model="quantity">
Costs:    <input type="number" ng-model="price">
Total in dollar: {{ quantity * price }}
</div>
````
Using ng-init is not very common. You will learn how to initialize data in the chapter about controllers.


###Repeating HTML Elements
The `ng-repeat` directive repeats an HTML element:
````
<div ng-app="" ng-init="names=['Jani','Hege','Kai']">
<ul>
<li ng-repeat="x in names">
{{ x }}
</li>
</ul>
<div>
````
The `ng-repeat` directive used on an array of objects:
````
<div ng-app="" ng-init="names=[
{name:'Jani',country:'Norway'},
{name:'Hege',country:'Sweden'},
{name:'Kai',country:'Denmark'}]">
<ul>
  <li ng-repeat="x in names">
    {{ x.name + ', ' + x.country }}
  </li>
</ul>
</div>
````
AngularJS is perfect for database CRUD (Create Read Update Delete) applications.
Just imagine if these objects where records from a database.


###The ng-app Directive
The `ng-app` directive defines the root element of an AngularJS application.
The `ng-app` directive will auto-bootstrap (automatically initialize) the application when a web page is loaded.
Later you will learn how ng-app can have a value (like ng-app="myModule"), to connect code modules.


###The ng-init Directive
The ng-init directive defines initial values for an AngularJS application.
Normally, you will not use `ng-init`. You will use a controller or module instead.
You will learn more about controllers and modules later.

###The ng-model Directive
The ng-model directive binds HTML elements to application data.
The ng-model directive can also:
* Provide type validation for application data (number, email, required).
* Provide status for application data (invalid, dirty, touched, error).
* Provide CSS classes for HTML elements.
* Bind HTML elements to HTML forms.

###The ng-repeat Directive
The ng-repeat directive clones HTML elements once for each item in a collection (in an array).
AngularJS controllers control the data of AngularJS applications.
AngularJS controllers are regular JavaScript Objects.

##AngularJS Controllers
AngularJS applications are controlled by controllers.
The ng-controller directive defines the application controller.
A controller is a JavaScript Object, created by a standard JavaScript object constructor.
The $scope of the controller is the application /the HTML element) it is referred from.
```
<div ng-app="" ng-controller="personController">
First Name: <input type="text" ng-model="person.firstName"><br>
Last Name: <input type="text" ng-model="person.lastName"><br>
<br>
Full Name: {{person.firstName + " " + person.lastName}}
</div>
<script>
function personController($scope) {
    $scope.person = {
        firstName: "John",
        lastName: "Doe"
    };
}
</script>
````
Application explained:
The AngularJS application is defined by ng-app. The application runs inside a `<div>`.
The `ng-controller` directive names the controller object.
The function personController is a standard JavaScript object constructor.
The controller object has one property: $scope.person.
The person object has two properties: firstName and lastName.
The ng-model directives binds the input fields to the controller properties (firstName and lastName).


##Controller Properties
The example above demonstrated a controller object with two properties, lastName and firstName.
A controller can also have functions as object properties:
````
<div ng-app="" ng-controller="personController">
First Name: <input type="text" ng-model="person.firstName"><br>
Last Name: <input type="text" ng-model="person.lastName"><br>
<br>
Full Name: {{person.fullName()}}
</div>
<script>
function personController($scope) {
    $scope.person = {
        firstName: "John",
        lastName: "Doe",
        fullName: function() {
            var x;
            x = $scope.person;
            return x.firstName + " " + x.lastName;
        }
    };
}
</script>
````

###Controller Methods
A controller can also have methods:
````
<div ng-app="" ng-controller="personController">
First Name: <input type="text" ng-model="person.firstName"><br>
Last Name: <input type="text" ng-model="person.lastName"><br>
<br>
Full Name: {{fullName()}}
</div>
<script>
function personController($scope) {
    $scope.person = {
        firstName: "John",
        lastName: "Doe",
     };
     $scope.fullName = function() {
         var x;
         x = $scope.person; 
         return x.firstName + " " + x.lastName;
     };
}
</script>
````

###Controllers In External Files
In larger applications, it is common to store controllers in external files.
Just copy the code between the <script> tags into an external file named personController.js:

````
<div ng-app="" ng-controller="personController">
First Name: <input type="text" ng-model="person.firstName"><br>
Last Name: <input type="text" ng-model="person.lastName"><br>
<br>
Full Name: {{person.firstName + " " + person.lastName}}
</div>
<script src="personController.js"></script>


Another Example
For the next example we will create a new controller file:
````
function namesController($scope) {
    $scope.names = [
        {name:'Jani',country:'Norway'},
        {name:'Hege',country:'Sweden'},
        {name:'Kai',country:'Denmark'}
    ];
}
````
And then use the controller file in an application:
````
<div ng-app="" ng-controller="namesController">
<ul>
<li ng-repeat="x in names">
{{ x.name + ', ' + x.country }}
</li>
</ul>
</div>
<script src="namesController.js"></script>
````
Filters can be added to expressions and directives using a pipe character.


##AngularJS Filters
AngularJS filters can be used to transform data:
Filter	Description
currency	Format a number to a currency format.
filter	Select a subset of items from an array.
lowercase	Format a string to lower case.
orderBy	Orders an array by an expression.
uppercase	Format a string to upper case.

###Adding Filters to Expressions
A filter can be added to an expression with a pipe character (`|`) and a filter.
(For the next two examples we will use the person controller from the previous chapter)
The uppercase filter format strings to upper case:
````
<div ng-app="" ng-controller="personController">
<p>The name is {{ person.lastName | uppercase }}</p>
</div>
The lowercase filter format strings to lower case:
<div ng-app="" ng-controller="personController"">
<p>The name is {{ person.lastName | lowercase }}</p>
</div>
````

###The currency Filter
The currency filter formats a number as currency:
````
<div ng-app="" ng-controller="costController">
<input type="number" ng-model="quantity">
<input type="number" ng-model="price">
<p>Total = {{ (quantity * price) | currency }}</p>
</div>
````

###Adding Filters to Directives
A filter can be added to a directive with a pipe character (|) and a filter.
The orderBy filter orders an array by an expression:
````
<div ng-app="" ng-controller="namesController">
<ul>
<li ng-repeat="x in names | orderBy:'country'">
{{ x.name + ', ' + x.country }}
</li>
</ul>
<div>
````

###Filtering Input
An input filter can be added to a directive with a pipe character (|) and filter followed by a colon and a model name.
The filter filter selects a subset of an array:
````
<div ng-app="" ng-controller="namesController">
<p><input type="text" ng-model="name"></p>
<ul>
<li ng-repeat="x in names | filter:name | orderBy:'country'">
{{ (x.name | uppercase) + ', ' + x.country }}
</li>
</ul>
</div>
````
##AngularJS has its own HTML attributes directives.

###The ng-disabled Directive
The ng-disabled directive binds application data directly to the HTML disabled attribute.
````
<div ng-app="">
<p>
<button ng-disabled="mySwitch">Click Me!</button>
</p>
<p>
<input type="checkbox" ng-model="mySwitch">Button
</p>
</div>
````
Application explained:
The ng-disabled directive binds the application data "mySwitch" to the HTML disabled attribute.
The ng-model directive binds "mySwitch" to the content (value) of the HTML input checkbox element.


###The ng-show Directive
The ng-show directive hides or shows an HTML element.
````
<div ng-app="">
<p ng-show="true">I am visible.</p>
<p ng-show="false">I am not visible.</p>
</div>
````
You can use an expression that evaluates to true or false (like ng-show="hour < 12"), to hide and show HTML elements.
In the next chapter, there is another example, using the click of a button to hide an HTML element.
AngularJS has its own HTML events directives.


###The ng-click Directive
The ng-click directive defines an AngularJS click event.
````
<div ng-app="" ng-controller="myController">
<button ng-click="count = count + 1">Click me!</button>
<p>{{ count }}</p>
</div>
````

###Hiding HTML Elements
The ng-show directive defines the visibility of an application.
The value ng-show="true" (boolean value) makes the element visible
The value ng-show="false" makes the element unvisible.
````
<div ng-app="" ng-controller="personController">
<button ng-click="toggle()">Toggle</button>
<p ng-show="myVar">
First Name: <input type="text" ng-model="person.firstName"><br>
Last Name: <input type="text" ng-model="person.lastName"><br>
<br>
Full Name: {{person.firstName + " " + person.lastName}}
</p>
</div>
<script>
function personController($scope) {
    $scope.person = {
        firstName: "John",
        lastName: "Doe"
    };
    $scope.myVar = true;
    $scope.toggle = function() {
        $scope.myVar = !$scope.myVar;
    };
}
</script>
````
Application explained:
The first part of the personController is the same as in the chapter about controllers.
The application has a new default property: $scope.myVar = true;
The ng-show directive uses the variable myVar (true or false).
A new function toggle() toggles myVar between true and false.
Modules define your applications.
All your controllers should belong to a module.
Modules keep the global namespace clean.


###AngularJS Module Example
In this example, "myApp.js" contains an application module definition, "myCtrl.js" contains a controller:
````
<!DOCTYPE html>
<html>
<body>
<div ng-app="myApp" ng-controller="myCtrl">
{{ firstName + " " + lastName }}
</div>
<script src="//ajax.googleapis.com/ajax/libs/angularjs/1.2.15/angular.min.js"></script>
<script src="myApp.js"></script>
<script src="myCtrl.js"></script>
</body>
</html>
````

###Controllers Pollute the Global Namespace
All examples, so far in this tutorial, have used global functions.
Global variables and global functions should be avoided in all applications.
Global values (variables or functions) can be overwritten or destroyed by other scripts.
To solve this problem AngularJS uses modules.


##AngularJS Modules
Using a simple controller:
````
<!DOCTYPE html>
<html>
<body>
<div ng-app="" ng-controller="myCtrl">
{{ firstName + " " + lastName }}
</div>
<script>
function myCtrl($scope) {
    $scope.firstName = "John";
    $scope.lastName = "Doe";
}
</script>
<script src="//ajax.googleapis.com/ajax/libs/angularjs/1.2.15/angular.min.js"></script>
</body>
</html>
Using a controller owned by a module instead:
<!DOCTYPE html>
<html>
<head>
<script src="//ajax.googleapis.com/ajax/libs/angularjs/1.2.15/angular.min.js"></script>
</head>
<body>
<div ng-app="myApp" ng-controller="myCtrl">
{{ firstName + " " + lastName }}
</div>

<script>
var app = angular.module("myApp", []);
app.controller("myCtrl", function($scope) {
    $scope.firstName = "John";
    $scope.lastName = "Doe";
});
</script>
</body>
</html>
````
Note that the AngularJS library is loaded in the <head> section in this example.


###Where to Put Module Definitions
A common advise for HTML applications, is to place all scripts at the very bottom of the <body> element.
This improves page loading, because HTML loading is not blocked by scripts loading.
In many AngularJS examples, you will se the Angular library being loaded in the <head> of the document.
In the example above, AngularJS is loaded in the <head> element, because the call to angular.module can only be done after the library has been loaded.
Another solution is to load the AngularJS library in the <body> element, but in front of your own AngularJS scripts:
````
<!DOCTYPE html>
<html>
<body>
<div ng-app="myApp" ng-controller="myCtrl">
{{ firstName + " " + lastName }}
</div>
<script src="//ajax.googleapis.com/ajax/libs/angularjs/1.2.15/angular.min.js"></script>
<script>
var app = angular.module("myApp", []);
app.controller("myCtrl", function($scope) {
    $scope.firstName = "John";
    $scope.lastName = "Doe";
});
</script>
</body>
</html>
````

###AngularJS Application Files
Now that you know what modules are, and how they work, it is time to build your application files.
Your application should have at least one module file, and one controller file for each controller.
First create a module file "myApp.js":
````
var app = angular.module("myApp", []);
````
Then create the controller file(s). In this case "myCtrl.js":
````
app.controller("myCtrl", function($scope) {
    $scope.firstName = "John";
    $scope.lastName = "Doe";
});
Finally, edit your HTML page:
<!DOCTYPE html>
<html>
<body>
<div ng-app="myApp" ng-controller="myCtrl">
{{ firstName + " " + lastName }}
</div>
<script src="//ajax.googleapis.com/ajax/libs/angularjs/1.2.15/angular.min.js"></script>
<script src="myApp.js"></script>
<script src="myCtrl.js"></script>
</body>
</html>
````

An AngularJS Application Example
Application Explained
````
<html ng-app="myNoteApp">
<body>
<div ng-controller="myNoteCtrl">
<h2>My Note</h2>
<p><textarea ng-model="message" cols="40" rows="10"></textarea></p>
<p>
<button ng_click="save()">Save</button>
<button ng-click="clear()">Clear</button>
</p>
<p>Number of characters left: <span ng-bind="left()"></span></p>
</div>
<script src="//ajax.googleapis.com/ajax/libs/angularjs/1.2.15/angular.min.js"></script>
<script src="myNoteApp.js"></script>
<script src="myNoteCtrl.js"></script>
</body>
</html>
````
The application file "myNoteApp.js":
var app = angular.module("myNoteApp", []);
The controller file "myNoteCtrl.js":
````
app.controller("myNoteCtrl", function($scope) {
    $scope.message = "";
    $scope.left  = function() {return 100 - $scope.message.length;};
    $scope.clear = function() {$scope.message = "";};
    $scope.save  = function() {$scope.message = "";};
});
````

The `<html>` element is the container of the AngularJS application: `ng-app="myNoteApp"`:
`<html ng-app="myNoteApp">`
A a `<div>` in the HTML page is the scope of a controller: `ng-controller="myNoteCtrl"`:
`<div ng-controller="myNoteCtrl">`
An `ng-model` directive binds a `<textarea>` to the controller variable message:
<textarea ng-model="message" cols="40" rows="10"></textarea>
The two `ng-click` events invoke the controller functions clear() and save():
````
<button ng_click="save()">Save</button>
<button ng-click="clear()">Clear</button>
````
An `ng-bind` directive binds the controller `function left()` to a `<span>` displaying the characters left:
Number of characters left: `<span ng-bind="left()"></span>`
The AngularJS library is added to the page:
````
<script src="//ajax.googleapis.com/ajax/libs/angularjs/1.2.15/angular.min.js"></script>
````
Your application libraries are added to the page (after the library):
````
<script src="myNoteApp.js"></script>
<script src="myNoteCtrl.js"></script>
````

##AngularJS Application Skeleton
Above you have seen the skeleton (staff holding) of a real life AngularJS, single page application (SPA).

The `<html>` element is the "container" for the AngularJS application (`ng-app=`).
A <div> elements defines the scope of an AngularJS controller (ng-controller=).
You can have many controllers in one application.

An application file (my...App.js) defines the application module code.
One or more controller files (my...Ctrl.js) defines the controller code.



##Summary - How Does it Work?
The recommended placement for AngularJS script tags is at the bottom of the <body> element. This improves page load time, because the HTML loading is not blocked by scripts loading.

Application code (module and controllers) must be loaded after the library, because they depend on the library code.
The ng-app directive should be placed at the root element the application.
For single page applications (SPA) the root of the an application is often the the <html> element.
One or more ng-controller directives define the application controllers. Each controller has its own scope: the HTML element where they were defined.

AngularJS starts automatically on the HTML DOMContentLoaded event. If an ng-app directive is found, AngularJS will load any module named in the directive, and compile the DOM with ng-app as the root of the application.
The root of the application can be the whole page, or a smaller portion of the page. The smaller portion, the faster the application will compile and execute.
