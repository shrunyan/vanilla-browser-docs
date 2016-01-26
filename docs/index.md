# Vanilla JavaScript in the Browser Workshop

## Overview

Welcome to Vanilla JavaScript in the Browser workshop hosted by [San Diego JS][san diego js].

The goal of this workshop is to learn how to effectively use vanilla JavaScript in the browser.

Modern web development workflows often rely on libraries like jQuery. But using libraries can add a lot of unneeded bloat to your project. This workshop will help you dig a little deeper into JavaScript and how to use the available browser APIs without using the $.

This workshop will feature:

* DOM API
* Browser Event API
* Using Cookies
* XHR Requests
* Dynamic HTML
* Testing with Mocha

### The example

For this workshop, we are going to build out a business card creator. There is a simple html form inside of `public/index.html` that we will build upon and add life to. We'll collect common profile information that one would normally share professionally, like contact info and skills, and then display it.

### Keep your resources handy

Workshops are fun and can be a fast way to learn while building something potentially reusable. There will come a time, however, when you'll want to reference the API and see if there are other methods or functionality that you didn't know existed.

A really good open-source resource that also provides great offline functionality for you future digital nomads is [DevDocs.io][devdocs]. You can select the different APIs you want to be immediately searchable and they cover everything from the browser, to node, to rails, to elixir, and even more you never even knew about!

For this workshop, it may be useful to reference the sections that will align with this workshop:

* [DOM API][dom]
* [Browser Event API][events]
* [Using Cookies][cookies]
* [XHR Requests][xhr]
* [Testing with Mocha][mocha]

Another good resource for learning about the available APIs and even the internals of how a browser parses, executes, and draws a page, is [Mozilla Developer Network][mdn].

**Have any other great resources you've used? We would love to hear about them and share them with the other attendees!**

## Pre-event Setup Instructions

Prior to your arrival the following should be installed on your system:

0. [Install Git][git-scm]
0. [Install Node.js 4.2 LTS][node-install]
0. Setup NPM for non-sudo installation
    0. NPM is the node package manager.  It will automatically be installed when you install node.
    0. NPM installs packages *locally* (within the directory it is invoked in) for per-project modules, or *globally* for packages you want accessible everywhere.
    0. However, by default NPM installs global packages in a root-restricted location, requiring SUDO to install.  This creates a **huge** headache.  As an alternative, _before_ you install any packages, follow [this guide][npm-g-without-sudo] to configure your NPM to install in your home directory without requiring sudo.
0. Clone this repository `git clone git@github.com:sandiegojs/vanilla-browser-workshop.git`
0. Change directories into the workshop folder: `cd vanilla-browser-workshop` and install your local dependencies with: `npm install`
0. Install these global dependencies using the `-g` flag (ex `npm install <package> -g`)
    * gulp
    * mocha
0. If you plan to deploy your app onto Heroku, setup [Heroku Toolbelt][heroku-toolbelt]

## API service

The app we will be building is a client-side application, meaning it will be running in a user's browser. We will need to persist the data that we are
creating to a backend service and database so that we can recall it later.

We could use fixture data or a mock service for this, but some friendly backend
developers have already made a working API for us, so let's use that.

