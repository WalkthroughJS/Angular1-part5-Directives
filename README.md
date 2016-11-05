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

Within the callback function, we're going to return an object. That object is going to have two key/value pairs in it. The first will be `restrict: 'E'` and the second will be ```text template: '<h1>Hello world</h1>'```
