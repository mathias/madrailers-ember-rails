# ember-rails hands-on intro

!SLIDE

# ember-rails hands-on intro

## Matt Gauger

### Mad Railers 2013-05-20

!SLIDE

## I work at Bendyworks

![Bendyworks logo](images/bendy.png)

!SLIDE

## What is Ember.js?

A framework for JS MVC apps

<div class="notes">
Or, another way of architecting these fancy websites we keep trying to build
</div>

!SLIDE

## MVC => Model, View, Controller

!SLIDE

## MVC => Model, View, Controller

(Let's not get in a debate as to whether it's really MVC when we say JS MVC, ok?)

!SLIDE

## Ember.js is responsible for:

## Controlling the entire viewport
#### (versus other frameworks which do not)

!SLIDE

## Ember.js is responsible for:

## Getting data from our backend server
#### In this case, Rails

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

## Thanks!

### [twitter.com/mathiasx](https://twitter.com/mathiasx)
### [github.com/mathias](https://github.com/mathias)
### Presentation: [blog.mattgauger.com/madrailers-ember-rails/](http://blog.mattgauger.com/madrailers-ember-rails/)

!NOTE

``` ruby
def method
  puts "Hello, World"
  more code
  more                     code


  more
end
```
