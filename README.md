# Angular-Directive-Project

Directives range from very basic to extremely complex. This project will build up to some somewhat difficult directives. Keep in mind that the format we're learning for directives is the same format used to build some extremely complex things in angular. Using directives often and well is one way to show you're a talented developer.

## Starting Out
We've included only a few things for you to begin with. `index.html`, `app.js`, `styles.css`. At this point the best way to get more comfortable with angular is to initialize an app without relying heavily on boilerplate code (reusable code that starts out your projects for you).

You'll notice that in the `index.html` we've included the angular-route CDN. Yes, we'll be using angular's router here. Put an `ng-view` into your `index.html`.
- In your `app.js` set up a `config` and set up our first route for when a user is at the `'/home'` url. *If you're having trouble remembering how to set up the router go look at how you set up the router on the previous project. One way these projects will be beneficial to you is allowing you to look back at something **you** did and seeing how **you** got that something to work.*
- You may also want add an otherwise that defaults to `/home`.
- Create a controller and a template file for this route in your `app` folder. *Don't forget to include the controller as a script in your index.html*
- Check that everything is hooked up correctly. Try adding a div with some text in your home template just to make sure it's showing up. Once you've got that going you're ready to start on some directives.

Now let's make our directive. We'll start with a simple one that we can use to display information passed to it.

## Step 1. Start your directive
Woot. When you're initializing your directive just remember that it works very similarly to how you start up a controller or a service.
It can also be very helpful to think of your directive as a route.
- Create your directive. You'll use the directive method on your angular module. It takes two arguments, the name `string` and the callback function, which will return the object that represents your directive.
- When naming your directive give it a name with two words; `dirDisplay` would be nice, but anything works. *Just remember it's best practice to give a directive a camel case name so that it's clear in your html what it is.*
- Also we're going to need a template html for our directive. We could do it inline, but let's make another file instead. Just name it something that makes sense for the name of your directive and put it in the same directory as your directive file.
- For your template just make a `<div>` and inside a `<h1>` tag that says `User`.
- Now in your `home` route html add in your directive. It will look like this if you named it `dirDisplay`: `<dir-display></dir-display>`
- Start up your app and go to the home route. Check and make sure you see `User` where your directive was placed.
- If you're not seeing it at this point it could mean a few things. Here's some more common issues.
 - You didn't link your directive in your index as a script.
 - Your name for your directive doesn't match the name in your html. *Remember camel case becomes snake case so `myDirective` becomes `<my-directive></my-directive>`.*
 - You're file path to your html template is wrong. *You have to think of file paths in angular as relative to the index.*
 
Here's some code to see just for this part, and just for the directive's `js` file.
```javascript
var app = angular.module('directivePractice');
app.directive('dirDisplay', function(){
  return {
    templateUrl: 'app/directives/dirDisplay.html'
  };
});
```

What we're returning is the directive object. You won't see anymore code in this tutorial so it's important you get things working right and refer back to what you've already done to advance from now on.

## Step 2. Advancing directives
Your directive should be loaded up now, but it's not really doing much. Let's make it better.
- In your home controller. Make a variable on your `$scope` called user. Set it's value to 
```javascript
{
  name: "Geoff McMammy",
  age: 43,
  email: "geofdude@gmail.com"
}
```
- Now inside your directive's html specifically inside the `<h3>` tags display our new user's name.
- Then inside maybe some `<h4>` tags display his email and age.
*This is going to work exactly the same as if it was just inside your `home` controller.*
- Reload the page and make sure it works.

This is still very cosmetic and really not all that useful. It needs functionality.
- Add into your directive's object the `link` property.
- The link property's value is a function definition that takes (generally) three parameters. `scope`, `element`, and `attributes`.

Unlike in other places with angular injection these parameter names don't carry meaning. The first parameter will always represent your `$scope` for that directive, the second 
will always be the element that wraps your whole directive, and the third will always be an object containing all the properties and values of the attributes on your directive in the dom.
Try the following to get a feel for all three.
- Add two attributes to your directive in your html. Like this - `<dir-display test="myTest" my-check="checkItOut"></dir-display>`
- Now in the link property you've added `console.log` the three parameters in the function.

You'll see an object for `scope` that should look identical to the `$scope` of your html function.
For `element` you'll see an object the represents the DOM wrapper for your directive.
For `attributes` you'll see an object that will look like this:
```javascript
{
  test: "myTest",
  myCheck: "checkItOut"
}
```
An important thing to notice is how it has again converted snake case to camel case for you. `my-check` became `myCheck`. Don't forget this. You'll run into this issue one day. It counts for 
both attributes and directive names.

To feel some of what the link function could do let's try this.
- Add a `ng-show` to both the email and age wrappers. This should be familiar to you.
- Now inside your link function add a click event listener to your element property. It's going to look just like jQuery.
```javascript
element.on('click', function(){
  
})
```

- Inside the click listener's callback add a toggle for the `ng-show` property you passed in. Along with a `console.log` to make sure things are connecting when you click.
- Try it out. Don't call for a mentor when it doesn't work. Let's talk about that first.

You should see the `console.log` firing, but why isn't it toggling. This is going to be a common problem when working with the link function and event listeners.
What we have here is an angular digest problem. The value is changing on the `scope` object, but the change isn't being reflected by our DOM. That's because angular isn't aware of the change yet. Anytime we cause an event to happen
using something like jQuery or even angular's jQLite we need to let angular know that we've made a change.

- Add this line of code in place of your `console.log`, `scope.$apply()`.
- Now try it out. It should be working now, so if you're still having issues it's time to debug.

What we've done is forced angular to run it's digest cycle. This is where angular checks the `scope` object for changes and then applies those to the DOM. This is another good lesson to learn for later. You'll most likely hit this when making changes 
to your element using event listeners.

