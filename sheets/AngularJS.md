#Angular#

#Module and Controller#

Angulars top level container is a module.

app.js
```js
(function(){
  var gem = { name: 'Azurite', price: 2.95 };
  var app = angular.module('gemStore', []);
  app.controller("StoreController", function(){
    this.product = gem;
  });
})();
```
In here we have defined a new angular module as app, with a name attribute of 'gemStore' and currently no dependencies on further libraries hence the empty array.

We have defined a controller which will deal with the state and actions for a particular element of the DOM, with the name 'StoreController' (note: it is important to use camel-case and that Controller is the last word). The function of this can then containe the state and actions.

index.html
```html
<!DOCTYPE html>
<html ng-app="gemStore">
  <head>
    <link rel="stylesheet" type="text/css" href="bootstrap.min.css" />
    <script type="text/javascript" src="angular.min.js"></script>
    <script type="text/javascript" src="app.js"></script>
  </head>
  <body ng-controller="StoreController as store">
    <div class="product row">
      <h3>
        {{store.product.name}}
        <em class="pull-right">{{store.product.price}}</em>
      </h3>
    </div>
  </body>
</html>
```
We have declared all the html to be related to the 'gemStore' app with ng-app directive in the html tag and defined the scope of the controller as all the elelments inside the body tag using `ng-controller` and aliasing 'StoreController' to store to keep the code brief.

After doing this all attributes of the controller are able to be accessed in the code using `{{store.product.name}}` notation to reach inside the objects in the controller.

