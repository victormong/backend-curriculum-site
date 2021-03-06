---
layout: page
title: jQuery DOM Traversal, Manipulation, and Events
length: 180
tags: javascript, jquery
status: draft
---

## Learning Goals

* Use jQuery selectors to find content
* Understand that jQuery collections allow you to manipulate multiple elements with a single method
* Use jQuery's DOM traversal methods to move around the DOM
* Add CSS styles using jQuery
* Append new content to the DOM
* Add event listeners to elements currently in the DOM
* Understand that adding an event listener will not effect elements you add to the DOM in the future

## Getting Up to Speed with JavaScript

### Syntax, Variables and Functions

#### Syntax

All declarations in JavaScript must end with a semicolon (`;`).

Camel case is prefered over snake case.

Comments are made with `//`.

#### Variables

JS variables MUST be declared using `var`.

```js
var isSnowing = true;
```

Redefining variables does not require the `var` keyword.

Variables can be _declared_ without being _defined_.

```js
var yetToBeDefined;
yetToBeDefined;
// => undefined
yetToBeDefined = true;
yetToBeDefined;
// => true
```

#### Functions

Functions will follow the following general structure:

```js
var doSomething = function(thing) {
  return thing;
};
```

Functions are called with its variable name and parens. Without parens, the function will not execute.

```js
doSomething("test");
// => "test"
```

Return values in JavaScript MUST be explicitly stated with the `return` keyword, otherwise they will return undefined.

### Debugging in Javascript

Debugging JavaScript is a different beast than debugging Ruby. Because JS is run entirely in the browser, the technique for troubleshooting broken code is more complicated than `binding.pry`. Luckily, modern browsers are aware of this and give us a collection of options for digging into your code.

