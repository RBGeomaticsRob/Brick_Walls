#Angular#

WOrking through the code school angular code, my notes and code samples.

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

##Directives##

Angular has a bunch of built in directives for controlling DOM elements and controller behaviour.

###ng-show and ng-hide###
Are set equal to a boolean variable to hide or show a DOM element. i.e. `ng-show = "store.product.available"`

###ng-repeat###
Will loop through all items in an array variable and create multiple elements inside a DOM structure.
i.e. `ng-repeat= "product in store.products"`

###ng-src###
Allows an element to be loaded in as a source location. Assuminging there is an array of image paths in product the following would work `<img ng-src="{{product.images[0]}}">`

##Expression Filters##

It is possible to filter how the data is displayed using filters, you may want to format a date or a currency or show so many characters or only show so many items in an ng-repeat, filters work by piping the data into a filter in the expression.

For example
```html
<p>{{product.price | currency}}</p>
```

###2 way data binding###

We can use ng-model in the front end to provide two way data binding in this case to show a preview of a review on the screen before using our controller to commit this review on submit

index.html
```html
<!--  Review Form -->
<form name="reviewForm" ng-controller="ReviewController as reviewCtrl" ng-submit="reviewCtrl.addReview(product)">

  <!--  Live Preview -->
  <blockquote>
    <strong>{{reviewCtrl.review.stars}} Stars</strong>
    {{reviewCtrl.review.body}}
    <cite class="clearfix">—{{reviewCtrl.review.author}}</cite>
  </blockquote>

  <!--  Review Form -->
  <h4>Submit a Review</h4>
  <fieldset class="form-group">
    <select ng-model="reviewCtrl.review.stars" class="form-control" ng-options="stars for stars in [5,4,3,2,1]" title="Stars">
      <option value="">Rate the Product</option>
    </select>
  </fieldset>
  <fieldset class="form-group">
    <textarea ng-model="reviewCtrl.review.body" class="form-control" placeholder="Write a short review of the product..." title="Review"></textarea>
  </fieldset>
  <fieldset class="form-group">
    <input ng-model="reviewCtrl.review.author" type="email" class="form-control" placeholder="jimmyDean@example.org" title="Email" />
  </fieldset>
  <fieldset class="form-group">
    <input type="submit" class="btn btn-primary pull-right" value="Submit Review" />
  </fieldset>
</form>
```
app.js
```js
  app.controller('ReviewController', function(){
    this.review = {};
    this.addReview = function(product){
      product.reviews.push(this.review);
      this.review = {};
    };
  });
```

##Form Validations##

Turn off default HTML validations `novalidate` in the form tag.

Mark the fields required for validation in their tags with `required`

We can print it to screen `{{reviewFrom.$valid}}`

We can stop the form from being called by adding it to other code in ng-submit, i.e. `ng-submit = "reviewForm.$valid && reviewCtrl.addReview(product)"` reviewForm is the name attribute of the form.

For forms that are in the process of being filled in but not valid angular assigns them the class `ng-dirty` for anything currently being filled in and `ng-invalid` for anything not valid. These can then be styled using css to give them, for example, a different border colour to help inform the user.

```css
.ng-invalid.ng-dirty {
border-color: red;
}

.ng-valid.ng-dirty {
border-color: green;
}
```

##Custom Directives##

###Element Directives###
Element directives allow you to extract template HTML to separate files and call them using custom directive tags, these tags can still also include other ng- directives.

index.html
```html
<product-description ng-show="tab.isSet(1)">
</product-description>
```
product-descriptions.html
```html
<div>
  <h4>Description</h4>
  <blockquote>{{product.description}}</blockquote>
</div>
```
app.js
```js
app.directive("productDescription", function() {
  return {
    restrict: 'E',
    templateUrl: "product-description.html"
  };
});
```
###Attribute Directives###
Attribute directives allow you to add html inside current tag structures. This can be used for extending the current html, for example for a tool-tip

index.html
```html
<div product-specs ng-show="tab.isSet(2)">
</div>
```

product-specs.html
```html
<h4>Specs</h4>
<ul class="list-unstyled">
  <li>
    <strong>Shine</strong>
    : {{product.shine}}</li>
  <li>
    <strong>Faces</strong>
    : {{product.faces}}</li>
  <li>
</ul>
```

app.js
```js
app.directive("productSpecs", function(){
  return {
    restrict: 'A',
    templateUrl: "product-specs.html"
  };
});
```

###Directives with Controllers###

Where a directive template is wanted and a controller is already an attribute in that tag, the controller can be included in the directive and the alias set also in the directive.

index.html
```html
<product-tabs></product-tabs>
```

product-tabs.html
```html
<section>
  <ul class="nav nav-pills">
    <li ng-class="{ active:tab.isSet(1) }">
      <a href ng-click="tab.setTab(1)">Description</a>
    </li>
    <li ng-class="{ active:tab.isSet(2) }">
      <a href ng-click="tab.setTab(2)">Specs</a>
    </li>
    <li ng-class="{ active:tab.isSet(3) }">
      <a href ng-click="tab.setTab(3)">Reviews</a>
    </li>
  </ul>

  <!--  Description Tab's Content  -->
  <div ng-show="tab.isSet(1)" ng-include="'product-description.html'">
  </div>

  <!--  Spec Tab's Content  -->
  <div product-specs ng-show="tab.isSet(2)"></div>

  <!--  Review Tab's Content  -->
  <product-reviews ng-show="tab.isSet(3)"></product-reviews>
</section>
```

app.js
```js
app.directive("productTabs", function(){
  return {
    restrict: 'E',
    templateUrl: 'product-tabs.html',
    controller: function(){
      this.tab = 1;

      this.isSet = function(checkTab) {
        return this.tab === checkTab;
      };

      this.setTab = function(setTab) {
        this.tab = setTab;
      };
    },
    controllerAs: 'tab'
  };
});
```

