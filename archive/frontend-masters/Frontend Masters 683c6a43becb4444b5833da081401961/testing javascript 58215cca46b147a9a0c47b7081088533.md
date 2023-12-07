# testing javascript

# Fundamentals of Testing in JavaScript

Let us understand the fundamentals of testing javascript by creating the problem ourselves. For example, look at the following module, which does some math.

 

```jsx
// math.js
// sum is intentionally broken so you can see errors in the tests
const sum = (a, b) => a - b
const subtract = (a, b) => a - b

// these are kinda pointless I know, but it's just to simulate an async function
const sumAsync = (...args) => Promise.resolve(sum(...args))
const subtractAsync = (...args) => Promise.resolve(subtract(...args))

module.exports = {sum, subtract, sumAsync, subtractAsync}
```

A simple test for it looks like →

```jsx
// index.js
const {sum, subtract} = require('./math')

let result, expected

console.log("Adjust math.js to pass the test.")
result = sum(3, 7)
expected = 10
if (result !== expected) { 
  throw new Error(`${result} is not equal to ${expected}`)
}

result = subtract(7, 3)
expected = 4
if (result !== expected) {
  throw new Error(`${result} is not equal to ${expected}`)
}
```

The above JS is interesting to us because:

1. You look from the perspective of test cases. In this case, `sum(3,7)` if we are passing to expect a wanted result. If it doesn't, we throw an error.
2. This is a classic example of writing tests — throw an error whenever the **result doesn't match the expected.** 

## Building an assertion library

So let's take the above code and build our own library in ways that hide many implementation details in a nice interface — `expect(..)` and `toBe(..)`

```jsx
const {sum, subtract} = require('./math')

let result, expected

result = sum(3, 7)
expected = 10
expect(result).toBe(expected)

result = subtract(7, 3)
expected = 4
expect(result).toBe(expected)

function expect(actual) {
  return {
    toBe(expected) {
      if (actual !== expected) {
        throw new Error(`${actual} is not equal to ${expected}`)
      }
    }
  }
}
```

1. `expect(..)` returns an object with `toBe(..)` property on it.
2. The value of `actual` is retained due to closure over `result` where invoked.
3. This simply only *asserts* on result/actual vs. expected

## Building Testing Framework

```jsx
const {sum, subtract} = require('./math')

test('sum adds numbers', () => {
  const result = sum(3, 7)
  const expected = 10
  expect(result).toBe(expected)
})

test('subtract subtracts numbers', () => {
  const result = subtract(7, 3)
  const expected = 4
  expect(result).toBe(expected)
})

function test(title, callback) {
  try {
    callback()
    console.log(`✓ ${title}`)
  } catch (error) {
    console.error(`✕ ${title}`)
    console.error(error)
  }
}

function expect(actual) {
  return {
    toBe(expected) {
      if (actual !== expected) {
        throw new Error(`${actual} is not equal to ${expected}`)
      }
    }
  }
}
```

1. When you have to test the programs which are quite long, we see errors at multiple places. We may have to drill down to what exactly went wrong and fix them.
2. Instead, we want to see all the tests alongside some title — *subtract, subtracts number,* or some helpful message.
3. So you pass execution contexts that have assertions in them as a callback to `test(title, callback)`

## Support Async tests with JS Promises

```jsx
const {sumAsync, subtractAsync} = require('./math') // Notice async functions are imported

test('sumAsync adds numbers asynchronously', async () => {
  const result = await sumAsync(3, 7)
  const expected = 10
  expect(result).toBe(expected)
})

test('subtractAsync subtracts numbers asynchronously', async () => {
  const result = await subtractAsync(7, 3)
  const expected = 4
  expect(result).toBe(expected)
})

async function test(title, callback) {
  try {
/** if not used '"**await"** callback()' , JS thread will invoke `callback()` and 
`console.log()` and leave the execution context. Instead we want `callback()`'s
execution is complete before `console.log()` is invoked.
*/
    await callback() 
    console.log(`✓ ${title}`)
  } catch (error) {
    console.error(`✕ ${title}`)
    console.error(error)
  }
}

function expect(actual) {
  return {
    toBe(expected) {
      if (actual !== expected) {
        throw new Error(`${actual} is not equal to ${expected}`)
      }
    }
  }
}
```

