# Events and Callbacks

## Learning Objectives

### Events

* Explain event-driven development
* Understand the different types of events we can work with in JS
* Setup an event listener and an event handler
* Use the event object

### Callbacks

* Explain the concept of a "callback" and how we can pass functions as arguments to other functions
* Explain why callbacks are important to asynchronous program flow
* Pass a named function as a callback to **another** function
* Pass an anonymous function as a callback to another function

## Framing

So far, we have needed to use the REPL in the browser console to interact with our programs. This is asking a bit much of our users. Instead we would like to write code to respond to user interactions with the webpage.

The **DOM** not only lets us manipulate the document (or webpage) using JavaScript, but also gives us the ability to write JavaScript that responds to interactions with the page. These interactions are communicated as **events**.

We can **listen** for certain kinds of user-driven events, such as clicking a button, entering data into a form, keypresses and many, many more.

## Events

JavaScript typically will run top-to-bottom. As developers, however, we have no idea when the code related to the button-click will actually be executed. It's totally dependent on the user.

Therefore, we need to write code that will execute asynchronously — in other words, outside of the typical top-to-bottom document flow — and not hold up the rest of our application.

Once your JS has fully loaded, it lives in the background of your browser window, waiting and listening for any event triggers you've programmed.

As its name implies, in *event-driven programming*, the flow of a program is driven by events.

This means:

- The program continually "waits" or listens for events to occur.
- There are many kinds of events, such as mouse events, form events, key events, and document or window events.
- The event acts as a "trigger," which calls, or runs, a function.

### Turn & Talk: What is an Event?

But first, a question for you: What is an event (on a webpage)? Spend two minutes doing the following tasks. You are encouraged to discuss your findings with a partner during the exercise.

1. Come up with your own definition without looking at any other sources. Don't worry about getting it right — what do you think an event is?
2. Now, find (i.e., Google) some documentation on JavaScript events. Does that information match your definition? How would you change it?
3. Write down three examples of an event.

