# ember-rails hands-on intro

!SLIDE

# ember-rails hands-on intro

## Matt Gauger

### Mad Railers 2013-05-20

!SLIDE

## I work at Bendyworks

![Bendyworks logo](images/bendy.png)

<div class="notes">
Stephen, my boss here at Bendyworks, asked me to give this talk after I gave a brownbag technical talk on Ember-rails and how I'd used it for the Snow Mobile Raffle app.

I want to show you how easy it is to include Ember-rails into your Rails app, and to use the default generators to start building your Ember app. And I'll have some lessons learned at the end.
</div>

!SLIDE

## Disclaimer:

### I spend most of my time writing Backbone code that talks to Ruby services right now

<div class="notes">
Quite a few of us on the Bendyworks team have been writing Backbone since late last year on a big project for a client, and so I'm most familiar with Backbone.

I have a lot of good things to say about Backbone, and a lot of hope for Ember. I don't always have good thing sto say about either Ember or Backbone, though.
</div>

!SLIDE

## What is Ember.js?

A framework for JS MVC apps

<div class="notes">
So Ember.js is a framework for JS MVC apps
Or, another way of architecting these fancy websites we keep trying to build
- We're building more complicated apps.
- More work is being done on the clientside.
- We're integrating with more external APIs where we don't control the server.
- It's cool!
</div>

!SLIDE
![Emberjs.com website](images/emberjscom.png)

<div class="notes">
If you're curious, you can go hit up the ember.js website.
Their guides have gotten much better and their documentation for functions and classes is better now than it used to be, too.
</div>

!SLIDE

## Why Ember.js?

<div class="notes">
Why do we need another JS MVC framework?

One size does not fit all.
Ember has some unique features that take the burden of uninteresting problems off the developer.
For example, Ember binds elements that change to DOM elements and updates them for you -- you don't have to call `render` like in other frameworks. I'll talk more about this later.
</div>

!SLIDE

## MVC => Model, View, Controller

<div class="notes">
So what does this really mean for a JS app?

This is going to be for a JavaScript MVC app, which is a little different than what Rails considers MVC.
</div>

!SLIDE

## MVC => Model, View, Controller

(Let's not get in a debate as to whether it's really MVC when we say JS MVC, ok?)

!SLIDE

## Templates

### Strings that we will render with data to present the user interface
#### (Note: not in the acroynm)

<div class="notes">
In Rails, this is what we call the View.. notice how we never write a View class in Rails? You could, but you don't have to.
</div>

!SLIDE

## View classes

### Translates browser events to application events
### Binds data to a template

!SLIDE

## Controller classes

### Stores representations of application state
#### ie, a collection of model instances that came back from our API

!SLIDE

## Model classes

### Stores persistant state

Typically loaded and saved to a server, but doesn't have to be

!SLIDE

## Router and route classes

### Manages application state

Most importantly, handles URLs and sends messages the correct Controller for what is being requested

!SLIDE

## How does Ember.js compare to other frameworks?

!SLIDE

## Favors convention-over-configuration

Like Rails!

!SLIDE

## Opinionated about what your API should look like

This is the volatile `ember-data` project; now a part of Ember.js itself

<div class="notes">
Other frameworks let you define the API and how you talk to it. But that's at the expense of developer time -- now you have to spend time maintaining your mapping to your API in addition to the business logic!
</div>

!SLIDE

## Ember handles rendering and re-rendering DOM elements that are bound to data

Other frameworks leave calling `render` up to you

<div class="notes">
As a result, this means that Ember performs best when it takes over the entire viewport.

This is unlike other frameworks in that you can't just use the framework for part of your page. It wants to be the whole app.
</div>

!SLIDE

## Push state (history), URLs are handled by Ember by default

Other frameworks let you turn this on

!SLIDE

## Ember.js is a whole framework

Other frameworks (like Backbone) are more like LEGO blocks

You can combine those frameworks in many ways

Ember is a set of reasonable defaults

!SLIDE

## Our example project:
## A guest book

