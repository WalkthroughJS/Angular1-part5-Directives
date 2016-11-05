# Angular1-part5-Directives

So, in the last lesson, we talked about different types of services and how you can inject and implement them. In this lesson, though, we're going to talk about the illusive directive and what that is. Directives can manifest in many different ways, but you'll see them almost always either as an attribute within an element or an element itself. For example, `ng-repeat` and `ng-click` are both attribute directives. Element directives are usually used to imitate components or functional templates of some sort. You'll see what I mean when we get there. We may have to do this in a couple parts, though, so stay tuned.

Let's get started, though. Let's go ahead and copy/paste the `app.js` and the `index.html` as we did last time and make sure that they worked just as where we left off last time. 

How do we even start a directive, though? Let's go to `app.js` and add `app.directive()` below the `app.factory` that we have. Inside the `app.directive()` parens, we're going to put two arguments, just like the last couple of times. The first argument is going to be the name, which we will aptly call `myFirstDirective`. The second argument, like before, is going to be a callback function. Your `app.js` will look like this now:

```text
var app = angular.module('myFirstNgApp', []);

app.controller('myFirstController', function($scope, $http, myFirstFactory) {
  $scope.makeAPIcall = function(character) {
    myFirstFactory.getCharacter(character)
     .then(function(api_response) {
       console.log(api_response);
       $scope.results = api_response.data.results;
     });
  }
});

app.service('myFirstService', function($http) {
  this.getCharacter = function(character){
    return $http.get('https://swapi.co/api/people/?search=' + character);
  }
});

app.factory('myFirstFactory', function($http) {
  return {
    getCharacter: function(character){
      return $http.get('https://swapi.co/api/people/?search=' + character);
    }
  }
});

app.factory('myFirstDirective', function() {
  
});
```

Within the callback function, we're going to return an object. That object is going to have two key/value pairs in it. The first will be `restrict: 'E'` and the second will be ```text template: '<h1>Hello world</h1>'```. The `restrict: 'E'` key/value will make the DOM expect this to be an element, hence the "E", and the second key/value will make the element output ```text <h1>Hello world</h1>```

Now, let's go back to the DOM and put this new directive in here. Let's remove the div with ``{{serviceValue}}`` and replace it with ```text<my-first-directive></my-first-directive>```. Note the camel-case to snake-case transition. This is the case with all directives that you name. It will be camel-case in your directive declaration and snake-case in your HTML. Now, let's save it and refresh the page. Now we see an `h1` of 'Hello world' on the page. You have officially created your first directive! Awesome! But alas, there's more to directives than just that.

You can actually pass values into directives from the parent controller, which in this case is `myFirstController`. You have the ability to pass three different things into directives. You can pass a "string" (denoted by "@"), which is only from the controller to the directive, a scope function (denoted by "&"), which you declare in the controller and call in the directive, or a two way binding for a scope variable in the parent controller (denoted by "=") that you can pass back and forth between the controller and the directive.

To prove that, let's start by passing a string into the directive. To do that, let's go into the directive and add another key inside the return object called `scope`. This key is an object and labels values from the controller that it expects to see as keys inside its own object. That may be hard to picture, so lets write it out. Inside the ```text <my-first-directive></my-first-directive>```, let's put an attribute of `string="stuff"` since we feel all arbitrary today. It will look like: ```text <my-first-directive string="stuff"></my-first-directive>```

Now, let's go into `app.directive` and add our scope key to our return object. To the `scope` object, we're going to add a key called `string`, since that's what we called it inside the element, then we're going to put the value as "@", since it is just a string, like was mentioned a couple of paragraphs back. Last thing, inside the template key of the directive's return object, we're going to put ```text <p>{{string}}</p>``` just to make it show on the page. It's going to look like this now: 
```text
app.directive('myFirstDirective', function() {
  return {
    restrict: 'E',
    scope: {
      string: '@'
    },
    template: '<h1>Hello World</h1> <p>{{string}}</p>'
  }
});
```

After you save and refresh the page, you'll notice that the string "stuff" shows up on the page. Awesome! So we were able to pass a string into a directive. Let's move on to passing a two-way data binding scope variable into the directive.

Let' start by adding another key to the scope object in the directive. Let's call it `binder`, then let's add "=" as the value so we can make the directive expect a scope variable from the parent controller. I also added `\n` (new line) characters to make it a little easier to read.
```text
app.directive('myFirstDirective', function() {
  return {
    restrict: 'E',
    scope: {
      binder: '=',
      string: '@'
    },
    template: '<h1>Hello World</h1>\n<p>string: {{string}}</p>\n<p>binder: {{binder}}</p>'
  }
});
```
Now, let's go into the controller and add a scope variable called `twoWay` and assign it to a value of 3. 
```text
app.controller('myFirstController', function($scope, $http, myFirstFactory) {
  $scope.makeAPIcall = function(character) {
    myFirstFactory.getCharacter(character)
     .then(function(api_response) {
       console.log(api_response);
       $scope.results = api_response.data.results;
     });
  }
  $scope.twoWay = 3;
});
```

From there, let's go back to the DOM and add `binder="twoWay"` to the `my-first-directive` element. It will look like this:
```text
<my-first-directive string="stuff" binder="twoWay"></my-first-directive>
```
Now we see all the values output into the DOM, including the two-way binding value!

The last thing we're going to do is pass in a function to alert something from inside the directive. Let's start by going to `app.controller` and adding another scope function. Let's call it `$scope.directiveAlert`. Let's just make it alert `$scope.twoWay`. It will look like this: 
```text
app.controller('myFirstController', function($scope, $http, myFirstFactory) {
  $scope.makeAPIcall = function(character) {
    myFirstFactory.getCharacter(character)
     .then(function(api_response) {
       console.log(api_response);
       $scope.results = api_response.data.results;
     });
  }
  $scope.twoWay = 3;

  $scope.directiveAlert = function() {
    alert($scope.twoWay);
  }
});
```
