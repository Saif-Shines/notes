# Complete Intro to React v6

[btholt/complete-intro-to-react-v6](https://github.com/btholt/complete-intro-to-react-v6)

# Prettier

If your project needs a code formatted as a development dependency for your project, `prettier` is a very nice tool. You can install it as an npm package by `npm i -D prettier`

*How do you let the prettier know which files to automatically format?*

1. Create a `.prettierrc` file in the root directory. This will tell npm module's bin prettier file to read what's in `.prettierrc`
2. Just mention, `{}` . This tells the prettier `bin` executable to use default configuration.
3. Add an npm script to format if needed by 
    
    ```json
    "scripts": {
        "format": "prettier --write \"src/**/*.{js,jsx}\""
      },
    ```
    

# ESLint

Configuring `.eslintrc.json`

```json
{
  "extends": ["eslint:recommended", "prettier"],
  "plugins": [],
  "parserOptions": {
    "ecmaVersion": 2021,
    "sourceType": "module",
    "ecmaFeatures": {
      "jsx": true
    }
  },
  "env": {
    "es6": true,
    "browser": true,
    "node": true
  }
}
```

npm script

```json
"scripts": {
    "format": "prettier --write \"src/**/*.{js,jsx}\"",
    "lint": "eslint \"src/**/*.{js,jsx}\" --quiet"
```

# All the events in react

[SyntheticEvent - React](https://reactjs.org/docs/events.html#supported-events)