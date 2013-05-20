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

### strings that we will render with data to present the user interface
### (Not in the acroynm)

<div class="notes">
In Rails, this is what we call the View.. notice how we never write a View class in Rails? You could, but you don't have to.
</div>

!SLIDE

## View

### Translates browser events to application events
### Binds data to a template

!SLIDE

## Controller

### Stores representations of application state
#### ie, a collection of model instances that came back from our API

!SLIDE

## Model

### Stores persistant state
#### Typically loaded and saved to a server, but doesn't have to be


!SLIDE

## Router

### Manages application state
#### Most importantly, handles URLs and sends messages the correct Controller for what is being requested

!SLIDE

## How does Ember.js compare to other frameworks?

!SLIDE

## Favors convention-over-configuration

### Like Rails!

!SLIDE

## Ember.js is responsible for:

### Controlling the entire viewport
#### (versus other frameworks which do not)

!SLIDE

## Ember.js is responsible for:

### Getting data from our backend server
#### In this case, Rails

<div class="notes">
But you can use other backends that conform to the ember-data API.
Notably, Firebase is available as an ember-data backend.
</div>

!SLIDE

## Ember.js is responsible for:
## Rendering templates with the data

!SLIDE

## Ember.js is responsible for:
## Handling interaction with the user

!SLIDE

## Ember.js is responsible for:
## Saving changed data back to the backend

!SLIDE

## What does a simple Ember.js application look like?

``` javascript

App = Ember.Application.create();

App.Person = Ember.Object.extend({
  firstName: null,
  lastName: null,

  fullName: function() {
    return this.get('firstName') +
           " " + this.get('lastName');
  }.property('firstName', 'lastName')
});

App.IndexRoute = Ember.Route.extend({
  model: function() {
    var people = [
      App.Person.create({
        firstName: "Matt",
        lastName: "Gauger"
      })
    ];
    return people;
  }
});
```

!SLIDE

## Thanks! Questions?

### I am:
### [twitter.com/mathiasx](https://twitter.com/mathiasx)
### [github.com/mathias](https://github.com/mathias)

### This presentation lives at:
### [blog.mattgauger.com/madrailers-ember-rails/](http://blog.mattgauger.com/madrailers-ember-rails/)
### Outline (with notes):[gist.github.com???](#)


!NOTE

``` ruby
def method
  puts "Hello, World"
  more code
  more                     code


  more
end
```