Because [Discourse](https://github.com/discourse/discourse) is a forum system on Rails and Ember, and we want to party like it's 1999!

!SLIDE

![dancing baby](images/dancing_baby.gif)

!SLIDE

## A quick refresher

### On the parts of Rails we'll be using

!SLIDE

## Rails resource generators:

``` bash
$ rails generate entry \\
   name:string email:string \\
   message:text
```
!SLIDE

## Gives us:

Routes:

``` ruby
resources :entries
```

`app/models/entry.rb` <br />
`app/controllers/entries_controller.rb` <br />

(no views!)

<div class="notes">
If we wanted views, we'd use `rails generate scaffold`
</div>

!SLIDE

## `active_model_serializers`

### Calling `rails generate resource entry` as before gives us:

`app/serializers/entry_serializer.rb`:

``` ruby
class EntrySerializer < ActiveModel::Serializer
  attributes :id, :name, :email, :message
end
```

<div class="notes">
- Gives us the JSON resource
- works with teh same resource generator as `rails generate resource` before

We can trust it to pass those attributes for now, without any additional logic
</div>

!SLIDE

## Introducing `ember-rails`

Works with the same `rails generate resource entry` command we used earlier.

`active_model_serializers` gives us the API expected by `ember-data`

!SLIDE 

From `rails generate resource entry` with `ember-rails`, we get:

``` bash

app/assets/javascripts/
├── application.js
├── controllers
│   ├── application_controller.js
│   ├── entry_controller.js
│   └── entries_controller.js
├── helpers
├── models
│   └── entry.js
├── router.js
├── routes
│   ├── application_route.js
│   └── entries_route.js
├── guest_book_app.js
├── store.js
├── templates
└── views
```

!SLIDE

`application.js`

``` javascript
//= require jquery
//= require jquery_ujs
//= require handlebars
//= require ember
//= require ember-data
//= require_self
//= require guest_book_app

GuestBookApp = Ember.Application.create();
```

Requires all our code and instantiates the Ember app.

!SLIDE

`guest_book_app.js`

Our application code, in turn, requires all of the Ember application classes:

``` javascript
//= require ./store
//= require_tree ./models
//= require_tree ./controllers
//= require_tree ./views
//= require_tree ./helpers
//= require_tree ./templates
//= require ./router
//= require_tree ./routes
//= require_self
```

!SLIDE

`store.js`:

``` javascript
App.Store = DS.Store.extend({
  revision: 12
});
```

Keeps track of the `ember-data` API version we're on

<div class="notes">
Since `ember-data` has traditionally been in flux, they felt it was important to specify it in a file.
</div>

!SLIDE

`router.js`:

``` javascript
GuestBookApp.Router.map(function() {
  this.resource('entries', { path: "/" });
});
```

(by default)

!SLIDE

`router.js` with changes:

``` javascript
GuestBookApp.Router.map(function() {
  this.resource('entries', function() {
    this.route('new');
    this.resource('entry', {path: ':entry_id'});
  });
});
```

Create and view guest book entries in addition to an index

!SLIDE

`/routes/entries_routes.js`:

``` javascript
GuestBookApp.EntriesRoute = Ember.Route.extend({
  model: function() {
    GuestBookApp.Entry.find();
  }
});
```
<div class="notes">
Assuming we were a request for /entries, we go down into the index route now.

This "find all" call on the model might get done by default now, but I wanted to point out that this is what calls our Rails resource to get the data.
</div>

!SLIDE

`/controllers/entries_controller.js`:

``` javascript
GuestBookApp.EntriesController = Ember.ArrayController.extend({});
```

<div class="notes">
Note that we're extending the ArrayController here since we handle a collection of model instances.
By default, there's not a whole lot here.
</div>

!SLIDE

`/models/entry.js`:

``` javascript
GuestBookApp.Entry = DS.Model.extend({
  name: DS.attr("string"),
  email: DS.attr("string"),
  message: DS.attr("string")
});
```

<div class="notes">
A couple things to note here:
- We have to define all our attributes that we're getting from the API.
- We're subclassing the DataStore classes now, instead of something from Ember.
- This is a relic of `ember-data` being a different project.
```

!SLIDE

`/views/entries_view.js`:

``` javascript
GuestBookApp.EntriesView = Ember.View.extend({});
```

<div class="notes">
Again, not a lot going on here because we're using the basic functionality and "magic"
</div>

!SLIDE

## Let's look at a slightly more interesting View class

!SLIDE

## Handling browser events in the View:

``` javascript
App.ClickableView = Ember.View.extend({
  click: function(event) {
    this.get('controller').send('turnItUp', 11);
  }
});
```

<div class="notes">
Here we're sending the message of turnItUp to our controller with 11 as the data. In this way, we can wire up DOM controls to notify changes in the application state.
</div>

!SLIDE

Our template for the application:

`/templates/application.handlebars`:

``` html
<div id="container">
  {{outlet}}
</div>
```

<div class="notes">
That's it!
The outlet is where our app knows to create its views.
</div>

!SLIDE

`/templates/entries.handlebars`:

``` html
<h2>Guests</h2>
<ul>
{{#each controller}}
  <li>
  <span><a href="mailto:{{email}}">{{name}}</a></span>
  <p>{{message}}</p>
  </li>
{{/each}}
</ul>
```

<div class="notes">
this might be a little confusing.
Why are we looping over Controller?
Well, remember our EntriesController is an ArrayController, and it's gonna expose that list of entries to us.
</div>

!SLIDE

## Thanks! Questions?

### I am:

[twitter.com/mathiasx](https://twitter.com/mathiasx)

[github.com/mathias](https://github.com/mathias)

This presentation lives at:

[blog.mattgauger.com/madrailers-ember-rails/](http://blog.mattgauger.com/madrailers-ember-rails/)

My example raffle app lives at:

[github.com/mathias/snow_mobile_raffle](https://github.com/mathias/snow_mobile_raffle)