## Step 3. Directive's re-usability.
Now our directive has some extremely basic functionality. One of a directive's greatest advantages though is its ability to be placed anywhere and still be functional. Let's say instead we had a list of users instead of just one.

- Change the `$scope` property in your home controller to be users and give it this array as its value:
```javascript
[
 {
   name: "Geoff McMammy",
   age: 43,
   email: "geofdude@gmail.com",
   city: "Provo"
 },
 {
   name: "Frederick Deeder",
   age: 26,
   email: "fredeed@gmail.com",
   city: "Austin"
 },
 {
   name: "Spencer Rentz",
   age: 35,
   email: "spencerrentz@gmail.com",
   city: "Sacramento"
 },
 {
   name: "Geddup Ngo",
   age: 43,
   email: "geddupngo@gmail.com",
   city: "Orlando"
 },
 {
   name: "Donst Opbie Leevin",
   age: 67,
   email: "gernee@gmail.com",
   city: "Phoenix"
 }
]
```
- Now in your `home` HTML add a `ng-repeat` to the directive call. Tell it to repeat for each `user` in `users`.
- Reload your page.

It's working! But why? How does each directive instance know what information to display?
- In the link function `console.log` the `scope` parameter. *Make sure it's outside of your click listener.*
- You'll see five print outs in your console. Open up any one of them and look to the bottom. Open up the user property.

It's exactly what we would want! But again why would that be the case? Don't get too caught up in this next bit if it's too hard to understand, but the ng-repeat is essentially making new tiny scope objects for each individual `user` in our users array.
Now each of our directives is still getting a `user` property on the scope object just like the directive wanted in the beginning. Woot.

## Step 4. Ramp it up with Isolate Scope.
Directives can do so much more. So let's make that happen. That means we should make.... a new directive!!! This directive's purpose will be to display a selected User and the weather in his/her/its location.
Link it up just like the last one.
- Create a `js` file for our directive and name it `dirWeather`. Make an `html` file named `dirWeather.html`. Link it up in your index.html and add the template to your new directive object.
- In your directive's template give it an `<h3>` tag that says `Weather` just so we can know it's working.
- Above your `ng-repeat` on `dirDisplay` add your new `dirWeather` directive.
 - If it's not working check the instructions above as to some common reasons why before asking a mentor for help.
- If you're seeing the `Weather` text on your page then we're ready to try out the dreaded **Isolate Scope**.

The isolate scope object is one of the stranger API's in angular. I'm sorry but it is. Just refer to this for now.
```javascript
scope: {
 string: '@',
 link: '=',
 func: '&'
}
```

The properties on the scope object represent the attributes on the directive in the html. Our example scope object here would look something like this in the html.
`<example-directive string="a string" link="user" func="updateUser()"></example-directive>`
The hard part here is the `@`, `=`, and `&`. They each have very important and distinct meanings.
- `@` says take in my attribute value as a string.
- `=` says take in my attribute value as a two-way bound variable from the parent scope.
- `&` says take in my attribute value as a reference to a function on the parent scope.

It's also critical to point out that once you add a `scope` object you have no isolated your directive's scope. Meaning, aside from the values passed in through attributes, this directive has no connection to the `$scope` of its parent.

That being said let's isolate our directive's scope. :worried:
- Add the scope property to your `dirWeather`.
- Give it the value of an object with a property of `currentUser` whose value is `'='`. *Remember in your html this will look like `current-user`. This is the third time I've said so don't expect it again.*

This means that whatever comes into the `currentUser` attribute is going to be a value of the parent's scope object.
- For now test this out by passing in `users[0]`. Find a way to show that users information inside your `dirWeather`'s html. *Remember inside your directive now the user is represented by `currentUser`.*

## Step 5. &? &!?

The `'='` value on your `scope` object has created a two-way binding between `users[0]` and `currentUser`. Now let's try out the `'&'`.
- On your `home` controller add a function called `getWeather`. It takes one parameter called `city`.
- This function will make a call to a service so we'll need to create that.
- Make a weather service. Name it something cool and creative like `weatherService`.
- Inside the weather service make a function called `getWeather` that also takes one parameter, `city`.
- Make an `$http` get to this url - `'http://api.openweathermap.org/data/2.5/weather?q='`
- After the `q=` add on the `city` parameter.
  - If you want you can test this out in postman. See what kind of data you get back. If it's the weather of that city then... you win!
- Use `$q` to return a promise that only resolves with the data you want. Temperature (preferably not in Kelvin) and the weather description. Use `console.log` on the data coming from the `$http` request to get to what you want. You'll need to add both on an object that you resolve your new promise with.
- On your `home` controller have it return the result of invoking the get `getWeather` function on the service. *You should be returning a promise.*
- Now in your `home` route's HTML pass in the getWeather function to the `dirWeather` directive through an attribute called `weather-call`.
- Add the attribute to your isolate scope object.

That was a lot of linking, but let's walk through it. Your controller has a function linked to the service, which is in turn linked to your directive. So if you run the `weatherCall` function in your directive it will go through your controller to your service and then back.

So let's put this new function to use and along with it learn a new directive property.
- Add to the `dirWeather` directive object a property called `controller`.
 - Don't forget to inject `$scope`.

Yes, this is a controller specifically for your one directive. It works the same as any other controller, except you don't give it a name. It's `$scope` object will only be accessible within an instance of your directive.

- Inside your controller function run the `weatherCall` function with the `city` property from the `currentUser` on your `$scope`.
- This should return a promise, so call `.then` afterward and add the data onto your `$scope` to display both the weather and temperature of the `currentUser`'s city.
 - You may want to find a way to get rid of all the decimal places on your temperature.