## Setting up utils as global

```jsx
// global.js 
async function test(title, callback) {
  try {
    await callback()
    console.log(`✓ ${title}`)
  } catch (error) {
    console.error(`✕ ${title}`)
    console.error(error)
  }
}

function expect(actual) {
  return {
    toBe(expected) {
      if (actual !== expected) {
        throw new Error(`${actual} is not equal to ${expected}`)
      }
    }
  }
}

global.test = test
global.expect = expect
```

### Running them

```bash
node --requrie abc/global.js testfile.js
```

Many libraries realize that many libraries will use utility functions like `test(..)` and `expect(..)` in every test case. So we can put them in global. At runtime,

 you can attach them by running `node --require` command followed by `global.js` that attaches `test` and `expect` to the global context. The result is, following files can use these functions from the global context.

# JavaScript Mocking Fundamentals

Mock functions allow you to test the links between code by erasing the actual implementation of a function, capturing calls to the function (and the parameters passed in those calls), capturing instances of constructor functions when instantiated with new, and allowing test-time configuration of return values.

The following are two pieces of code that we will use to understand some mocking fundamentals using Jest. The `utils.js` is simply a random function that returns the winner based on `Math. random()`. While on the other hand, `thumb-war.js` is really a function that takes in 2 arguments and returns a winner. To evaluate the winner, the `utils.js` returned function is used.

### utils.js

### thumb-war.js

```jsx
// returns the winning player or null for a tie
// Let's pretend this isn't using Math.random() but instead
// is making a call to some third party machine learning
// service that has a testing environment we don't control
// and is unreliable so we want to mock it out for tests.
function getWinner(player1, player2) {
  const winningNumber = Math.random()
  return winningNumber < 1 / 3
    ? player1
    : winningNumber < 2 / 3
      ? player2
      : null
}

module.exports = { getWinner }
```

```jsx
const utils = require('./utils')

function thumbWar(player1, player2) {
  const numberToWin = 2
  let player1Wins = 0
  let player2Wins = 0
  while (player1Wins < numberToWin && player2Wins < numberToWin) {
    const winner = utils.getWinner(player1, player2)
    if (winner === player1) {
      player1Wins++
    } else if (winner === player2) {
      player2Wins++
    }
  }
  return player1Wins > player2Wins ? player1 : player2
}

module.exports = thumbWar
```

## Monkey Patching

```jsx
// monkey-patching.js
const assert = require('assert');
const thumbWar = require('../thumb-war.js');
const utils = require('../utils');

const originalGetWinner = utils.getWinner;
utils.getWinner = (p1,p2) => p1;

const winner = thumbWar('Kent', 'Saif');
assert.strictEqual(winner, 'Kent');

//clean up
utils.getWinner = originalGetWinner;
```

## Ensure Functions are Called Correctly with JavaScript Mocks

### With Jest

### Without Jest

```jsx
const thumbWar = require('../thumb-war');
const utils = require('../utils');

test('returns winner',() => {
	const originalGetWinner = utils.getWinner;
	utils.getWinner = jest.fn((p1,p2) => p1); // jest.fn(..) observes passed in mock implementation

	const winner = thumbWar('Kent', 'Saif');
	expect(winner).toBe('Kent');
	expect(utils.getWinner.mock.calls).toEqual([['Kent', 'Saif'], ['Kent', 'Saif']]); // getWinner might've been called two times within thumbWar fn.
	
  // the above assertion in reality replaces the following:
	expect(utils.getWinner).toHaveBeenCalledTimes(2);
	expect(utils.getWinner).toHaveBeenNthCalledWith(
    1,
    'Kent C. Dodds',
    'Ken Wheeler'
  )
  expect(utils.getWinner).toHaveBeenNthCalledWith(
    2,
    'Kent C. Dodds',
    'Ken Wheeler'
  )

  // cleanup
  utils.getWinner = originalGetWinner
});
```

