---
layout: post
title: "Data Mining Social Media with AngularJS and Rails - Part 2"
date: 2014-03-08 00:30:08 -0500
comments: true
published: false
categories: ["Web Development"]
---

In Part 1, we set up our Rails app and integrated AngularJS and a the Bootstrap UI with it. 

<strong>Angular Routing</strong>

Inevitably, an application will grow past the point where declaring all of our page components in one index.html file will become too cumbersome and hard to maintain. This is where routing comes in. We can tell Angular to display a certain view depending on the URL that is used making it easy to wire together controllers and views in a RESTful manner.

First, add the Angular route module:

<code>app/assets/javascripts/main.js.coffee</code>
{% codeblock lang:coffeescript%}
...
#= require angular-route

#create new Angular module called 'Miner'
Miner = angular.module('Miner', ['ngRoute'])

#set up routing for the Miner module
Miner.config(['$routeProvider', ($routeProvider) ->
    $routeProvider
        .when('/posts', {
            templateUrl: '/assets/templates/mainIndex.html',
            controller: 'MainCtrl'
        })
        .otherwise({
            templateUrl: '../assets/mainIndex.html',
            controller: 'MainCtrl'
        })
])
{% endcodeblock %}

Add a new folder in the assets folder named <code>template</code> and then transfer the portion of the index.html page that shows the actual content to a partial view named mainIndex.html:

<code>assets/templates/mainIndex.html</code>
{% codeblock lang:html %}
<h1 class='text-center'>SocialMiner AngularJS + Rails App</h1>
<div class="row" ng-repeat="post in data.posts">
    <p>{{ post.content }}</p>
</div>
{% endcodeblock %}