Our API is setup at [//sandiegojs-vanilla-workshop.herokuapp.com][sdjs-app] and supports the following endpoints


<table class="table table-bordered table-striped">
  <colgroup>
    <col class="col-xs-1">
    <col class="col-xs-3">
    <col class="col-xs-5">
  </colgroup>
  <thead>
    <tr>
      <th>Verb</th><th>path</th><th>Description</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>GET</td><td>/forms</td><td>List of forms</td>
    </tr>
    <tr>
      <td>POST</td><td>/forms</td><td>Create a new form</td>
    </tr>
    <tr>
      <td>GET</td><td>/forms/:id</td><td>Retrieve a form</td>
    </tr>
    <tr>
      <td>PUT</td><td>/forms/:id</td><td>Update a form</td>
    </tr>
    <tr>
      <td>DELETE</td><td>/forms/:id</td><td>Delete a form</td>
    </tr>
    <tr>
      <td>GET</td><td>/skills</td><td>List of skills</td>
    </tr>
    <tr>
      <td>POST</td><td>/skills</td><td>Create a new skill</td>
    </tr>
    <tr>
      <td>GET</td><td>/skills/:id</td><td>Retrieve a skill</td>
    </tr>
    <tr>
      <td>PUT</td><td>/skills/:id</td><td>Update a skill</td>
    </tr>
    <tr>
      <td>DELETE</td><td>/skills/:id</td><td>Delete a skill</td>
    </tr>
  </tbody>
</table>


## Getting started

This project uses the build tool [Gulp][gulp] to automate a bunch of stuff for you. The gulp tasks are all defined in the `tasks` directory if you're the curious type. They are used to compile the sources and styles found within the `app` directory and put the final compiled source and styles into the `public` directory. The `public/index.html` will include the final styles and scripts inside of it.

In order to get started, this boilerplate has been setup to show us a simple form right from the start. So let's dive in and give the gulp step a try!

Type the following from the root of the project:

`$ gulp`

You should see some output similar to the following:

```
$ gulp
[16:02:13] Requiring external module babel-core/register
[16:02:14] Using gulpfile ~/Code/vanilla-browser-workshop/gulpfile.babel.js
[16:02:14] Starting 'scripts'...
[16:02:14] Starting 'styles'...
[16:02:14] Starting 'serve'...
[16:02:14] Finished 'serve' after 46 ms
livereload[tiny-lr] listening on 35729 ...
[16:02:15] Finished 'styles' after 175 ms
[16:02:15] Finished 'scripts' after 355 ms
[16:02:15] Starting 'default'...
[16:02:15] Finished 'default' after 114 μs
folder "public" serving at http://localhost:3000
```

As you can see, the scripts and styles are compiled, and then the server get's started. Now that we have that running, let's jump over to the browser and visit [localhost:300][localhost].

![image of boilerplate running](https://s3.amazonaws.com/f.cl.ly/items/1R3O1Q39443m1B2d3k23/Screen%20Shot%202016-01-17%20at%204.03.41%20PM.png?v=bd9646af)

Cool! We have a very simple form with some basic styling already good to go for us.

**If you're having issues bug one of the instructors! We're here to help you out.**


## The HTML

We should know what kind of document we are working with before we move forward. Since the boilerplate was setup for us, we'll just need to open up the `public/index.html` file and take a look.

You should see that the styles are included in the header from `public/index.css` and the script for our JavaScript is included at the top of the body from `public/index.js`.

**ProTip™:** When the browser encounters a `<script>` tag, it is blocking - meaning the browser will immediately download and execute it. To load the `<script>` asynchronously and not block the browser's parsing of the document, set `async='true'` on the tag.

Within the `public/index.html` you should see a basic form with labels and inputs. Familiarize yourself with the different fields.

Are you ready to get coding, yet?

## HTML field validation

Have you ever used a form where you didn't realize you missed a required field until after clicking the submit button, waiting for the page to send data off to the server, waiting for the page to reload, and then finally to get the perplexing red error message at the top? What a pain!

Let's save our users the hassle and let them know right away that they are required to fill in certain fields _before_ sending anything off to the server.

There are a few different ways to do this, and we will elaborate on this later, but the most basic way to get going with form validation is using the built-in HTML field validations.

### Required fields

We really only need the `name` and `email` field to be required. In order to make these inputs required, all we have to do is add the word `required` as an attribute on the `<input>` tag.

For example:

`<input type='text' placeholder='Name' required>`

Go ahead and update the `name` and `email` inputs to include this special attribute.

*After making any changes to this project, be sure to save the file and reload your browser!*

Let's head over to [localhost:3000][localhost] again and try it out.

If you hit the submit button now, you should immediately see a pop-up message near the first missing input telling you the field is required like below.

![required error box](https://s3.amazonaws.com/f.cl.ly/items/172n3w0f0h0f2d0y0i01/Screen%20Shot%202016-01-17%20at%204.43.54%20PM.png?v=b83630c4)

Neato! This is all taken care of for you by the browser out-of-the-box!

### Native validations

Some `<input>` types have intrinsic constraints, such as `type='email'`. If you look at the `public/index.html` you will see that our email field is currently setup as a `type='text'`. Go ahead and change it.

Once you have, we can test it out. Head over to the browser, type in a `Name` value (so that we don't get the required error) and type in a phoney string that doesn't look like an email address. Once you hit submit, you should see a nice message come up and tell you that your input doesn't look like an email.

![email error message](https://s3.amazonaws.com/f.cl.ly/items/3y1l2h0R2x1Q0S1r351B/Screen%20Shot%202016-01-17%20at%204.52.45%20PM.png?v=e72c1557)

If you want to learn more about validations that are available for inputs, [MDN has a great article covering the details][mdn-validations].

## The DOM

The Document Object Model(DOM) is how we are able to interact with our page via JavaScript. The DOM is a representation of what is on the page, including the elements and styles.

> The Document Object Model (DOM) is a programming interface for HTML, XML and SVG documents. It provides a structured representation of the document (a tree) and it defines a way that the structure can be accessed from programs so that they can change the document structure, style and content. The DOM provides a representation of the document as a structured group of nodes and objects that have properties and methods. Nodes can also have event handlers attached to them, and once that event is triggered the event handlers get executed. Essentially, it connects web pages to scripts or programming languages. -[MDN](https://developer.mozilla.org/en-US/docs/Web/API/Document_Object_Model)

One of the trickest things about the DOM is that it's up to the browser vendor to implement it and therefore every implementation is a little different. This is why we get browser compatability issues.

But as you will see there is a large API exposed by the DOM which allows us to craft powerful user experiences. An easy way to think of the DOM is a tree of nested nodes and each node is an element from our HTML document, e.g. a `div`, `p`, `button`, etc...

To visualize this all you need to do is open your web inspector and look at the `Elements` tab to see this structure realized. Each one of those nodes (elements) has many properties and functions you can take advantage off.

![element inspector](https://s3.amazonaws.com/f.cl.ly/items/2o2Y3L1w433t2p1c2M38/kfm9fl7Prq.gif?v=8c898ff8)

## DOM selection

In order to interact with the DOM within JavaScript, the first thing you will need to understand is element selection. If you're familiar with a library such as [jQuery][jquery] or [Zepto][zepto] than you'll be used to doing this by way of the `$()` selector. By selecting an element (DOM node) we get a reference to that element which allows us to take actions on it.

There are several functions we can use to get a reference to a DOM node. Overtime these functions have been introduced to solve different needs. As such, the browser support for them can vary. You should always research browser compatability to ensure they will work for your user base.

- [querySelector](https://developer.mozilla.org/en-US/docs/Web/API/Document/querySelector)
- [querySelectorAll](https://developer.mozilla.org/en-US/docs/Web/API/Document/querySelectorAll)
- [getElementById](https://developer.mozilla.org/en-US/docs/Web/API/Document/getElementById)
- [getElementsByClassName](https://developer.mozilla.org/en-US/docs/Web/API/Document/getElementsByClassName)
- [getElementsByName](https://developer.mozilla.org/en-US/docs/Web/API/Document/getElementsByName)
- [getElementsByTagName](https://developer.mozilla.org/en-US/docs/Web/API/Document/getElementsByTagName)

For now, we are going to focus on `querySelector`. This function behaves similar to jQuery in that it allows you to specify a [css selector][css-selector] that will be used to find the first matching node. One of the nice feature of this function is that it is available both on the `document` as well as `element`. This means you can search for an element in the entire document or narrow your search to the sub elements of an existing element.

For now, let's just open up the dev console and mess around. We can begin by selecting the input with a name attribute of `name`.

```js
var inputName = document.querySelector('input[name="name"]')
```

This returns us an [`HTMLInputElement`][html-input] which has a `value` property available. Go ahead and set the value property to see an example of how you can change the DOM.

```js
inputName.value = 'My new value!'
```

You should see the value you set reflected in the form's input.

Whenever dealing with a DOM node it's important to understand what type of element you have and what are it's parents. That will determine the properties and functions available to you.

## Event handling

When users interact with the web page the DOM publishes these interactions as events, for example; `click`, `scroll`, `keypress`, and [more][mdn-events].

After selecting an element from the DOM, we can call it's [`addEventListener`][event-listener] method which will execute a callback function we provide any time that event occurs. Lets start by listening for a `click` on the `document` and trigger an `alert` whenever that event occurs.

```js
var handler = function() {
  alert('user clicked on the page')
}

document.addEventListener('click', handler)
```

Now if you click on the page you should receive an alert with your message. Alerting users every time they click the page can get annoying so lets remove that event listener.

```js
document.removeEventListener('click', handler)
```

It's important to note that in order for us to be able to remove an event listener we need to name our functions so we can specify what to remove for that event.

Using this simple API we can trigger the complex logic we will be writing shortly.


### Multiple event listeners

One of the great things about event listeners is that we can attach multiple listeners per event. For example:

```js
var handlerOne = function() {
  // Do some logic
}

var handlerTwo = function() {
  // Do some logic
}

document.addEventListener('click', handlerOne)
document.addEventListener('click', handlerTwo)
```

Now both of these functions will be executed whenever someone clicks the page.

### Event listener parameter

When we attach callback functions with event listeners these functions are passed an event parameter. The type of event given will change depending on the type of input device which triggered it. Since we are listening for a `click` event we will receive a [`MouseEvent`][mouse-event].

```js
var logEvent = function(evt) {
  console.log(evt)
}

document.addEventListener('click', logEvent)
```

If you inspect this event in the console you will see there are quite a few properties available to us. The ones most commonly used are `target` and `currentTarget`.

`target` is the element who dispatched the event.

`currentTarget` is the element who handled the event during the event capture (a.k.a bubbling) phase.

Although important, we won't dive into the distinction between these two just yet. For our purposes we are going to use the `currentTarget` event to always get a reference to the element who has the event listener listening for the event.

## Add and remove skills

Now that we know how to add event listeners, let's add some additional functionality to our form. We have more than just a single skill to boast about, so we want to be able to add multiple skills. We may also change our mind while we're filling out the form so we will have to be able to remove a skill, also.

### Adding a skill

You probably noticed the nice `+` button next to the `skills` input. Right now it doesn't do anything. We need to add a `click` handler in order to bring it to life.

In order to do that, we need to first select the `.add-skill` button which we will attach the handler to and then we need to also have a "blueprint" of the original div containing the skill elements so that we can copy it when we add another one.

```js
var addSkillButton = document.querySelector('.add-skill')
var skillTemplate = document.querySelector('.skills')

var addSkillHandler = function(evt) {
  alert('adding skill')
}
addSkillButton.addEventListener('click', addSkillHandler)
```

If you give this a try in your browser now, you'll see the alert pop up when you click on the plus.

What does "add skill" really mean? We want to duplicate the skill blueprint we have, `skillTemplate` and create another Node just like it. Then, we want to append it at the end of the list of skills.

We can clone any DOM Node with the `cloneNode` method. The `cloneNode` method takes an optional boolean argument to determine whether it should be a deep or shallow clone. Since we want the entire `input-group` element and all of it's children Nodes, we are going to pass a `true` in.

Unfortunately, there is no `appendAfter` method we can use like in jQuery, but there is an [`insertBefore`][insert-before] method. We can use `insertBefore` to append the new cloned Node just before the submit button.

The `insertBefore` method uses a handle on the parent node to attach the new Node in the correct spot within the tree. We can use the `parentNode` accesor on any DOM Node to get it's parent, or, since we know the parent is the form we can just directly select it again.

```js
var addSkillButton = document.querySelector('.add-skill')
var skillTemplate = document.querySelector('.skills').cloneNode(true)

var addSkillHandler = function(evt) {
  var submitNode = document.querySelector('.submit')
  var parentNode = submitNode.parentNode
  var newSkill = skillTemplate.cloneNode(true)
  parentNode.insertBefore(newSkill, submitNode)
}
addSkillButton.addEventListener('click', addSkillHandler)
```

We just grew a branch on our DOM tree 🌳! Does it look right, though?

![added skill](https://s3.amazonaws.com/f.cl.ly/items/113x3C3q1H133y3b3C1e/Screen%20Shot%202016-01-24%20at%2010.14.00%20AM.png?v=ad58e105)

After we clone a new node, we need to change the plus sign to a minus sign on the previous one. If you look at the `public/index.html` you'll see that both the plus and minus icons already exist in the DOM, except that the minus is currently `hidden` with the class `hidden`.

In jQuery, you can use the `.last()` method after selecting a group of elements to get the last one. Let's write a helper method that does just that for a given selector.

```js
var last = function(selector) {
  var all = document.querySelectorAll(selector)
  var length = all.length
  return all[length - 1]
}
```

Now let's update our `addSkillHandler` method to get a handle on the previous skill, update it to show the minus instead of the plus, and _then_ add the new skill to the end of the list.

When we have a handle on the DOM Node as we will with the previous skill, we can get a list of the Node's classes by using the [`classList` accessor][class-list]. We can then use `.add()` and `.remove()` to add and remove classes from this Node.


```js
var addSkillButton = document.querySelector('.add-skill')
var skillTemplate = document.querySelector('.skills')

var addSkillHandler = function(evt) {
  var prevSkill = last('.skill')
  var newSkill = skillTemplate.cloneNode(true)
  var submitNode = document.querySelector('.submit')
  var parentNode = submitNode.parentNode

  prevSkill.querySelector('.add-skill').classList.add('hidden')
  prevSkill.querySelector('.remove-skill').classList.remove('hidden')
  parentNode.insertBefore(newSkill, submitNode)
}
addSkillButton.addEventListener('click', addSkillHandler)
```

If you head over to the browser, it should work! Sweet DOM manipulation.

Did you try to click the second plus button like a smarty pants? Then you realize that it didn't work, right?

Fix it by attaching the handler to the new element as well.


```js
var addSkillButton = document.querySelector('.add-skill')
var skillTemplate = document.querySelector('.skills')

var addSkillHandler = function(evt) {
  var prevSkill = last('.skill')
  var newSkill = skillTemplate.cloneNode(true)
  var submitNode = document.querySelector('.submit')
  var parentNode = submitNode.parentNode

  prevSkill.querySelector('.add-skill').classList.add('hidden')
  prevSkill.querySelector('.remove-skill').classList.remove('hidden')
  newAddSkill.querySelector('.add-skill').addEventListener('click', addSkillHandler)

  parentNode.insertBefore(newSkill, submitNode)
}
addSkillButton.addEventListener('click', addSkillHandler)
```

Great, now you're ready to remove some skills!

### Removing a skill

Much like we created an event handler for adding a skill, we will now just create one to remove a skill.

```js
var removeSkillHandler = function(evt) {}
```

When the minus button is clicked, you'll remember from the event section that the minus button element becomes the `currentTarget` on the event object passed in as the first argument to our handler.

If we have a handle on the minus button, then we can select the `parentNode` to get the group of elements that makes up a single skill. And if we can do that, then we can just call [`remove()`][remove-node] do remove the Node from the DOM.

```js
var removeSkillHandler = function(evt) {
  var skill = evt.currentTarget.parentNode
  skill.remove()
}
```

## Add submit event

Now that we've covered the basics of getting a DOM element and hooking into it's events, let's add a submit event handler to the form. Later, we'll use this handler to kick off a request to the backend.

To start create a new file called `main.js` in the app directory.

Our first step in this new file is to get a reference to the form element. Create a variable named `form`, and assign the form DOM element on the page to it.

```js
var form = document.querySelector('form')
```

On the next line create a `submitHandler` function. We will need to have the event passed into the handler available to us so add the parameter `evt` to the function declaration.

```js
var submitHandler = function(evt) {}
```

We will be using the `preventDefault` method on the event object. This handy method will stop the form from submitting to the backend so that we can submit this information using AJAX.

Let's also throw in an `alert` so that we can see some feedback from the submit handler. Otherwise, when we click on the submit button nothing will happen which can be confusing. Add the two new commands to the submit handler declaration like below.

```js
var submitHandler = function(evt) {
  evt.preventDefault()
  alert('submit!')
}
```

In order to hook this handler up to our form element, we will need to use the `addEventListener` method that exists on the form node. Pass in the event we want to listen for (submit) as well as the custom handler we just wrote.

```js
form.addEventListener('submit', submitHandler)
```

Now if you refresh the page you should be able to submit the form after filling out the two required fields, but instead of the page refreshing and losing everything you've entered the page will show you an alert and you won't lose any of the information you typed.

Our event handler is now working!

## Serialize the form data

Before we can begin to assemble the pieces for talking to our server, we will need to make sure that we can capture the data from our form in a format that our API endpoint can digest. The API for this workshop is built with [`rails-api`][rails-api] which will be expecting a JSON API object on the request.

We need to take the form data and convert it to an object that looks like this

```json
{
  "form": {
    "name": "",
    "email": "",
    "city": "",
    "state": "",
    "github": "",
    "twitter": "",
    "bio": "",
    "skills_attributes": []
  }
}
```

If we were using something like jQuery we may use the `.serializeArray()` method. Let's write our own `serializeArray` method.

```js
var serializeArray = function(selector) {}
```

We want to be able to pass in our selector for the form to this method and get the JSON object back. First, let's use `document.querySelector` again to grab the form.

```js
var serializeArray = function(selector) {
  var form = document.querySelector(selector)
}
```

**ProTip™:**  In our example, we know we are passing in a form selector but if you wanted to use this method in a different scenario, you might want to check that the element we have selected is actually a form by using `form.tagName` to check the element's tag name.

Now we need to grab all the inputs from inside the form element. Remember the `querySelectorAll` method mentioned above? While `querySelector` returns a single Node in the DOM, `querySelectorAll` will return a [`NodeList`][node-list].

The unusual thing about this is that `NodeList` is not a JavaScript `Array` and so we cannot use methods like `map` or `forEach` to iterate over the items returned. We could do some fancy work and add these methods to the prototype for `NodeList`, but to keep things simple we will just use a basic `for` loop to iterate over them.

```js
var serializeArray = function(selector) {
  var form = document.querySelector(selector)
  var formInputs = form.querySelectorAll('input,textarea')

  for (var i = 0; i < formInputs.length; i++) {
    var item = formInputs[i]
  }
}
```

Now that we can iterate over them we will need to store the `name` and `value` of each item onto a JSON object as the `key`,`value` pair. When we are accessing the `Node` directly, we can easily call `node.name` or `node.value` to access these attributes.

We can't test our method unless we call our `serializeArray` method from somewhere, so let's add the call into our `submitHandler` method.

```js
var submitHandler = function(evt) {
  evt.preventDefault()
  var data = serializeArray('form')
}
```

Ok, now let's build our final `data` JSON object. Let's also log it to the console so that we can inspect it make sure it matches our expected output that we defined above.

```js
var serializeArray = function(selector) {
  var form = document.querySelector(selector)
  var formInputs = form.querySelectorAll('input,textarea')

  // Empty object for us to set key values of inputs
  var data = {}

  for (var i = 0; i < formInputs.length; i++) {
    var item = formInputs[i]
    data[item.name] = item.value
  }

  // Log out our final object so we can inspect it
  console.log(data)
}
```

When you reload the page and submit the form again, you should see something similar to this outputted in the console.

![build the form JSON](https://s3.amazonaws.com/f.cl.ly/items/3V3p121o1C093I0B0F1s/6aUSiYmFDT.gif?v=bb86545c)

Well that looks _nearly_ correct. Can you spot the problems?

Firstly, we're capturing the submit button. There are a couple ways to avoid this.

We could change the submit button from an `input` to a `div` and change our handler from the `form` `submit` event to the `div` `onclick` event. If we were to do this, we would lose the native validations, so let's nix this idea.

We could instead just change our `querySelectorAll(...)` selector string to ignore the `type=submit` input. That seems simple enough, so let's add that in.

```js
var serializeArray = function(selector) {
  var form = document.querySelector(selector)
  var formInputs = form.querySelectorAll('input:not([type=submit]),textarea')

  // Empty object for us to set key values of inputs
  var data = {}

  for (var i = 0; i < formInputs.length; i++) {
    var item = formInputs[i]
    data[item.name] = item.value
  }

  // Log out our final object so we can inspect it
  console.log(data)
}
```

Ok, let's try this again!

![build the form JSON part 2](https://s3.amazonaws.com/f.cl.ly/items/2N0C470x010K2k302e3Q/RRBJpcGUMt.gif?v=94639764)

Something is still off. Did you spot it?

We need our `skills_attributes` key to have an array value.

## Build XHR and submit

Now that we have our data we need to send it to the server using an XHR or [`XMLHttpRequest`][xhr-mdn]. This allows us to communicate with the server without changing pages then do some action based on the data we get back. We are going to create a function that we can put in our event handlers that will do this.

```js
var xhr = function(method, path, data, callback) {}
```

First, we create a new instance of XHR.

```js
var xhr = function(method, path, data, callback) {
  var request = new XMLHttpRequest()
}
```

Next we'll use `open(method, path, async)` to initialize the request.

- `method` a string of an HTTP method to use, such as 'GET', 'POST', 'PUT', 'DELETE'. This will match the Verb on the API table up top.
- `path` a string of the full path to send the request to. This will include the API endpoint URL as well as the path.
- `async` a boolean flag that dictates whether the script should run asynchronously.

**ProTip™:** `async` should always be `true` to prevent blocking. Stopping JavaScript execution especially hurts time sensitive things like rendering or event listening/handling.

```js
var xhr = function(method, path, data, callback) {
  var request = new XMLHttpRequest()
  request.open(method, path, true)
}
```

Because we'll be using a JSON endpoint any requests we make to it should contain a `Content-Type` header.

```js
var xhr = function(method, path, data, callback) {
  var request = new XMLHttpRequest()
  request.open(method, path, true)
  request.setRequestHeader('Content-Type', 'application/json')
}
```

XHR only has one event we care to listen to and that's [onreadystatechange][onreadystatechange].

The ready state of the XHR will change a few times, but we are looking for the last state of `4` which is triggered when the request operation is complete.

We'll also check to make sure we got a good server response. If it passes those checks, we'll parse the response JSON, then send the object back though a callback.

```js
var xhr = function(method, path, data, callback) {
  var request = new XMLHttpRequest()
  request.open(method, path, true)
  request.setRequestHeader('Content-Type', 'application/json')
  request.onreadystatechange = function() {
    // ignore anything that isn't the last state
    if (request.readyState !== 4) { return }

    // if we didn't get a "good" status such as 200 OK or 201 Created send back an error
    if (request.readyState === 4 && (request.status !== 200 && request.status !== 201)) {
      callback(new Error('XHR Failed: ' + path), null)
    }

    // return our server data
    callback(null, JSON.parse(request.responseText))
  }
}
```

Lastly just close and send the request with our data using the `send` function.

```js
var xhr = function(method, path, data, callback) {
  var request = new XMLHttpRequest()
  request.open(method, path, true)
  request.setRequestHeader('Content-Type', 'application/json')
  request.onreadystatechange = function() {
    // ignore anything that isn't the last state
    if (request.readyState !== 4) { return }

    // if we didn't get a "good" status such as 200 OK or 201 Created send back an error
    if (request.readyState === 4 && (request.status !== 200 && request.status !== 201)) {
      callback(new Error('XHR Failed: ' + path), null)
    }

    // return our server data
    callback(null, JSON.parse(request.responseText))
  }
  request.send(data)
}
```

Now we're ready to use it inside of our `submitHandler`. We can call it with the 'POST' method and pass it the form data.

```js
var apiURL = '//sandiegojs-vanilla-workshop.herokuapp.com'

var submitHandler = function (evt) {
  evt.preventDefault();
  var path = apiURL + '/forms'
  xhr('POST', path, serializeArray('form'), function(err, data) {
    if (err) { throw err }
    console.log(data)
  })
}
```

## Handle server response

Now that we are able to submit the form to the server let's do something with the response. We're going to write any errors from our XHR request to the screen. We're also going to render the contents of the form to the screen if it was successfully submitted.

Some of the methods we'll be using are:

- [`Document.createElement`][mdn-createelement] this method creates a DOM element of the type specified in the first parameter. For example, we can pass in `div`, `p`, `ul`, etc.
- [`Document.createTextNode`][mdn-createtextnode] this method creates a text node that can be placed in the document. We'll use this to hold content we author.
- [`Node.appendChild`][mdn-appendchild] this method can be called on any element created using createElement. As you can guess from the name it adds the node passed in to the end of node you call appendChild on.

Let's create a small helper function. It creates a DOM node and adds some text to it. We're going to be making a lot calls to this.

```js
var createElementWithTextNode = function (tagName, tagContent) {}
```

Right inside of that function declaration we should add a call to `document.createElement`.

```js
var createElementWithTextNode = function (tagName, tagContent) {
  var node = document.createElement(tagName);
}
```

Next up we will create a text node that will hold whatever is inside of the tagContent variable.

```js
var createElementWithTextNode = function (tagName, tagContent) {
  var node = document.createElement(tagName);
  var textNode = document.createTextNode(tagContent)
}
```

Then we combine the two freshly created nodes. You can add other HTML nodes or text nodes to a node even if the HTML node would usually have children, such as `<br>`.

```js
var createElementWithTextNode = function (tagName, tagContent) {
  var node = document.createElement(tagName);
  var textNode = document.createTextNode(tagContent)
  node.appendChild(textNode)
}
```

And lastly we will return the node we created with a child text node.

```js
var createElementWithTextNode = function (tagName, tagContent) {
  var node = document.createElement(tagName);
  var textNode = document.createTextNode(tagContent)
  node.appendChild(textNode)
  return node
}
```

Now that we have that built, we can start to output some content. First create the renderError function

```js
var renderError = function (error) {}
```

The error render will be very simple. We are going to get a reference to the response container, create a node with our helper function, add a custom class to it, and append it to the document.

```js
var renderError = function (error) {
  var responseNode = document.querySelector('.response-wrapper')
  var errorNode = createElementWithTextNode('div', error.toString())
  errorNode.className = 'error'
  responseNode.appendChild(errorNode)
}
```

Now it's time to create the renderFormData function. This function is going to create many DOM nodes and append them to the response container.

```js
var renderFormData = function(data) {}
```

The first thing we want to do inside of the renderFormData function is get a reference to the response container just like in our `renderError` function

# UNFINISHED

```js
var renderFormData = function (data) {
  var responseNode = document.querySelector('.response-wrapper')

  //generic success message
  var successNode = createElementWithTextNode('div', 'You\'ve add a new card')
  successNode.className = 'success'
  responseNode.appendChild(successNode)

  var dictionaryNode = document.createElement('dl')
  var keys = ['name', 'email', 'github', 'twitter'], node;
  keys.forEach(function (key) {
    //create a dom node with the name of a value
    node = createElementWithTextNode('dt', key)
    dictionaryNode.appendChild(node)

    //create another dom node with the value
    node = createElementWithTextNode('dd', data[key])
    dictionaryNode.appendChild(node)
  })

  //create a dom node with the name of a value
  node = createElementWithTextNode('dt', 'location')
  dictionaryNode.appendChild(node)

  //create another dom node with the value
  node = createElementWithTextNode('dd', data['city'] + ', ' + data['state'])
  dictionaryNode.appendChild(node)

  //create a dom node with the name of a value
  node = createElementWithTextNode('dt', 'bio')
  dictionaryNode.appendChild(node)

  //create another dom node with the value
  node = createElementWithTextNode('dd', data['bio'])
  dictionaryNode.appendChild(node)

  dictionaryNode.className = 'response'
  responseNode.appendChild(dictionaryNode)
}
```

Finally, we just call renderError & renderFormData in the appropriate places in the original submitHandler.

```js
var submitHandler = function (evt) {
  evt.preventDefault();
  var path = apiURL + '/forms'
  xhr('POST', path, serializeArray('form'), function(err, data) {
    if (err) {
      renderError(err)
      throw err
    }
    console.log(data)
    renderFormData(data)
  })
}
```


If you fill out the form, you should see a response similar to what was described in the **Serialize the form data** section. For example:

```json
{
  "id": 1,
  "name": "Testy McTesterson",
  "email": "test@sandiegojs.org",
  "city": "San Diego",
  "state": "CA",
  "github": "testdev",
  "twitter": "testtwitter",
  "bio": "Lorem Ipsum...",
  "created_at": "2016-01-21T07:09:01.640Z",
  "updated_at": "2016-01-21T07:09:01.640Z"
}
```

Sometimes, something will go wrong with a request and instead of receiving a new record from the backend we'll get an error of some kind! We already built our xhr function to let us know when something goes wrong during the AJAX request, but now we need to do something to let the user know that there was a problem.

Update the xhr callback inside of the submitHandler function to show the user a generic error message in an alert when the callback receives an error.

```js
  ...
  xhr('POST', path, serializeArray('form'), function(err, data) {
    if (err) {
      alert('Oh no! We\'re sorry, but something went wrong! \nCheck the console for more details.')
      throw err
    }
    console.log(data)
  })
  ...
```

## Render the response

## Add JS Validation
Earlier you added form validation using attributes. This is a quick and easy way to perform validation, but it does have limits. For example, you can't add custom error messages or styling. To get more flexibility and control, use JavaScript.

In this section, you will be playing with the HTML5 constraint validation API to check and customize the state of a form element. You have several goals:

- Provide a custom validation message when the Email the user types is invalid
- Hide that custom validation message as soon as the email is valid
- Add custom validation logic to the State field without using an attribute
- Apply custom styling to the validation error messages
- Prevent form submission if there are validation errors.

To achieve these goals you will:

0. Add a new file to handle validation logic.
0. Add a keyup event listener to the email field to present a custom message
0. Add a submit event listener to the form
0. Prevent form submission if there are any errors
0. Loop through the form elements
0. Check the validity of each field
0. Set or clear the validation error messages
0. Add custom validation logic to the state field

### Add a new file to handle validation logic

0. In the `app` directory, create `validation.js`.

This file will automatically be concatenated with any other js files during the build process.

### Handle `keyup` event for email field

0. Get a reference to email field. Hint: Use a query selector you learned about earlier.
0. Add `keyup` event listener attached to the email field.
0. Inside the event listener function, you will check the `email.validity.typeMismatch` property to determine if the email the user has typed is valid.

```
if (email.validity.typeMismatch) {

} else {

}
```

0. If there is a problem `typeMatch` will be `true`. This is your opportunity to set a custom error message using the `setCustomValidity` function.

```
if (email.validity.typeMismatch) {
  email.setCustomValidity('Oops, can't send email to that.');
} else {
  email.setCustomValidity('');
}
```

If you pass `''` to `setCustomValidity` the field will be considered valid.

The complete event handler should look like this:

```
var email = document.querySelector('input[name="email"]');

// Check for valid email while the user types
email.addEventListener('keyup', function(event) {
  // see https://developer.mozilla.org/en-US/docs/Web/API/ValidityState for
  // other validity states
  if (email.validity.typeMismatch) {
    email.setCustomValidity('Oops, try a real email address.');
  } else {
    email.setCustomValidity('');
  }
  setError(email);
}, false);
```

Try the new logic. Go to your form, add a name, but leave the email blank. Click Submit. Start typing your name in the Email field. Notice that now you see your custom message. Now type a valid email address. The message goes away as soon as the address is valid.

### Handle `submit` event for form

You can use the technique above to handle each validated field, but you are still stuck with the standard formatting for the messages. To get more control, you need to turn off the automatic validation and handle the submission event yourself.

0. Get a reference to the form element. Hint: Use the tag name.
0. Prevent the automatic validation behavior.

`form.noValidate = true;`

Pro Tip: You can achieve the same thing by adding a `novalidate` attribute to the form.

0. Now add a `submit` event handler that calls a `validateForm` function.

You should have the following:

```
var form = document.getElementsByTagName('form')[0];

...

form.noValidate = true;

form.addEventListener('submit', validationForm, false);

function validateForm(event) {

}
```

### Prevent submission if there are any errors

Now that you turned off the automatic validation, the form will always submit! You certainly don't want that.

0. If you didn't already create the `validateForm` function.
0. Inside the function, get a reference to the form using the `event.target`.
0. Have the form check if it is valid.

```
var form = event.target;
if (!form.checkValidity()) {
  // form is invalid
}
```

0. Now prevent the submission if there is a problem.

```
var form = event.target;
if (!form.checkValidity()) {
  // form is invalid
  event.preventDefault();
}
```

0. Try it out! In the form, click Submit. Nothing happened? That's right. Now add a name and a valid email address and click Submit. Notice that the form submitted. In the next few steps, you will bring back the error messages.

### Loop through the fields

Every form has an elements array. You can use that loop through the fields.

0. Create the loop above the form validity check.

```
var form = event.target;
var f;
for (f = 0; f < form.elements.length; f++) {

}

if (!form.checkValidity()) {
  // form is invalid
  event.preventDefault();
}
```

0. Within the loop, get a reference to the current field. `validateForm` should now look like:

```
function validateForm(event) {
  var form = event.target;
  var f;
  var field;

  for (f = 0; f < form.elements.length; f++) {

    // get field
    field = form.elements[f];

  }

  if (!form.checkValidity()) {
    // form is invalid
    event.preventDefault();
  }
}
```

The behavior of the form hasn't changed so charge on to the next section.

### Check the field validity

You know you will have custom logic for validating the state field, so use a function to encapsulate all of the field validation logic. Start with using the standard validation logic.

0. Create a new function `isValid` that takes a field as a parameter.
0. Like the form, fields have a `checkValidity` function. Use this function to determine the return value for the function. Here's how it should look:

```
function isValid(field) {
  return field.checkValidity();
}
```

0. Now call this function from the loop inside the `validateForm` function.

```
...
  for (f = 0; f < form.elements.length; f++) {
    // get field
    field = form.elements[f];

    if(isValid(field)) {
      // remove error styles and messages
    } else {
      // style field, show error, etc.
    }
  }
...
```

### Set and clear error messages

Now that you are looping through each field and checking its validity, you can use the information to format the feedback to the user. In the HTML, you may have noticed that below the fields that have validation, there is a `span` element. You will use this element to display feedback to the user.

0. Create a `setError` function that takes a field as a parameter.
0. Inside the function, get a reference to the `span` you will use to show the error. To do that, use the handy `nextElementSibling`.
0. Now that you have a reference to the element, set its `innerHTML` to the field's `validationMessage`. The `validationMessage` is either the default message from the browser or a custom message you set. The finished function should loop like:

```
function setError(field) {
  var error = field.nextElementSibling;
  if (error) error.innerHTML = field.validationMessage;
}
```

0. Set up a similar function to clear the error text. Copy and paste the `setError` function.
0. Change the function name to `clearError`.
0. Instead of `field.validationMessage`, set the `innerHTML` to `''`. The complete function is:

```
function clearError(field) {
  var error = field.nextElementSibling;
  if (error) error.innerHTML = '';
}
```

0. Finish the logic by calling these function from `validateForm`. If the field is valid, call `clearError` and if not call `setError`.

```
...
    if (isValid(field)) {
      // remove error styles and messages
      clearError(field);
    } else {
      // style field, show error, etc.
      setError(field);
    }
...
```

0. Try it out. Click Submit. Do you see the error messages? Do the error messages disappear when you enter valid data?

As you type in the email field, does the message change? No? What is missing from the `keyup` event listener?

### Add custom validation logic to the state field

Time for the bonus round! Custom validation logic could involve evaluating multiple fields on the form, making a call to the server, or performing some calculation in a worker thread. While those are beyond the time you have for this workshop, this section will walk you through creating a some custom logic. Keep in mind that there are better ways to enforce the logic presented in the here, but hey, use your imagination. ;-)

You will add this logic to the top of the `isValid` function you created earlier.

0. First, check if the field passed to the function is named `state`.
0. If so, check if the state is in the list of valid states. Valid states include: CA, TX, NY.

```
function isValid(field) {
  // custom logic for state
  if (field.name === 'state') {
    var validStates = ['CA', 'TX', 'NY'];
    if (validStates.indexOf(field.value) === -1) {
      // invalid state
    } else {
      // valid state
    }
  }
  return field.checkValidity();
}
```

0. When the state is invalid, set a custom validity message. When it is not, set an empty string.

```
    ...
    if (validStates.indexOf(field.value) === -1) {
      // invalid state
      field.setCustomValidity('Use CA, TX, or NY');
    } else {
      // valid state
      field.setCustomValidity('');
    }
    ...
```

Now when you try to submit without a state or an invalid state, you will get an error message just like with name and email.

That's it for validation! You may now add "HTML5 constraint validation API" to your resume.

## Welcome text with cookies

## Adding classes to style error message or boxes

## Test with Mocha

## Heroku to Publish

[Heroku][heroku] is a web hosting platform that allows developers to go from code to running apps in minutes. It has a free tier, as well as a super fast workflow. To try it out create a free account on Heroku, and then use the button below to automatically deploy this app directly from GitHub to a running Heroku instance.

[![Deploy](https://www.herokucdn.com/deploy/button.svg)](https://heroku.com/deploy?template=https://github.com/sandiegojs/vanilla-browser-workshop)

Read [Getting Started with Node.js on Heroku][node-heroku] for more information.

[cookies]: https://devdocs.io/dom/document/cookie
[css-selector]: https://developer.mozilla.org/en-US/docs/Web/Guide/CSS/Getting_Started/Selectors
[devdocs]: http://devdocs.io
[dom]: https://devdocs.io/dom/
[event-listener]: https://developer.mozilla.org/en-US/docs/Web/API/EventTarget/addEventListener
[events]: https://devdocs.io/dom_events
[git-scm]: http://git-scm.com/downloads
[gulp]: http://gulpjs.com/
[heroku]: http://heroku.com
[heroku-node]: https://devcenter.heroku.com/articles/getting-started-with-nodejs#introduction
[heroku-toolbelt]: https://devcenter.heroku.com/articles/getting-started-with-nodejs#set-up
[html-input]: https://developer.mozilla.org/en-US/docs/Web/API/HTMLInputElement
[localhost]: http://localhost:3000
[mdn]: https://developer.mozilla.org/en-US/
[mdn-events]: https://developer.mozilla.org/en-US/docs/Web/Events
[mdn-validations]: https://developer.mozilla.org/en-US/docs/Web/Guide/HTML/HTML5/Constraint_validation
[mocha]: https://devdocs.io/mocha/
[mouse-event]: https://developer.mozilla.org/en-US/docs/Web/API/MouseEvent
[node-install]: https://nodejs.org/download/
[npm-g-without-sudo]: https://github.com/sindresorhus/guides/blob/master/npm-global-without-sudo.md
[san diego js]: http://sandiegojs.org/
[xhr]: https://devdocs.io/dom/xmlhttprequest
[zepto]: http://zeptojs.com/
[jquery]: https://jquery.com/
[xhr-mdn]: https://developer.mozilla.org/en-US/docs/Web/API/XMLHttpRequest
[onreadystatechange]: https://developer.mozilla.org/en-US/docs/Web/API/XMLHttpRequest/onreadystatechange
[sdjs-app]: //sandiegojs-vanilla-workshop.herokuapp.com
[rails-api]: https://github.com/rails-api/rails-api
[node-list]: https://developer.mozilla.org/en-US/docs/Web/API/NodeList
[insert-before]: https://developer.mozilla.org/en-US/docs/Web/API/Node/insertBefore
[class-list]: https://developer.mozilla.org/en-US/docs/Web/API/Element/classList
[remove-node]: https://developer.mozilla.org/en-US/docs/Web/API/ChildNode/remove
[mdn-createtextnode]: https://developer.mozilla.org/en-US/docs/Web/API/Document/createTextNode
[mdn-createelement]: https://developer.mozilla.org/en-US/docs/Web/API/Document/createElement
[mdn-appendchild]: https://developer.mozilla.org/en-US/docs/Web/API/Node/appendChild