```jsx
const assert = require('assert');
const thumbWar = require('../thumb-war');
const utils = require('../utils');

const originalGetWinner = utils.getWinner;
utils.getWinner = fn((p1,p2) => p1);

const winner = thumbWar('Kent','Saif');
assert.strictEqual(winner, 'Kent');
assert.deepStrictEqual(utils.getWinner.mock.calls, [
	['Kent', 'Saif'],
	['Kent', 'Saif']
])

function fn(impl){
	const mockFn = (...args) => {
		mockFn.mock.calls.push(args);
		return impl(...args);
	}

	mockFn.mock = { calls: [] }
	return mockFn;
}

utils.getWinner = originalGetWinner;
```

 

## Restore the Original Implementation of a Mocked JavaScript Function with jest.spyOn

### with Jest

### without Jest

```jsx
const thumbWar = require('../thumb-war');
const utils = require('../utils');

test('returns winner', () => {
	jest.spyOn(utils, 'getWinner');
	utils.getWinner.mockImplementation((p1,p2) => p1);

	const winner = thumbWar('Kent', 'Saif');
	expect(winner).toBe('Kent');
	expect(utils.getWinner.mock.calls).toEqual([
		['Kent', 'Saif'],
		['Kent', 'Saif']
	])
	
  //clean up
	utils.getWinner.mockRestore();
})
```

```jsx
const assert = require('assert');
const thumbWar = require('../thumb-war');
const utils = require('../utils');

function fn(impl = () => {}){
	const mockFn = (...args) => {
		mockFn.mock.calls.push(args);
		return impl(...args);
	}

	mockFn.mock = { calls: [] };	
	mockFn.mockImplementation = newImpl => (impl = newImpl)
	return mockFn;
}

function spyOn(obj,prop){
	const originalValue = obj[prop];
	obj[prop] = fn();
	obj[prop].mockRestore = () => (obj[prop] = originalValue);
}

spyOn(utils, 'getWinner');
utils.getWinner.mockImplementation((p1, p2) => p1)

const winner = thumbWar('Kent C. Dodds', 'Ken Wheeler')
assert.strictEqual(winner, 'Kent C. Dodds')
assert.deepStrictEqual(utils.getWinner.mock.calls, [
  ['Kent C. Dodds', 'Ken Wheeler'],
  ['Kent C. Dodds', 'Ken Wheeler']
])

utils.getWinner.mockRestore();
```

 

## Mock a user module inline

### with Jest

### without Jest

```jsx
const thumbWar = require('../thumb-war');
const utilsMock = require('../utils');

jest.mock('../utils', () =>{
	return {
		getWinner: jest.fn((p1,p2) => p2);
	}
});

test('returns winner', () => {
	const winner = thumbWar('Kent C. Dodds', 'Ken Wheeler');
	expect(winner).toBe('Kent C. Dodds');
	expect(utilsMock.getWinner.mock.calls).toEqual([
		['Kent C. Dodds', 'Ken Wheeler'],
    ['Kent C. Dodds', 'Ken Wheeler']
	])

	utilsMock.getWinner.mockReset();
})
```

```jsx
function fn(impl = () = {}){
	const mockFn = (...args) => {
		mockFn.mock.calls.push(args);
		return impl(...args);
	}
	mockFn.mock = { calls: []}
	return mockFn;
}

const utilsPath = require.resolve('../utils');
require.cache[utilsPath] = {
	id: utilsPath,
	filename: utilsPath,
	loaded: true,
	exports: {
		getWinner: fn((p1,p2) => p1)
	}
}

const assert = require('assert');
const thumbWar = require('../thumb-war');
const utils = require('../utils');

const winner = thumbWar('Kent', 'Saif');
assert.strictEqual(winner, 'Kent');
assert.deepStrictEqual(utils.getWinner.mock.calls, [
  ['Kent C. Dodds', 'Ken Wheeler'],
  ['Kent C. Dodds', 'Ken Wheeler']
])

// cleanup
delete require.cache[utilsPath]
```

