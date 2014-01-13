---
title: Simple AngularJS Forms
layout: post
---

Reactive programming and templating are extremely powerful because they allow you to define how your UI should work, without the need to manually manipulate the UI. 

Here is an extremely simple example of a form that should calculate and display an area to the user, based on the length and width.

```html
<form id="area-form">
    <input id="length" type="number" step="any" placeholder="Length" value="9" />
    <input id="width" type="number" step="any" placeholder="Width" value="25.75" />
    <input id="area" type="number" step="any" placeholder="Area" readonly="readonly" />
</form>
```

Before discovering reactive programming, this is how I would code the dynamic behavior of the form.

```coffeescript
jQuery ->
 $('#length, #width').bind 'change keyup', ->
   $('#area').val(parseFloat($('#length').val()) * parseFloat($('#width').val())) 
 .change()
```
[JSFiddle Simple jQuery Form](http://jsfiddle.net/abstractcoder/w3NJK/)

This worked well for simple forms, but it always felt WRONG. As the complexity of calculations increased the debugging complexity increased exponentially.

I experimented with various frameworks that included reactive programming and templating functionality such as [AngularJS](http://angularjs.org/), [Ember.js](http://emberjs.com/), [Meteor](https://www.meteor.com/), as well as many others. Having the UI automatically update as the underlying values change felt like magic, but I still wanted to handle everything else server side. Fortunately I found a way to use only the reactive and templating features of AngularJS.

Here is the same example rewritten using AngularJS

{% raw %}
```html
<form id="area-form" ng-controller="Template as template">
    <input id="length" type="number" step="any" placeholder="Length" ng-model="template.length" />
    <input id="width" type="number" step="any" placeholder="Width" ng-model="template.width" />
    <input id="area" type="number" step="any" placeholder="Area" readonly="readonly" value="{{template.getArea()}}" />
</form>
```
{% endraw %}

```coffeescript
class Template
  constructor: (@$scope) ->
    @length = 9
    @width = 25.75
  getLength: -> parseFloat(@length)
  getWidth: -> parseFloat(@width)
  getArea: -> @getLength() * @getWidth()
  
Template.$inject = ["$scope"];
app = angular.module 'templateApp', []
app.controller 'Template', Template
angular.bootstrap document.getElementById('area-form'), ['templateApp']
```

[JSFiddle Simple AngularJS Form](http://jsfiddle.net/abstractcoder/3jfk8/)

This might seem like overkill for such a simple form, but I find it to be much cleaner and readable. As the forms and calculations grow in size and complexity that becomes much more important.