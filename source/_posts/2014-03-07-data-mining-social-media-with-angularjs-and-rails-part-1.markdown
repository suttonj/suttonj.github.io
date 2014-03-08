---
layout: post
title: "Data Mining Social Media with AngularJS and Rails - Part 1"
date: 2014-03-07 23:24:25 -0500
comments: true
categories: [Web Development]
---

AngularJS has seen a steady rise in popularity over the past year, and for good reason. Many Angular devs like to use Node.js as the back-end to keep the stack all one language; however, there are plenty of Rails users that want to get in on the fun without changing their whole stack. 

This series of posts will show you how to set up a simple data mining web app aimed at social media, using AngularJS on the front-end and Ruby on Rails for the back-end.
<!--more-->

<strong>Why use AngularJS and Rails?</strong>

There are many reasons why AngularJS has shot up the ranks of JavaScript frameworks:
<ul>
	<li>it's backed by Google, and as such has access to the immense vault of talent and resources that the tech behemoth has,</li>
	<li>it's a top-tier MVC framework well-suited for single-page web applications based in HTML,</li>
	<li>it enables mass-scale parallel development,</li>
	<li>it helps easily manage the state of the application,</li>
	<li>just to name a few...for a more in-depth explanations of the benefits of using AngularJS, read <a title="10 Reasons Web Developers Should Learn Angular" href="http://java.dzone.com/articles/10-reasons-web-developers">this article.</a></li>
</ul>

Single page applications are becoming prevalent on the web and in the app stores, and Angular has shown to be one of the best front-end frameworks to get one up and running quickly and easily.

We're going to use Ruby on Rails on the back-end to serve the data and perform the data-mining; Rails is well-established and there's already tons of documentation on the web, so it should be easy to pick up for anyone who isn't familiar. Rails is especially popular among start-ups and smaller companies in the software industry, so it's useful to have that in your skill set. Not to mention, Rails makes it incredibly fast and easy to get a working application up and running.

While both Rails and Angular have plenty of documentation to accompany them, there isn't much information on how to integrate the two pipelines. I'll start off by showing you how to set up the two frameworks to seamlessly work together. This tutorial assumes that you have a basic knowledge of Rails.

First off, we'll create our new project. From your working directory:
<pre class="lang:default decode:true ">$ rails new socialminer --database=postgresql --skip-test-unit</pre>
After the project files are created, configure your Postgres database by editing the /config/database.yml file. If you're migrating to Postgres from another type of database, <a title="" href="http://railscasts.com/episodes/342-migrating-to-postgresql">this RailsCast</a> may help.

Once the configuration is set, create the database:
<pre class="lang:default decode:true ">$ bundle exec rake db:create</pre>
Run the server to check that everything is working.


<strong>Adding the AngularJS framework</strong>

<i><b>Note:</b> If you have Turbolinks enabled in your Rails app, disable it (delete all references to it). It will conflict with Angular.</i>

Integrating AngularJS with your Rails app is pretty straight-forward and easy. Download the latest version of AngularJS from <a href="http://angularjs.org/">the website.</a>

We're also going to add Underscore.js to the mix too, because it provides some valuable functionality to JavaScript that'll make things easier for us. Download Underscore.js from <a href="http://underscorejs.org/"> the website.</a>

Unzip both downloads and add all of the javascript files to the <code>vendor/assets/javascripts</code> folder.

To provide a quick and powerful UI for us to work with, we'll use Bootstrap; to do that, add the Bootstrap gem to your Gemfile, and include Bootstrap in the application.css file:

{% codeblock lang:css Gemfile%}
...
gem 'bootstrap-sass'
...
{% endcodeblock %}

{% codeblock lang:css app/assets/stylesheets/application.css%}
...
 *= require_self
 *= require_tree .
 *= require bootstrap
 */

@import "bootstrap";

{% endcodeblock %}

Head over to the <code>app/assets/javascripts</code> folder and add Underscore.js, Angular, and Bootstrap to the Sprockets declarations in the <code>application.js</code> file:

