# angular-satellite
An angular publish / subscribe library that promotes event channels as duck-typed methods


  Provides a high level api for managing angular publish / subscribe events in a mor uniform way. It makes use of the global
  $rootScope object (single point of entry for any controller, service, directive in a single angular application) to abstract event
  subscriptions under duck-typed methods. That way we keep our event namepsaces clean and uniform.

 For example

 ===========================
 Without Angular Satellite
 ===========================
```javascript
       //content of myController.js
       function myController($scope, $rootScope) {
           var clean = function () { ...do some cleanup }
           $scope.exit = function () {
               $rootScope.$broadcast('exit:button:click', clean)
           };
       }

       //content of  myDirective.js
       function myDirective() {
           return {
               restrict: 'A',
               link: function () {
                   $rootScope.on('exit:button:click', function ($event, cleanUp) {
                       cleanUp() //do some cleanup
                   })
               }
           }
       }
```

 ===========================
 With Angular Satellite 
 ===========================
 ```javascript
       //content of myController.js
       function myController($scope, Satellite) {
           Satellite.setupEvent('popup', 'exit');
           $scope.exit = function () {
               Satellite.popup.raise.exit(clean)
           }
       }

       //content of  myDirective.js
       function myDirective(Satellite) {
           return {
               restrict: 'A',
               link: function () {
                   Satellite.popup.on.exit(function (cleanUp) {
                       cleanUp() //do some cleanup
                   })
               }
           }
       }
```

  See more examples at ../tests/satellite.html
  