## Intro to Static Analysis Testing JavaScript Applications

### Review  ESLint

ESLint is a command-line tool that is also available as an npm package for most of the web applications out there to do some static analysis and prevent bugs in the future. 

- You can install eslint by `npm install --save-dev eslint` in a node project.
- Once the eslint is in the toolchain, you can look at the defined configuration using a `.eslintrc` file in order to check for warnings and errors in our codebase.
- You can also run `npx eslint .` command to check for errors and warnings as specified in `.eslintrc` file.

The following is a example configuration.

```jsx
{
  "parserOptions": {
    "ecmaVersion": 2019,
    "sourceType": "module",
    "ecmaFeatures": {
      "jsx": true
    }
  },
  "rules": {
    "strict": ["error", "never"],
    "valid-typeof": "error",
    "no-unsafe-negation": "error",
    "no-unused-vars": "error",
    "no-unexpected-multiline": "error",
    "no-undef": "error"
  },
  "env": {
    "browser": true
  }
}
```

- The `"strict"` rule basically says the codebase doesn't need to `use strict` in each and every user module. We use this rule because babel will automatically add this line for us in the toolchain.

You can find the definitions of most of the rules looking at their documentation

[ESLint - Pluggable JavaScript linter](https://eslint.org/)

### eslint recommended

eslint allows some soft of recommended rules to be applied on our code base. the following is how you can do it.

All the `"rules"` will override recommended errors

```jsx
{
  "parserOptions": {
    "ecmaVersion": 2019,
    "sourceType": "module",
    "ecmaFeatures": {
      "jsx": true
    }
  },
 "extends": ["eslint:recommended"],
  "rules": {
    "strict": ["error", "never"]
  },
  "env": {
    "browser": true
  }
}
```

### ignoring linting a few files

In some cases when you run `eslint .` all the directories even with the build files will be check for lint errors. That throws a lot problems. For example, if you are using babel, then all the lint rules will be checked against transpiled babel files or build files as well. We don't want that.

There are two ways you can overcome this

1. You can add a `.eslintignore` file and then mention the directories that you don't want to lint.
2. Or you can do `eslint --ignore-path .gitignore .`  which will ignore whatever that is in `.gitignore` as well.

Finally, add that as a `"lint"` script.

## Prettier

```bash
npx prettier src/example.js # doesn't write to files but simply logs correct files

npx prettier src/example.js # re writes the code file
```

### adding npm script for prettier

```json
"scripts": {
    "build": "babel src --out-dir dist",
    "lint": "eslint --ignore-path .gitignore .",
    "format": "prettier --ignore-path .gitignore --write \"**/*.+(js|json)\""
  },
```

1. The `--ignore-path` works the same way as it does for lint
2. `**/*.+(js|json)` is called a glob that matches patterns

### prettier playgound

[Prettier](https://prettier.io/playground/)

Copy and paste your code in the playground. Once you check everything looks right, you can copy the config file and past it in `.prettierrc` file.

### Conflicts between Prettier and ESLint

1. install a another dependence `npm i --save-dev eslint-config-prettier`
2. and add an extends to `.eslintrc` - `"extends": ["eslint:recommended", "eslint-config-prettier"],`

**More on scripts and validation**

```json
{
  "name": "static-testing-tools",
  "private": true,
  "author": "Kent C. Dodds (http://kentcdodds.com/)",
  "license": "GPLv3",
  "scripts": {
    "build": "babel src --out-dir dist",
    "lint": "eslint --ignore-path .gitignore .",
    "format": "prettier --ignore-path .gitignore --write \"**/*.+(js|json)\"",
    "check-format": "prettier --ignore-path .gitignore --list-different \"**/*.+(js|json)\"",
    "validate": "npm run check-format && npm run lint && npm run build"
  },
  "devDependencies": {
    "@babel/cli": "^7.7.0",
    "@babel/core": "^7.7.2",
    "@babel/preset-env": "^7.7.1",
    "eslint": "^7.31.0",
    "eslint-config-prettier": "^8.3.0",
    "prettier": "^2.3.2"
  }
}
```

1. The `validate` command checks if there are lining errors or there's code that doesn't follow prettier rules
2. The way we will see if there's code that doesn't follow prettier rules is by that `--list-different` flag instead of `--write`
3. Now if there are any issues you can run `npm run format` for the prettier to automatically rewrite for you.

There a lots of commanatiliy between the prettier related scripts so we can shorten it

```json
{ 
"prettier":"prettier --ignore-path .gitignore \"**/*.+(js|json)\""
"format": "npm run prettier -- --write",
"check-format": "npm run prettier -- --list-different",
}
    
```

## Type checking with typescript

1. Install typescript using `npm i --save-dev typescript`
2. convert `.js` to `.ts` file. That also means you will need to write the code in typescript. Defining types and interfaces.
    - The js code
        
        ```json
        function add(a, b) {
          return a + b
        }
        
        function getFullName(user) {
          const {
            name: {first, middle, last},
          } = user
          return [first, middle, last].filter(Boolean).join('')
        }
        
        add(1, 'two')
        
        getFullName({name: {first: 'Joe', midd1e: 'Bud', last: 'Matthews'}})
        
        ```
        
    - the ts code
        
        ```tsx
        function add(a: number, b: number): number {
          return a + b
        }
        
        interface User {
          name: {
            first: string
            middle: string
            last: string
          }
        }
        function getFullName(user: User): string {
          const {
            name: {first, middle, last},
          } = user
          return [first, middle, last].filter(Boolean).join(' ')
        }
        
        add(1, 2)
        
        getFullName({name: {first: 'Joe', middle: 'Bud', last: 'Matthews'}})
        ```
        
3. if you look at `/bin` within node modules, you will find `tsc` binary. You can actually run `npx tsc` but first time it will throw an error because you did not mention any configuration yet.
4. So create `tsconfig.json` 
    
    ```json
    {
      "compilerOptions": {
        "noEmit": true, # to say, don't compile it. we are already using babel to compile.
        "baseUrl": "./src" # obviously the directory.
      }
    }
    ```
    
5. Well, now when you run `npx tsc` you will see type check errors in your console.

### Settling

```json
"scripts": {
    "build": "babel src --extensions .js,.ts,.tsx --out-dir dist",
    "lint": "eslint --ignore-path .gitignore .",
    "check-types": "tsc",
    "format": "prettier --ignore-path .gitignore --write \"**/*.+(js|json|ts|tsx)\"",
    "check-format": "prettier --ignore-path .gitignore --list-different \"**/*.+(js|json|ts|tsx)\"",
    "validate": "npm run check-types && npm run check-format && npm run lint && npm run build"
  },
```

1. You will suddenly notice `--extensions .js,.ts,.tsx` because we are telling babel to see those files. And also install `npm install --save-dev @babel/preset-typescript` because by default babel doesn't know how to transpile typescript.
    - Also update the `.babelrc`
        
        ```json
        {
          "presets": [
            [
              "@babel/preset-env",
              {
                "targets": {
                  "node": "10"
                }
              }
            ],
            "@babel/preset-typescript"
          ]
        }
        ```
        

Finally,

```json
{
  "name": "static-testing-tools",
  "private": true,
  "author": "Kent C. Dodds (http://kentcdodds.com/)",
  "license": "GPLv3",
  "scripts": {
    "build": "babel src --extensions .js,.ts,.tsx --out-dir dist",
    "lint": "eslint --ignore-path .gitignore .",
    "check-types": "tsc",
    "prettier": "prettier --ignore-path .gitignore \"**/*.+(js|json|ts|tsx)\"",
    "format": "npm run prettier -- --write",
    "check-format": "npm run prettier -- --list-different",
    "validate": "npm run check-types && npm run check-format && npm run lint && npm run build"
  },
  "devDependencies": {
    "@babel/cli": "^7.7.0",
    "@babel/core": "^7.7.2",
    "@babel/preset-env": "^7.7.1",
    "@babel/preset-typescript": "^7.7.2",
    "eslint": "^6.6.0",
    "eslint-config-prettier": "^6.5.0",
    "prettier": "^1.19.1",
    "typescript": "^3.7.2"
  }
}
```

### Make ESLint Support TypeScript Files

Basically install two more dependencies and extend them in eslintrc files specifically.

[Comparing tjs/step-09...tjs/step-10 · kentcdodds/static-testing-tools](https://github.com/kentcdodds/static-testing-tools/compare/tjs/step-09...tjs/step-10)

### Run npm scripts to be run parllely

[Comparing tjs/step-10...tjs/step-11 · kentcdodds/static-testing-tools](https://github.com/kentcdodds/static-testing-tools/compare/tjs/step-10...tjs/step-11)

# Intro to Use DOM Testing Library to test any JS framework

This is wasn't of much help. Kent shows some of the ways you can use same the testing library to test JS frameworks.

# Jest for JS Apps

The source code on which I am going to run tests is a calculator app. You can simply start a server and see what it does for yourself.

You don't need a config file to run tests after jest is installed a dev dependency. All that is needed is to match a file pattern or glob for the jest to run tests.

```json
npm i -D jest # installs jest
```

1. This simply installs `jest` to my project dependencies.
2. You can add `npm test` to one of the `validate` commands
    
    ```json
    "validate": "npm run lint && npm run test && npm run build",
        "setup": "npm install && npm run validate"
    ```
    

### Compile Modules with Babel in Jest Tests

[Comparing tjs/jest-01...tjs/jest-02 · kentcdodds/jest-cypress-react-babel-webpack](https://github.com/kentcdodds/jest-cypress-react-babel-webpack/compare/tjs/jest-01...tjs/jest-02)

1. In our project we seem to use both *babel* and *webpack*.
    1. Babel transpiles the code and webpack bundles the code. 
2. If you simply install Jest and start running the tests, the tests will fail. Because Jest looks at Babelrc file as well. Initially we asked Babel to not to [transpile esmodule](https://github.com/kentcdodds/jest-cypress-react-babel-webpack/compare/tjs/jest-01...tjs/jest-02#diff-b417af757044d72f7b8d629728f646d5cf42b8be85ace886526f50bb8c7645a7L5) in ways browser will understand, because we had webpack taking care of it.
3. So if we tell babelrc to transpile, jest will read the it and and run tests on transpiled code and the test will pass. But we loose the benefit of tree shaking with webpack. So why not just transpile in test env and not prod? [So we do that](https://github.com/kentcdodds/jest-cypress-react-babel-webpack/compare/tjs/jest-01...tjs/jest-02#diff-b417af757044d72f7b8d629728f646d5cf42b8be85ace886526f50bb8c7645a7R6).
4. Jest will automatically assin `process.env.NODE_ENV`  to `test`
5. Now the tests pass and we retain the benefit of webpack treeshaking.

### Configure Jest's Test Env for Testing Node and Browser Code

When you start running your tests, Jest will use Node environment to test your units. But how do you test the JS that will rely on browser APIs? You have to tell Jest to create environment as well

1. In the root dir of your projects create `jest.config.js`
2. So you will end up creating 
    
    ```json
    module.exports = {
      testEnvironment: 'jest-environment-jsdom',
    }
    ```
    
3. This will automatically install a dependency called `jest-environment-json` in your node modules.

### Forumla to write each test

```json
1. Arrage
2. Act
3. Assert
```