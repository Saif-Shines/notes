# Intro to React v5

# Introduction

[Complete Intro to React v5](https://bit.ly/react-v5)

[btholt/complete-intro-to-react-v5](https://github.com/btholt/complete-intro-to-react-v5)

# Pure React

Reach is just another frontend library. You can include scripts just like anything else. 

For example, Let's see how we can do it.

```html
<script src="https://unpkg.com/react@16.8.0-alpha.1/umd/react.development.js"></script>
    <script src="https://unpkg.com/react-dom@16.8.0-alpha.1/umd/react-dom.development.js"></script>
```

Then in HTML index file,

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <meta http-equiv="X-UA-Compatible" content="ie=edge" />
    <link rel="stylesheet" href="./style.css" />
    <title>Adopt Me</title>
  </head>

  <body>
    <div id="root">not rendered</div>
    <script src="https://unpkg.com/react@16.8.0-alpha.1/umd/react.development.js"></script>
    <script src="https://unpkg.com/react-dom@16.8.0-alpha.1/umd/react-dom.development.js"></script>
    <script src="./App.js"></script>
  </body>
</html>
```

*Notes*

1. Look that `./App.js` is included after the react browser library scripts are imported.

On the other hand, You can write App.js

```jsx
const Pet = props => {
  return React.createElement("div", {}, [
    React.createElement("h1", {}, props.name),
    React.createElement("h2", {}, props.animal),
    React.createElement("h2", {}, props.breed)
  ]);
};

const App = () => {
  return React.createElement("div", {}, [
    React.createElement("h1", {}, "Adopt Me!"),
    React.createElement(Pet, {
      name: "Luna",
      animal: "Dog",
      breed: "Havanese"
    }),
    React.createElement(Pet, {
      name: "Pepper",
      animal: "Bird",
      breed: "Cockatiel"
    }),
    React.createElement(Pet, { name: "Doink", animal: "Cat", breed: "Mix" })
  ]);
};

ReactDOM.render(React.createElement(App), document.getElementById("root"));
```

**Notes**

1. After the App.js script is added in the index.html
    1. You see two   constants holding a return value of React.createElement()
    2. One is App and another one is Pet
2. You can call each one of these a Reach Component that is made up of html. Each html is created by React's createElement method of the module.
3. Interestingly can pass one Reach Component to React.createElement() method's first argument. 
    1. That first argument of React component can have html elements of which values can substituted before ReactDOM renders it.
    2. You can pass those values using React.createElement's second paramenters as arguments.
        1. See Pet function where props is a paramenter os passed arguments and values in which are being substituted.

# Tools

In your react development you might have to use a few tools that can help.

**.prettierrc**

**package.json**

```json
{} // means use default prettier settings set form vs code extension
```

.**gitignore**

```json
node_modules/
.DS_Store
.cache/
dist/
coverage/
.vscode/
.env
```

**.eslintrc.json**

```json
{
  "extends": [
    "eslint:recommended",
    "plugin:import/errors",
    "plugin:react/recommended",
    "plugin:jsx-a11y/recommended",
    "prettier",
    "prettier/react"
  ],
  "rules": {
    "react/prop-types": 0,
    "no-console": 1
  },
  "plugins": ["react", "import", "jsx-a11y"],
  "parserOptions": {
    "ecmaVersion": 2018,
    "sourceType": "module",
    "ecmaFeatures": {
      "jsx": true
    }
  },
  "env": {
    "es6": true,
    "browser": true,
    "node": true
  },
  "settings": {
    "react": {
      "version": "detect"
    }
  }
}
```

<aside>
💡 In the eslint rules, "rules" object — 0 is turn off — 1 is warn — 2 is error

</aside>

```json
{
  "name": "adopt-me",
  "version": "1.0.0",
  "description": "",
  "main": "index.js",
  "scripts": {
    "dev": "parcel src/index.html",
    "format": "prettier \"src/**/*.{js,html}\" --write",
    "test": "echo \"Error: no test specified\" && exit 1",
    "lint": "eslint \"src/**/*.{js,jsx}\" --quiet"
  },
  "keywords": [],
  "author": "",
  "license": "ISC",
  "devDependencies": {
    "babel-eslint": "^10.1.0",
    "eslint": "^7.8.1",
    "eslint-config-prettier": "^6.11.0",
    "eslint-plugin-import": "^2.22.0",
    "eslint-plugin-jsx-a11y": "^6.3.1",
    "eslint-plugin-react": "^7.20.6",
    "parcel-bundler": "^1.12.4",
    "prettier": "^2.1.1"
  },
  "dependencies": {
    "react": "^16.13.1",
    "react-dom": "^16.13.1"
  }
}
```

<aside>
💡  The difference between package.json and package-lock.json is that, package-lock.json as string and precise recording of current dev environment.

</aside>

<aside>
💡 The difference between linter and prettier is that, prettier focusses more on formatting, and linter focus more on what error might occur?

</aside>

# JSX

<aside>
💡 .jsx is a format where developers were allowed to write html inside of javascript. That's it, no more to that. But turns out now-a-days, Reach team basically tells there's no need to that. Actually, node is the one that recognises .jsx and handles them. But now, it's just encouraged to use .js itself.

</aside>

**.js**

**.jsx / .js**

```jsx
const Pet = props => {
  return React.createElement("div", {}, [
    React.createElement("h1", {}, props.name),
    React.createElement("h2", {}, props.animal),
    React.createElement("h2", {}, props.breed)
  ]);
};
```

.js

```jsx
const App = () => {
return React.createElement("div", {}, [
    React.createElement("h1", {}, "Adopt Me!"),
    React.createElement(Pet, {
      name: "Luna",
      animal: "Dog",
      breed: "Havanese",
    }),
    React.createElement(Pet, {
      name: "Pepper",
      animal: "Bird",
      breed: "Cocktail",
    }),
    React.createElement(Pet, {
      name: "Doink",
      animal: "Cat",
      breed: "Mixed",
    }),
  ]);
```

```jsx
function Pet({ name, animal, breed } = {}) {
  return (
    <div>
      <h1>{name}</h1>
      <h2>{animal}</h2>
      <h2>{breed}</h2>
    </div>
  );
}
```

- In the curly braces you can use any expresstion and ternary operators.

```jsx
const App = () => {

  return (
    <div>
      <h1 id="something-important">Adopt Me!</h1>
      <Pet name="Luna" animal="Dog" breed="Havanese" />
      <Pet name="Pepper" animal="Bird" breed="Cockatiel" />
      <Pet name="Doink" animal="Cat" breed="Mixed" />
    </div>
  );
};
```

# Hooks

```jsx
import React, { useState } from "react";

const SearchParams = () => {
  const [location, setLocation] = useState("Seattle,WA");
  return (
    <div className="search-params">
      <h1>{location}</h1>
      <form>
        <label htmlFor="location">
          Location
          <input
            id="location"
            value={location}
            placeholder="Location"
            onChange={(e) => setLocation(e.target.value)}
          />
        </label>
        <button>Submit</button>
      </form>
    </div>
  );
};

export default SearchParams;
```

IN THE PROCESS OF RE-RENDERING

1. useState hook has given setLocation with a call back.
2. Developer uses a an attribute of `onChange` and a js expression is written to capture the value entered on the keyboard by the user.
3. The retrun value is passed to setLocation callback that we received on a hook.
4. So when `<SearchParams />` is rerendered, the keyboard hit value will be appended to `location`

## Notes

- The code on the left demostrates my first interaction with hooks.
- Reach documentation calls it as "They let you use state and other React features without writing a class."
    - But from what I understood at this point,
        - *State* means the information that identifier contains at certain point of time. And it changes along with time. Previously, react developers used to write classes to do this. But now React began to give more functions which developers can handle the state.
        - Every react functoin which ever that starts with 'use' is called  a hook.
- In this example, `useState("Seattle, WA")` is a hook that gives back an array.
    - This array has two elements to it. One is argument that you passed in and another is a callback function.
    - We chose to call it `setLocation` here.
- Every time, use hits keyboard, an event is captured. ReactDOM then rerenders its components that is `< App />`
    - The way SearchParams components load is by following example,
    - ReactDOM gives access to render method. Render method takes two arguments.
        - First component is the React component which usually is returned by React.createElement(). But alternatively, developer can use composite components. That is using `<App />`
        - Second arguments is basically the DOM node where you want to render this component to. root is a div html element with same argument.
    
    ```jsx
    import React from "react";
    import { render } from "react-dom";
    import SearchParams from "./SearchParams";
    
    const App = () => {
      return (
        <div>
          <h1 id="something-important">Adopt Me!</h1>
          <SearchParams />
        </div>
      );
    };
    
    render(<App />, document.getElementById("root"));
    ```
    
    - So App will be rerendered so does SearchParams would be re-rendered.
    - Look on to your left — THE PROCESS OF RE-RENDERING

# Effects

useEffect() is the only life cycle method or hook that I have been introduced to in this course.

- It runs the first time the callback passed to it as first parameter.
- second parameter takes a array of arguments on asking the useEffect method's passed callback to run when one of those argument passed react components are subject to change.

```jsx
import React, { useState, useEffect } from "react";
import pet, { ANIMALS } from "@frontendmasters/pet";
import useDropdown from "./useDropdown";

const SearchParams = () => {
  const [location, setLocation] = useState("Seattle,WA");
  const [breeds, setBreeds] = useState([]);
  const [animal, AnimalDropdown] = useDropdown("Animal", "dog", ANIMALS);
  const [breed, BreedDropdown, setBreed] = useDropdown("Breed", "", breeds);

  useEffect(() => {
    setBreeds([]);
    setBreed("");

    pet.breeds(animal).then(({ breeds: apiBreeds }) => {
      const breedStrings = apiBreeds.map(({ name }) => name);
      setBreeds(breedStrings);
    }, console.error);

  }, [animal, setBreeds, setBreed]);

  return (
    <div className="search-params">
      <h1>{location}</h1>
      <form>
        <label htmlFor="location">
          Location
          <input
            id="location"
            value={location}
            placeholder="Location"
            onChange={(e) => setLocation(e.target.value)}
          />
        </label>
        <AnimalDropdown />
        <BreedDropdown />
        <button>Submit</button>
      </form>
    </div>
  );
};

export default SearchParams;
```

# DevTools

# Async & Routing

# Class Components

# Error Boundaries

# Context

# Portals

# Wrapping Up