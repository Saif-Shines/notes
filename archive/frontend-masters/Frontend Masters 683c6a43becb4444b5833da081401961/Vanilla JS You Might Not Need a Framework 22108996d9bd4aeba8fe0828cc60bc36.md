# Vanilla JS: You Might Not Need a Framework

# Resources

[You Don't Need that Library](https://firtman.github.io/vanilla/)

[](https://firtman.github.io/vanilla/slides.pdf)

# Introduction

The idea of this workshop is not to use Vanilla JS for everything. The idea is not just stick with frameworks but know the vanilla level of things.

Why Vanilla? In software developer, it’s used to described something “plain”. It’s coming from a italian ice cream called gelato, where vanilla is pure base before any flavour is added. This analogy as been taken up.

### History

There’s a general moment happening in history, that developers are extensively using frameworks, like 12 years ago - there’s this parody website called [vanilla-js launched.](http://vanilla-js.com/) If you take a look at it, it basically gives you all the features, to convince the the developer you don’t need anything more to use the features you need to build the site you want.

### Benchmarks

[Interactive Results](https://krausest.github.io/js-framework-benchmark/current.html)

### Code

[GitHub - firtman/coffeemasters-vanilla](https://github.com/firtman/coffeemasters-vanilla)

# Vanilla Javascript

### DOM vs DOM API

Document Object Model(DOM) is a representation of the HTML (or web page) in the memory. So, JS can interact with it through the API exposed to us, as developers.

<aside>
💡 The `window` object was literally supposed mean a window, because users cannot open mulitple web pages in same browser window. There’s no concept called *****tabs***** at all. That’s where this is coming from.

</aside>

We often come across *********************web page is not responding********************* dialog box. It occurs because the thread of javascript execution manipulating the DOM does not finish it’s execution, so browser waits and waits for some time and throws this message. 

### Finding elements in the DOM

When you select elements using `document.getElementbyID()` or `document.querySelector()` , more often the return values are one of the following:

1. `HTMLElement`
2. `HTMLCollection` — 
3. `NodeList`

<aside>
💡 If the element is not found, the return value of dom API would be `null`

</aside>

<aside>
💡 If you use DOM method like `getElementByClassName(..)` that returns a `HTMLCollection`, which does not have any class by that name, it returns an empty array `[]` .

</aside>

<aside>
💡 When it comes to Live Collections, they don’t have modern Array interfaces such as *****************filter, map, reduce.***************** For example, `querySelectorAll(..)` can only allow `.forEach(..)` to run on it. In those cases you can do `Array.from(collection)`

</aside>

### Modifying DOM

To change the style

```jsx
element.style.color = "yellow"
element.style.fontSize = 1rem // font-size: 1rem
```

To listen to events

```jsx
element.addEventListner('click', (event) => {
  // something
})
```

Accessing and Editing - `textContent` and `innerHTML`

```jsx
element.textContent = "The text has been changed"
element.innerHTML = `
<h1> The text has been changed </h1>
`;
```

# The DOM

It’s a in memory representation of the actual HTML source. To illustrate, you can write a simple HTML as follows:

```html
<!DOCTYPE html>
<title> A title </title>
<h1> This is a Heading </h1>
<style>
body {
	color: red;
}
</style>
```

<aside>
💡 Even if there’s no `<body>` tag present in the HTML source, you will see the browser adding the `red` background. It’s because, it’s not mandatory for HTML source to have a header and a body, but DOM should have it according to W3C standards.

</aside>

There’s a validation service to understand if an HTML is living up to W3C standards.

[The W3C Markup Validation Service](https://validator.w3.org/)

When writing code without any formal `head` or `body`, the browsers actually categorise the HTML source into visible and non-visible elements. For example, in the above code snippet, ******************title and doctype****************** are invisible to render content, so they’ll be automatically wrapped under a `head` and while starting from ***h1*** , body begins to play.

## Scoping querySelector

`querySelector(..)` vs `querySelectorAll(..)`

The first one returns the fist element in the DOM tree, while the all returns the entire DOM elements in the form of ***nodeList.***

```bash
#
document.querySelector('.navlink')
>> <a class="navlink material-symbols-outlined" id="linkHome" href="/"> local_cafe </a>
document.querySelectorAll('.navlink')
>> NodeList(2) [..]
Array.from(document.querySelectorAll('.navlink'))
>> (2) [..]
```

You can also do query selection within the selected element in the DOM. Since we are always used to do with `document.querySelector(..)` , we often complain about performance.

```bash
const nav = document.querySelector('nav');
>> nav.querySelector('#badge');
<span id="badge" hidden=""></span>
```

## Adding scripts async & defer

- It’s a deprecated advice to add all the scripts at the bottom of your HTML source file. The reason: When browser renders the HTML line one after another, and comes across a script tag `<script src="app.js"></script>` , the browser will download the `app.js` file and execute it, and then continue parsing the rest of HTML. So, the advice is deprecated because there are new ways to handle it today.
- It’s also old advice to follow folder directory to have scripts, images, js in seperate folders. We are yet to see the newer approach.

**`defer`** 

`<script src="app.js" defer></script>`

1. Browser continues comes across the script with ******defer****** attribute.
2. Browser will download the `app.js` in parallel and yet continue to parse the rest of HTML.
3. Browser will execute the downloaded `app.js` after the HTML parsing is complete.
4. Effectively, `defer` has deferred the ******************execution****************** after parsing.

<aside>
💡 You can use `defer` as default in most cases.

</aside>

**`async`**

`<script src="app.js" async><script>`

1. Browser continues comes across the script with ******async****** attribute.
2. Browser will download the `app.js` in parallel and yet continue to parse the rest of HTML.
3. Browser will halt the parsing, and execute the `app.js` downloaded, then resume parsing.
4. Effectively, parsing asynchronously. 

<aside>
💡 `async` is popular to be used for small js executions. For example, analytics, pings etc.,

</aside>

## Main script setup

We now know the differences between `async` and `defer` attributes for `app.js` script. More often than not, the DOM might not be ready for manipulation. To ensure certainty, we wait for the browser to emit an event named `DOMContentLoaded` on `window`.

```jsx
window.addEventListener("DOMContentLoaded", () => {
  const nav = document.querySelector("nav");
  console.log(nav);
});
```

`window.addEventListener(”load”,() ⇒{..}`

The `load` event used to be common a decade ago, where you will wait for the page to be completely loaded - video is downloaded, css is downloaded and parsed, js is downloaded, everything.

- The first pixel on the screen is termed, first paint.
- The first meaningful part rendered on the screen is termed, first contentful paint.

## Event Binding  & Handlers

[Event reference | MDN](https://developer.mozilla.org/en-US/docs/Web/Events)

<aside>
💡 The spec suggests to use the naming pattern: use lowercase, no word seperator, no camel or kebab. 
Example: `webkitplaybacktargetavailabilitychanged` from [Apple](https://developer.apple.com/documentation/webkitjs/adding_an_airplay_button_to_your_safari_media_controls#2940021)

</aside>

Advanced Event Handling

```jsx
function eventHandler(event) {}

const options = {
  once: true, // in some cases you want to ensure event is triggered just once
  passive: true, // allows browser to prioritize its work of repainting or re-rendering over existing your event
};

element.addEventListener("load", eventHandler, options);
element.removeEventListner("load", eventHandler);
```

Dispatching Custom Events

```jsx
const customEvent = new Event("mycustomevent");
element.dispatch(customEvent)
```

## Helpful Shorthands

JQuery

```jsx
const $ = function(args){ return document.querySelector(args);}
const $$ = function(args){ return document.querySelectorAll(args);}

HTMLElement.prototype.on = function(a, b, c){ return this.addEventListener(a, b, c); }
HTMLElement.prototype.off = function(a, b){ return this.removeEventListener(a, b); }
HTMLElement.prototype.$ = function(s){ return this.querySelector(s); }
HTMLElement.prototype.$$ = function(s){ return this.querySelectorAll(s); }
```

Directory

```markdown
.
├── LICENSE: MIT License                   
├── README.md: An about file
├── app.js
├── app.webmanifest
├── components
│   ├── DetailsPage.css
│   ├── MenuPage.css
│   └── OrderPage.css
├── data 
│   ├── images
│   └── menu.json
├── images
│   ├── icons
│   ├── logo.png
│   ├── logo.sv
│   ├── screen1.jpg
│   └── screen2.jpg
├── index.html : this is where you include a script with type:module
├── services
│   ├── API.js : Capital letters because Caps indicates it acts a a Object.
│   └── Store.js
├── serviceworker.js
└── styles.css 
```

********Using modules in the Browser********

**********************************Change the script to a module type**********************************

```html
<script src="app.js" type="module" defer></script>
```

******************Import the modules from services******************

```jsx
import Store from "./services/Store";
import API from "./services/API";

window.addEventListener("DOMContentLoaded", () => {});
```

Since the `Store` is intended to be a data store for the entire app, we could add it to the global by doing something like this:

```jsx
window.app = {}; 
app.store = Store;
```

<aside>
💡 The reason why `window.store = Store` is ********not******** recommended because we would like the window space to not have multiple properties for the same app. Instead we could have one space (called `app`) and everything related to app can be added as properties on the `app`.

</aside>

************Milestone************

1. We started running serving `index.html` through a dummy server or express in the future.
2. If the UI needs assets we put them in
    1. `images` : Just the images.
    2. `components` : Has css specific to each components like menu page, details page, etc.,
    3. root-level `styles.css` : Has css relavant to home
3. We are building a coffee shop online store. The data, which other wise comes from an API is kept within a folder labeled `data`.
4. The best of the best abstractions for the `app.js` is categorised to a folder labeled `services`.
    1. `API.js` : calls to external services
    2. `Store.js` : a storage that’s later attached to `window.app.store`
    3. `Menu.js` : Funcional logic, that uses `API.js` 

# Routing

We will build a single page application. 

1. The app can either create and destroy the pages.
2. The app can either hide or show the pages.

History API

On the other hand, browsers have History API that allows apps to access the browser session history. These methods allow you to programmatically change the URL displayed in the browser's address bar without causing a page refresh, which is a key component of many "single page" web applications.

```jsx
// pushing new URL; the second argument is unused
history.pushState(optional_state, null, "/new-url");

// to listen to the changes to the URL
window.addEventListener("popstate", event => {
	let url = location.href;
})
```

<aside>
💡 `popstate` won’t be fired if the user clicks on an external link or changes the URL manually.

</aside>

Router.js

```jsx
const Router = {
  init: () => {
    document.querySelectorAll("a.navlink").forEach((a) => {
      a.addEventListener("click", function (event) {
        event.preventDefault();
        console.log("Link Clicked");
      });
    });
  },
  go: (route, addToHistory = true) => {
    console.log(`Going to ${route}`);
  },
};

export default Router;
```

Our objectives with the router are:

1. It should observe the changes to the URL
2. The default behaviour of the browser when it comes across an link or hyperlink is to make a HTTP request via internet to the server. We don’t want do that. So we `preventDefault()`
3. Our app specifically has two buttons - a home and order. Both of them should not make a HTTP call, instead render the right HTML. So we update their click event handlers to prevent the default behaviour.
4. The `addToHistory=true` is a parameter with a default argument of `true`. Sometimes, when you let the user log into a website, you don’t want the users to simply click back to last redirected URL. The back button (or history) must be cleared. Instead the only way forward for the user is to click logout. 

Get href path from event

```jsx
a.addEventListener("click", function (event) {
  event.preventDefault();
	let url1 = a.href;
	let url2 = a.getAttribute("href");
	let url3 = event.target.href;
	let url4 = event.target.getAttribute("href");
  console.log("Link Clicked");
});
```

1. The event listener is tied to anchor tag `a`.
2. `[event.target](http://event.target)` : The event happening here is the ******click.****** The target is the subject I subscribed to listened to *****click***** event. So, it’s the `event.target`.

<aside>
💡 The `href` property returns the full URL while the `getAttribute` method will only return the pathname if that's what's in the attribute.

</aside>

On the other hand,

```jsx
go: (route, addToHistory = true) => {
    console.log(`Going to ${route}`);
    if (addToHistory) {
      history.pushState({ route }, null, route);
    }
    let pageElement = null;
    switch (route) {
      case "/":
        pageElement = document.createElement("h1");
        pageElement.textContent = "Home";
        break;
      case "/order":
        pageElement = document.createElement("h1");
        pageElement.textContent = "Order";
        break;
    }
    if (pageElement) {
      // document.querySelector("main").children[0].remove();
      document.querySelector("main").innerHTML = "";
      document.querySelector("main").appendChild(pageElement);

			// reset the page (helpful when long pages of content)
      window.scrollX = 0;
      window.scrollY = 0;
    }
  },
```

`history.pushState(..)` shows an URL in the browser without really making an HTTP Request. But it can only alter the path. The 3rd argument is string that appears in the URL. 

# Web Components

A modular, reusable building block for web development that encapsulates a set of related functionality and user interface elements. In short, our won HTML tag element.

## Custom Elements

*We can define our own tags using Custom Elements API*

```jsx
class MyElement extends HTMLElement {
  constructor() {
    super();
  }
}
customElements.define("my-element", MyElement);
```

```html
<body>
 <my-element> /my-element>
</body>

<script>
document.createElement("my-element");
</script>
```

<aside>
💡 The HTML tag we define must contain a hyphen (-) to assure future compatibility

</aside>

Custom Elements with Attributes

```jsx
class MyElement extends HTMLElement {
  constructor() {
    super();
    this.dataset.language;
  }
}
customElements.define("my-element", MyElement);
```

```jsx
<body>
  <my-element data-language="en"></my-element>
</body>;
```

Custom Elements Lifecycle

We can override some methods of the super class

```jsx
class MyElement extends HTMLElement {
 constructor() { // Set up initial state, event listeners, etc.
 super();
 }

 connectedCallback() { } // The element is added to the document
 disconnectedCallback() { } // The element is removed to the document
 adoptedCallback() { }. // The element has been moved to a new document
 attributeChangedCallback(name, oldValue, new Value() { }

}

customElements.define("my-element", MyElement);
```

Custom Elements with Slots

```jsx
class MyElement extends HTMLElement {
  constructor() {
    super();
  }
}
customElements.define("my-element", MyElement);
```

```html
<body>
  <my-element>
    <div>
      <h2>Slot of My Element </h2> 
</div> 
</my-element> 
</body></h2>
    </div></my-element>
</body>
```

Custom Elements with Customized Built-INs

```jsx
class MyCustomElement extends HTMLElement {
  constructor() {
    super();
  }
}
customElements.define("my-element", MyElement);
```

```html
<div is="my-element"></div>
```

## HTML Template

- `<template>` is fundamentally ignored by the browser.
- Javascript can use the template to clone and render it to the DOM.

```html
<template id="template1">
 <header>
 <h1>This is a template </h1>
 <p>This content is not rendered initially </p>
 </header>
</template>
```

```jsx
connectedCallback() {
 const template = document.getElementById("template1");
 const content = template.content.cloneNode(true);
 this.appendChild(content);
}
```

We clone the template and we append it as a child; typically in connectedCallback method of the Custom Element

<aside>
💡 By default, the nodes of our custom element are part of the same page's DOM, so CSS style declaration applies to all the document.

</aside>

## Shadow DOM

A private, isolated DOM tree within a web component that is separate from the main document's DOM tree