> You can find information on events and examples at [Mozilla Developer Network](https://developer.mozilla.org/en-US/docs/Web/Events).

### Types of Events

There are many events that can trigger a function. Here are a few:

|  Event | Description  |
|---|---|
|  `'load'` | When the page has finished loading.  |
|  `'resize'` |  When the browser window has been resized. |
|  `'scroll'` |  When the user scrolls up or down on a page. |
| `'click'`  | When the button (usually a mouse button) is pressed and released on a single element.  |
| `'mouseenter`' | When the user's mouse enters an element. |
| `'mouseleave'` | When the user's mouse leaves an element. |
| `'keydown'`  | When the user first presses a key on the keyboard.  |
|  `'keyup'` | When the user releases a key on the keyboard.  |
| `'focus'`  | When an element receives focus.  |
| `'blur'`  |  When an element loses focus. |
|  `'submit'` | When the user submits a form.  |

---
Now that we have an idea of what DOM events are in theory, let's wire up our code and begin interacting with them. There are two steps to working with events:
 
1. We set up an event listener with `.addEventListener`
2. We define an event handler, a callback function that get's passed to `.addEventListener`

### Step 1: Setting Up An Event Listener

In order to listen for an event, we need to define an **event listener**. Below you'll find a simple event listener associated with a `'click'` event on a `button` element.

First we target the button:

<details>
	<summary>If a button element has a class of 'js-button', how would we capture this from the DOM?</summary>

```js
const button = document.querySelector('.js-button');
```
</details>


Once we have the element from the DOM, we can tell JS to listen for an event:

```js
// This is the event listener

button.addEventListener('click', handleClickEvent);

// first argument: event
// second argument: callback function
```

### Step 2: Setting Up An Event Handler

For step two, we need to define the function that will be called whenever this event is emitted. This is just a function, but it has a special name due to how it's being used -- a **callback** function:

```js
function handleClickEvent() {
  console.log('I was clicked!');
}
```

Altogether, our code looks like this:

```js
const button = document.querySelector('.js-button');
button.addEventListener('click', handleClickEvent);

function handleClickEvent() {
  console.log('I was clicked!')
}
```

Or we could use an anonymous callback function:

```js
const button = document.querySelector('.js-button');

button.addEventListener('click', function() {
  console.log('I was clicked!');
});
```

<details>
	<summary>What is the code doing?</summary>
	
- The `button` refers to the DOM node to which we want to tie the event
- Then, it attaches an event listener to the `button` element with the `addEventListener()` method
- The `addEventListener()` method takes two arguments: 
 1. The event we want to listen for and 
 2. The function that should be called whenever that event is invoked. 
- In the case of the code above, we're saying we want to listen for `click` events on our `button`, and whenever someone does click on our button, call the `handleClickEvent()` function.
</details>

### Independent Practice - Color Scheme Switcher

It's time to get some practice in creating event handlers.

1. Open the [starter_code > color_scheme_switcher]() folder in your text editor.

2. Turn and Talk: Take a look at the code that has been provided to you in your main.js and style.css. What do you think will happen when the functions turnRed, turnWhite, turnBlue, and turnYellow run?

3. Add event handlers to the main.js file so that when a user clicks on one of the colored dots, the background color of the entire page changes to match that dot. You should not need to change any HTML or CSS.

### Code Along Exercise -- Simple Form

Say we've created a simple form that allows users to subscribe to our email newsletter.

When the user tabs or clicks away from the email input field, we want to make sure the user has entered a value in the field.

Here we have a simple HTML snippet of an email form:

```html
<form>
	<h1>Email Form</h1>
	<input id="email" type="email" placeholder="Email Address">
	<button type="submit">Subscribe</button>
	<p id="message"></p>
</form>
```
The form contains an input field where the user can enter an email address, a button for submitting the form, and a paragraph with the id message that currently does not have any text inside of it.

Our stylesheet is also very basic.

Take a look at the class error, which will give a solid red border to any elements that have the error class.

```
.error {
    border: 2px solid #fa4542;
}
```
Now let's take a look at the event handler in our JS:

```js
// First in our JS, let's find the email input field.
var emailInputField = document.getElementById('email');

// Next up in our JS, let's add our event handler that will trigger the function when the user
// hits tab or clicks out of the email field (the 'blur' event).
emailInputField.addEventListener('blur', checkEmailInput);

// And finally in our JS, let's go ahead and set up that function we want to run when the blur event occurs, checkEmailInput:
function checkEmailInput () {

    // Check to see whether the user has entered a value to the email field.
    if (emailInputField.value.length === 0) {
        // If the email field is blank, display a message to the user.
        document.getElementById('message').innerText = 'Please enter an email address.'

        // Add an error class to the input field that will give it a red border.
        emailInputField.className = 'error';
    } else {
        //Otherwise, clear out the error message.
        document.getElementById('message').innerText = '';

        // Remove the error class from the input field
        emailInputField.className = '';
    }

}
```

Let's examine what the page looks like when the user hits the tab key or clicks away from the email field without entering any information.

![Error](assets/email_form.png)

The email input now has the `error` class, giving the input field a red border.

We've also added a message in the paragraph with the id `message`, alerting the user that they need to enter an email address.

### Independent Practice - Adding Event Handlers

It's time to get some practice in creating event listeners.

1. Open the [starter_code/event_listener_practice]() folder in your text editor. We've provided you with three files: index.html, style.css, and main.js.

2. Your job is to add event handlers to create the following functionality:

 - When the user hovers a mouse cursor over the `<div>`, the background of the page should turn blue.
 - When user's mouse cursor is no longer hovering over the `<div>`, the background of the page should turn white.
3. You have been provided with two functions — `changeBackgroundColorToBlue` and `changeBackgroundColorToWhite` — that can be used as callbacks. You do not need to change the content of these functions.


## This

As we saw earlier in this unit, the keyword `this` refers to the object that "owns" the function that the executed code runs within. It's important to remember that, when we have a method that is inside an object, this refers to the object that contains that method.

However, when a callback function is executed within the context of an event handler, it is the element (the DOM node) that owns the context.

So in this case, this will refer to the element that we selected when we set up our event handler.

Let's look at an example where we'll change the background color of a circle from blue to red, just by clicking on it:

##### HTML
```html
<div class="circle"></div>
```
##### JAVASCRIPT
```JS
document.querySelector('.circle').addEventListener('click', turnRed)

function turnRed () {
	this.style.backgroundColor = "red";
}
```
When we click on the circle and trigger the turnRed function, this will refer to the element with the class circle within the turnRed function.

Here's what that looks like in action:

![](http://circuits-assets.generalassemb.ly/prod/asset/4629/Slide-8.gif)

Okay, but why use the keyword `this`:

`this.style.backgroundColor = "red";`

Instead of just writing:

`document.querySelector('.circle').style.backgroundColor = "red";`

Well, let's imagine that there are several circles on our page, and we only want the `.circle` that we just clicked to have the updated red background color. That is where the `this` keyword really becomes useful.

Let's take a look:

```js
//Select all elements with the class .circle on the page
var circles = document.querySelectorAll('.circle');

//loop through each .circle element and add an event handler.
for (var i = 0; i < circles.length; i++) {
	circles[i].addEventListener('click', turnRed);
}

function turnRed () {
	this.style.backgroundColor = "red";
}
```

Here we are adding an event handler to each element with the class `.circle`.

When an element with the `.circle` class is clicked, the `turnRed` function will be called; within that `turnRed` function, `this` will only refer to the `.circle` that triggered the `turnRed` function, and not to any of the other circles.

Let's see this in action:

![](http://circuits-assets.generalassemb.ly/prod/asset/4630/Slide-11.gif)

See how we are only adding the style attribute to the circle we are currently clicking on (i.e., the one that triggered the callback function)? Pretty cool, huh?

## Independent Practice: This  
It's time to get some practice in creating using the `this` keyword.

Work through this exercise with a partner.

- Open the [starter_code/color\_scheme\_switcher\_part\_2](starter_code/color_scheme_switcher_part_2) folder in your text editor. We've provided you with three files: `index.html`, `style.css`, and `main.js`.
- Follow the instructions in the `main.js` file.
- You should only need to write code within the `switchTheme` function.

**Challenge Instructions (for advanced students)**

Want to try your hand at this exercise with a little less guidance?

- Open the [starter\_code > color\_scheme\_switcher](starter_code/color_scheme_switcher) folder in your text editor that you were working from earlier.
- Refactor the code using the following guidlines:
	- Use `querySelectorAll` to select all `li`s on the page.
	- Loop through all list items and add an event listener to each. When a `li` is clicked, call the `switchTheme` function.
	- Create the `switchTheme` function. When the function runs, use the `this` keyword to get the className on the button that was just clicked and update the className on the body to that class.

---

## The Event Object

Now that we've gotten the hang of writing event handlers, let's talk a bit about the event object.

When an event occurs, we might want to find out some information about it. For example, which element did the user interact with to cause the event? What type of event was it? A click event? A mouseover?

To obtain this information, we use the **event object**.

### Explore The Event Object

Open up your event listener practice exercise and modify your event handler to accept the event object as a parameter. Then log it to the console.

With your partner, spend three minutes clicking the button and exploring what properties the event (or `e`) object contains. Look for:

* A way to figure out what element was clicked on.
* A way to tell the position of the mouse when it was clicked.
* One other piece of useful or interesting information.

> The reason we're not actually using `event` is that it's a "reserved word" in Javascript, like "if" and "return".

Examine what the event object looks like when you log it to the console, and notice all of the properties we have available to us as part of the event object:

<img src="assets/Slide-27-Codeblock.svg" width="500px">


We'll take a look at a few of these properties, but for now, just note how much information about the event the event object holds.

### Preventing Default Behavior

The event object is useful to us as programmers for many reasons - one of those reasons is preventing the default browser behavior for a certain event.

A very common use case for this is when the user clicks on an anchor element (`<a>`) but we want to control what happens in our code, rather than rely on what the browser tries to do in response to this event by default (navigate to another location).

For example:

To prevent the default behavior, we have a special method inside the event object: `preventDefault()`.

- How do we call methods on objects?
- How might we then invoke `preventDefault()` to prevent the default browser behavior?

<details>
	<summary> Answer </summary>

```js
var button = document.querySelector('.js-button');

button.addEventListener('click', handleClickEvent);

function handleClickEvent(e) {
	e.preventDefault();
  console.log('I was clicked!');
  console.log(e);
}
```

</details>

### Event Propagation

Given the following scenario and what we've learned about events so far, what would you do?

> You have a `<nav>` element that contains an unordered list with links in the header of your website. This `<nav>` serves as the primary navigation. Whenever the user clicks on one of these links however, you want to run some code and update the page with JavaScript. How would you attach and event to each link in the list?

One way to go about this is to set an individual event listener on each link, as we did in the color switcher example above.

Another way to go about this is to take advantage of event propagation. There are three phases of **event propagation**: capture phase, target phase, and bubbling phase.

When an event (e.g. click) occurs, all nodes up the DOM tree are notified, beginning at the window level and working its way down the DOM branch to the event target. This is the capture phase. Once the propagation reaches the event target, all event listeners on the target will be triggered. Then, event propagation continues back up the DOM tree in the event bubbling phase. All three of these phases are very nearly instantaneous.

For a visual, consider the event propagation flow phase illustration below.

![Event Propagation Flow Chart (from World Wide Web Consortium)](./event-propagation-flow.png)

Going back to our example with the `<nav>` element containing links: Given event propagation, you can put an event listener on the `<nav>` element. When a click event occurs on each child `<a>`, the event will propagate up through all parent elements of that `<a>` element, including the `<nav>` element. Thus, the event listener on the `<nav>` element will be triggered.

To trigger specific outcomes for each event target child element, you can use a conditional (if/else) or switch statement to invoke the intended function for each the event target.

### target

We can use dot notation to access those properties, as we did when working with objects.

```js
document.querySelector('a').addEventListener('click', viewComments);

function viewComments (e) {
	// To access a property of the event object, we can use dot notation:
	var eventTarget = e.target;
	// Log the target to the console
	console.log(eventTarget);

}
```

Here we access the target of the event by using dot notation e.target. Then we log the target to the console.

Let's take a look at what we see in the console:

```html
<a href="#">View Comments</a>
```

Aha! That's the target of the event, or the element we clicked on that caused the event to fire.

## Additional Topics

### Event Flow (If time permits)

### Event Bubbling

### Key Events (If time permits)

### Multiple Events (If time permits)

---

### We do: Color Switcher

Let's write use javascript event listeners to finish the color switcher code and get the buttons working.

## You Do: Traffic Light and DOTS: The Game

1. In the traffic light directory, implement the JavaScript required to get the traffic light to illuminate the correct light when the corresponding button is pressed. **Note**: Only one light should be on at a time!

2. Visit this [repository](https://git.generalassemb.ly/WDI-Epiphany/event-listener-demo) and follow the instructions.

## Additional Reading
- [Build a Drum Kit in Vanilla JS](https://www.youtube.com/watch?v=VuN8qwZoego)
- [Event reference](https://developer.mozilla.org/en-US/docs/Web/Events)
- [Introduction to events](https://developer.mozilla.org/en-US/docs/Learn/JavaScript/Building_blocks/Events)
- [Eloquent JavaScript: Handling Events](http://eloquentjavascript.net/14_event.html)
- [Philip Roberts: What the heck is the event loop anyway?](https://www.youtube.com/watch?v=8aGhZQkoFbQ)
- Readings
 - Eloquent JavaScript [Events](http://eloquentjavascript.net/15_event.html)