{% codeblock lang:javascript app/assets/javascripts/application.js %}
...
//= require jquery
//= require jquery_ujs
//= require underscore
//= require angular
//= require bootstrap
...
{% endcodeblock %}

Update your gems to add Bootstrap to the pipeline:
<pre>$ bundle install</pre>

Now that we have all the UI components in place, we can create our main controller:
<pre>$ rails generate controller Main index</pre>

Add the root folder to the routes.rb file to redirect to the main controller:

{% codeblock lang:ruby app/config/routes.rb %}
...
root :to => 'main#index'
...
{% endcodeblock %}
Run the rails server and make sure everything is working smoothly.


<strong>Setting up Angular</strong>

In order to properly use the Angular framework, we have to create a simple directory structure for our Angular files to reside in. How this structure is implemented is up to you and depends on how complex your app will be; the directory structure is important for maintaining and managing your app as it grows.

In this tutorial, we'll use the generally accepted structure of having folders for services, filters, controllers, and directives.

Your folder structure should look like this:
<p>
<img src="http://i.imgur.com/5juLUo2.png" alt="Javascript directory structure" />
</p>
If you want to know more about how to structure your directory for app growth and scaling, as well as general styling guidelines for Angular, you can <a href="https://github.com/mgechev/angularjs-style-guide"> read this informative guide on github.</a>

We can test out our folder structure by creating basic controller for the main view app:

{% codeblock lang:coffeescript app/assets/javascripts/controllers/MainCtrl.js.coffee%}
@MainCtrl = ($scope) ->
	$scope.data = 
		posts: [
			{ content: "hello world!"},
			{ content: "123 test"}
		]

@MainCtrl.$inject = ['$scope']
{% endcodeblock %}

If the code above looks strange to you, it's because we're using Coffeescript in place of JavaScript. CoffeeScript is just syntactic sugar that compiles into JavaScript and is included by default in Rails 4.0 applications. It bears resemblence to the Ruby language so it's easy for a Ruby/Python/Haskell enthusiast to pick up, and it integrates nicely with the rest of the Rails pipeline.

I'm using some placeholder data for now while we get the app up and running. The view for this controller, which is also the main view, will look like this:

{% codeblock lang:html app/assets/views/main/index.html %}
<h1 class='text-center'>Social Miner AngularJS+Rails App</h1>
<div class="container" ng-controller="MainCtrl">
  <h1 class="text-center"&gt;My blog</h1>
 <div class="row" ng-repeat="post in data.posts">
      <p>{{ post.content }}</p>
  <div>
</div>
{% endcodeblock %}

We also want to create the module that will be our main application module, which will we declare in our javascripts folder:

{% codeblock lang:coffeescript app/assets/javascripts/main.js.coffee %}
#= require_self
#= require_tree ./controllers
#= require_tree ./directives
#= require_tree ./filters
#= require_tree ./services

Miner = angular.module('Miner', [])
{% endcodeblock %}

Our main application module is named 'Miner'. Now we need to specift the <code>ng-app</code> directive in our main application template:

{% codeblock lang:html app/views/layout/application.html.erb %}
<!DOCTYPE html>
<html ng-app="Miner">
<head>
  <title>Socialminer</title>
  <%= stylesheet_link_tag    "application", :media => "all" %&>
  <%= javascript_include_tag "application", controller_name %>
  <%= csrf_meta_tags %>
</head>
<body>

<%= yield %>

</body>
</html>
{% endcodeblock %}

In addition to adding <code>ng-app</code> to the html tag, add <code>controller_name</code> to the javascript include tag in order to include the view's controller.

Now that the Angular framework is set up, we can navigate to our main page at <b>http://localhost:3000</b> to confirm that everything works. You should see the index page with the contents of the sample data we provided.


<strong>Conclusion</strong>
So far, we've set up our Rails app, created our Angular module, and implemented a view and a controller for the main page. This will provide the base for our application.

In Part 2 of this series, we will implement routing in Angular and create an API for accessing social media. 