#### 1. Developer Tools
One of the first things you should familiarize yourself with when working with JavaScript (or HTML...or CSS...) are the dev tools. You can find a cool tutorial to dive deeper with  [Code School's Discover-DevTools Tutorial.](http://discover-devtools.codeschool.com/) (Chapters 3 & 4 are particularly helpful)

To open developer tools in Chrome:
-   Mac: `Cmd` + `Opt` + `i` (or `Cmd` + `Opt` + `j`)
-   (or) Right click on the browser window and select `inspect`
-   (or) Select `View` in the navbar, then `Developer`, then `Developer Tools`

When working with JavaScript, it is useful to keep your console open at all times to watch for errors and anything you've told your code to print out. Bringing us to...

#### 2. console.log()
`console.log()` is to JS what `puts` is to Ruby. This line of code will print whatever is provided as an argument to the console.

Given the following function called `printStuff()`, adding console.log() will print the value of `myVariable` to the console.

```
var printStuff = function(){
  var myVariable = 5 + 5
  console.log(myVariable);
}

printStuff()
=> 10
```

If you're confused about what a variable or function is returning, throw `console.log()` into your code or directly into the `console` in your browser to confirm/deny suspicions.

#### 3. Debugging In the Console

Debugger is the `pry` of JS. Stick `debugger;` within a function to pause the browser from running the script when it hits a particular part of your code.

```
// index.js
$('#search-ideas').on('keyup', function() {
  var currentInput = this.value.toLowerCase();

  $ideas.each(function (index, idea) {
    var $idea = $(idea);
    var $ideaContent = $idea.find('.content').text().toLowerCase();
    debugger;
    if ($ideacontent.indexOf(currentInput) >= 0) {
      $idea.show();
    } else {
      $idea.hide();
    }
  });
```

In the browser, if we open up the dev tools, navigate to the console and try to search for something.  The program will freeze on the line `debugger`. This lets us type stuff into our `console` to see what's going on.

For more details and information about other ways to dig into your js, check out the [Chrome Documentation](https://developer.chrome.com/devtools/docs/javascript-debugging).

## Intro to the DOM

DOM stands for Document Object Model. The browser uses it to represent everything on the page. It's an "object model" because it is made of objects. Each element is an object. If you wanted to, you could directly translate the DOM to a JavaScript object.

The DOM is hierarchical. If you have a tag wrapping another tag, then the inner object is a child of the outer object, which is the parent.

```html
<ol>                <!-- parent -->
  <li>First</li>    <!-- child  -->
  <li>Second</li>   <!-- child  -->
</ol>

```

The browser creates the DOM by reading from HTML, but from then on, JavaScript controls any changes to the DOM.

```
HTML --> DOM <--> JS
```

## Lecture, Part One: Selectors

### Basic Selectors

Out of the box, jQuery supports the selector syntax from CSS to find elements on the page. So, you already come pre-equipped with a bunch of knowledge for finding elements.

That said, let's review some of the different ways we can find an element on page.

* `$('p')`, selects all of a given element.
* `$('#heading')`, selects the element with a given id.
* `$('.important')`, selects all of the elements with a given class.

You can also use multiple selectors in the same statement using commas:

* `$('p, #heading, .important')`, selects everything listed above.

### Chaining Selectors

There are a few different ways to chain selectors to use them together. You can seperate these selectors with a comma, a space, or nothing at all.

* Comma: `$('p, #heading, .important')` just combines all of the selectors together.
* Space: `$('p #heading .important')` treats each selector as a child of the previous. This will give you items of the class `important` that are children of the id `heading` which are inside a `<p>` tag.
* Nothing: * `$('p#heading.important')` matches elements that have all three selectors. This selector would select a paragraph which was defined like this: `<p id="heading" class="important">`

### Attribute Selectors

See the API documentation [here](http://api.jquery.com/category/selectors/attribute-selectors/).

### Laboratory

[Here is an little experiment][exp] where you can play around and try out some different selectors.

Play around with this on your own for a bit.

[exp]: http://codylindley.com/jqueryselectors/

## Exercise, Part One: The Presidents

For this exercise, we're going to play with [a table of the Presidents of the United States of America](https://gist.github.com/neight-allen/a49b8caa02d127a3a3fcf409eea3ed14).

```
git clone https://gist.github.com/a49b8caa02d127a3a3fcf409eea3ed14.git jquery_lesson
```

Let's try out a few things, just to get our hands dirty. We'll use the console in the Chrome developer tools to validate our work.

* Select each `tr` element.
* Select all of the elements with the class of `name`.
* Select all of the elements with either the class of `name` or `term`.
* Select all of the checked—umm—checkboxes. (You'll probably want to check some checkboxes first.)
* Select all of the `td` elements with the class of `number` that appear in a row of a `tr` with the class of `whig`.

(This should take about five minutes total.)

## Lecture, Part Two: Manipulating CSS

Once we have an element in our sites, we probably want to do something with it, right?

In this case, let's add some CSS styling. Let's say we wanted to grab all of the Federalist presidents and turn their font color pink. We could do something like this:

```js
$('.federalist').css('color', 'pink');
```

One thing you might have noticed about CSS is that it really likes hyphens. So, to change a background color, you would use `background-color`. The thing about hyphens is that they are a no-no in JavaScript. So, we *should* to camel-case our property names in our CSS methods.

```js
$('.federalist').css('backgroundColor', 'pink');
```

You'll notice I said "should" instead of must. At the end of the day that's just a string. You can do it the other way, but it's against convention.

```js
$('.federalist').css('background-color', 'pink');
```

Right now, we're setting individual properties. We can also pass in a conditional object in order to change multiple CSS attributes all at once.

```js
$('.federalist').css({
  backgroundColor: 'pink',
  fontWeight: 'bold'
});
```

If you ignored me earlier and insisted on using hyphens, you're going to have to wrap those property names in quotes now. Yuck.

Writing CSS by hand is probably a bad idea. We're better off using classes to style our content.

We can add and remove classes pretty easily in jQuery.

```js
$('.federalist').addClass('red');
$('.federalist').removeClass('red');
```

Keeping track of state is hard. jQuery is here to help.

```js
$('.federalist').hasClass('federalist'); // Returns true, obviously.
```

The other option is to use `toggleClass`, which will either add or remove the class depending on whether or not the class currently exists.

```js
$('.federalist').toggleClass('red');
```

## Exercise, Part Two: Style the Presidents

* Add the class of `red` to all of the Republicans.
* Add the class of `blue` to all of the Democrats.
* Add the class of `yellow` to the term column of the table.
* Take all the whig presidents and give them a purple background and white text.

## Lecture, Part Three: Filtering and Traversal

Let's talk about a few [DOM traversal methods](http://api.jquery.com/category/traversing/tree-traversal/).

Here are some of the all-stars of the DOM traversing world:

* `find()`
* `parent()`
* `parents()`
* `children()`
* `siblings()`

## Exercise, Part Three: One-Term Presidents

* Add the `green` class to anyone who served right after a president who died.
* Find all of the presidents who only served one term and add the class `red`.
* Add the class of `blue` to the parent of a checked checkbox.
* Add the class of `yellow` to the siblings of the parent (`td`, in this case) of an unchecked checkbox.

## Lecture, Part Four: Adding to the DOM

Let's take a look at some approaches of adding/changing content in the DOM.

* `.text()`
* `.html()`
* `.append()`
* `.prepend()`

## Exercise, Part Four: Dead Presidents

* Find all of the presidents who died in office (hint: they have a `died` class on their `tr`).
* Append `<span class="died">(Died)<span>` to the the `term` column of presidents who have `.died`.

## Lecture, Part Five: Simple Event Binding

### Event Driven Programming

Event driven programming relies on some external action to determine how the program behaves. Some external actor (a user or another computer) takes an action, and your program responds.

You've done event driven programming before. Can you think of projects that use this pattern?

### Event binding using jQuery

Let's start by looking at the [jQuery Events API](http://api.jquery.com/category/events/).

The Events API tends to mimic the native DOM events, but with some abstraction to standardize across all of the browsers in use today.

Our main focus today is going to be on the `.on()` method. As of jQuery 1.7 and later, this is the preferred method for binding events. You may see `.bind()` as well, but this support older code.

Talking points:

* Talk about `this` in an event handler.
* Bind a `console.log` to a checkbox.
* Inline handlers vs named functions

## Exercise, Part Five

As pairs, try to work through the following prompts. We'll do the first one together.

* Add an event handler to all of the checkboxes that when the box is checked, adds the `yellow` class to the parent `tr`.
* Add an event handler that adds the `red` class to a `td` element when it's clicked on.
* Modify the event handler above to remove the `red` class when it is clicked a second time.

## Form Challenge

Let's clone down [this simple form](https://github.com/turingschool-examples/jquery-form-challenge).

```bash
git clone git@github.com:turingschool-examples/jquery-form-challenge.git
cd jquery-form-challenge
open index.html
```

Right now, it doesn't work and needs to be wired up.

1.  First, how could we select all `input`s?
2.  How could we use jQuery to fill in the value for the `.link-title`?
3.  How could we use jQuery to fill in the value for the `.link-url`?
4.  How could we click the submit button from our console?

Let's hop out of the console and actually edit the JS now.

5.  Capture the `click` event for the submit button
6.  On submit, let's get the value of the inputs
7.  Now let's append those values under `My Links`
