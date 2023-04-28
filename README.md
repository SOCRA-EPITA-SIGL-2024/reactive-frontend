# Reactive Frontend Workshop

This workshop is made for students of EPITA - SIGL 2024.

The aim of the workshop is to implement **your** reactive user interface (UI) for Socarotte.

To implement it, we will use:

- [NodeJS](https://nodejs.org/en/about/): to be able to use dependencies (other developper's code)
- [ReactJS](https://fr.reactjs.org/): to manage DOM elements rendering and local component state
- [ViteJS](https://vitejs.dev/): to bundle all our source files into a single `JavaScript` file (that will be our production artifact).

> Note: We are doing only the user interface without any data persistency...for now!

## Step 1: Add `React` and `Vite`

Sofar, we have a responsive application, but we don't handle any user interactions (menu navigation or button clicks).

[ReactJS](https://fr.reactjs.org/) is a JavaScript framework from Facebook.

We chose this since it seems to be the most notorious one, when checking [NodeJS framework trends](https://www.npmtrends.com/@angular/core-vs-angular-vs-react-vs-vue-vs-svelte) with its competitors.

### Install the provided template

To avoid tons of copy / paste, we've prepared a small template project to start with `React` and  `Vite`.

Follow the README.md instructions on the provided template [SOCRA-EPITA-SIGL-2024/react-vite-template](https://github.com/SOCRA-EPITA-SIGL-2024/react-vite-template)

Once done, you should have the following tree of files:

```plain
frontend/
├── .gitignore
├── index.html
├── node_modules
│   ├── ...
├── .nvmrc
├── package.json
├── package-lock.json
├── public
│   └── favicon.ico
├── src
│   ├── App.css
│   ├── App.jsx
│   └── main.jsx
└── vite.config.js

59 directories, 11 files
```

> Note: if you have an issue running `nvm use`, make sure node v19 is installed on your machine.
> To install it `nvm install v19`

### Create your App component

**Objective**: Replace this `Hello SIGL` with **your** application from the the template with your version of Socarotte from [responsive-frontend workshop](https://github.com/SOCRA-EPITA-SIGL-2024/responsive-frontend).

You already some CSS imported in the `frontend/src/App.jsx` component by the line

```jsx
import "./App.css";
```

For this step, you can put all your CSS code in this `frontend/src/App.css` file.

React `JSX` format is very very close to regular HTML, except attribute are turn in caml case (e.g. `colspan` in html would be `colSpan`), and the `class="mystyle"` attribute is `className="mystyle"` in JSX.

Read more about [how to convert JSX to HTML on react website](https://react.dev/learn/writing-markup-with-jsx#converting-html-to-jsx)

> Note: There is even an online converter to convert your HTML to JSX directly: [html-to-jsx](https://transform.tools/html-to-jsx)

Try to copy paste the content of your `<body></body>` from **your** [responsive-frontend workshop implementation](https://github.com/SOCRA-EPITA-SIGL-2024/responsive-frontend) as the return of your `function App() { ... }` in `frontend/src/App.jsx`:

```jsx
import React from "react";
import "./App.css";

function App() {
  return (
    <div className="app">
      <nav className="menu-horizontal">
        ...
      </nav>
      <div className="banner">
        ...
      </div>
      <h1>Promotions</h1>
      <div className="deals">
        ...
      </div>
      <h1>Produits</h1>
      <div className="products">
        <nav className="menu-vertical">
          ...
        </nav>
        ...
      </div>
    </div>
  );
}

export default App;
```

> Note: React components, like your `function App`, needs to return only **one** JSX element, with as many children as you like. You can use the React fragment component `<> ... </>`if you do not want to add another div:

```jsx
    function App() {
        return (
            <>
                <h1>Socarotte<h1>
                <div>...</div>
            </>
        )
    }
```

If you're stuck, feel free to look at the [SOCRA-EPITA-SIGL-2024/frontend-template (react-vite)](https://github.com/SOCRA-EPITA-SIGL-2024/frontend-template/tree/react-vite).

> Note: It's similar to what you have done, but without the `frontend/` directory.

You should now see same page from **your** [responsive-frontend workshop implentation](https://github.com/SOCRA-EPITA-SIGL-2024/responsive-frontend) but using React, congrats!

## Step 2: Make the menu reactive

You will use [react-router-dom (v6)](https://reactrouter.com/en/6.10.0/start/tutorial) JavaScript library to implement it.

- Install `react-router-dom` in your `frontend/` project:

```sh
# from frontend/ directory
nvm use
npm install --save react-router-dom
```

Follow the nice tutorial on [react router documentation](https://reactrouter.com/en/main/start/tutorial) to get familiar with basic concepts.

### Split your components

Reorganize your code to split this big `function App () { ... }` React component into several different React components.

Follow the documentation on [React components](./how-to/REACT-COMPONENTS.md) to know more about React components.

### Implement navigation using react-router

Feel free to look at the [SOCRA-EPITA-SIGL-2024/frontend-template (component-split)](https://github.com/SOCRA-EPITA-SIGL-2024/frontend-template/tree/component-split) to see how the template uses react router.

- [How to use the `createBrowserRouter` in the template](https://github.com/SOCRA-EPITA-SIGL-2024/frontend-template/blob/ccba9c8e16f5559bd0af32db27d4065e3bce8310/src/Layout.jsx#L8)
  - [How to use the created browser router with the `RouterProvider`](https://github.com/SOCRA-EPITA-SIGL-2024/frontend-template/blob/component-split/src/App.jsx)
- [How to navigate using navigation links](https://github.com/SOCRA-EPITA-SIGL-2024/frontend-template/blob/component-split/src/MainMenu.jsx) with [NavItem custom component (only applying css if the current link is active)](https://github.com/SOCRA-EPITA-SIGL-2024/frontend-template/blob/component-split/src/NavItem.jsx)

## Step 3: Add state management to your app

**Objective** have several React components reacting on the same action

Several options are available when it comes to state management: [Redux (for React)](https://react-redux.js.org/), [XState](https://xstate.js.org/docs/recipes/react.html) or the [React Context](https://react.dev/reference/react/useContext).

You will use the `React context` to implement state management in your frontend.

Since its implementation is not so trivial, we are providing you the code.

Add a new file `frontend/src/AppContext.jsx` with the following code:

```jsx
import React from "react";

export const initialState = {
  basket: [],
};

export const reducer = (state, action) => {
  switch (action.type) {
    // This `case` is an example of how you can
    // reduce an `action` that adds a new `item` to 
    // the `state.basket` array
    case "NEW_BASKET_ITEM":
      const { item } = action;
      return {
        ...state,
        basket: [...state.basket, item],
      };
    default:
      return state;
  }
};

export const AppContext = React.createContext();

function useAppContext() {
  return React.useContext(AppContext);
}

export default useAppContext;
```

And initalize the `AppContext` in your root component (e.g. `App` in the template):

```jsx
import React from "react";
import {AppContext, reducer, initialState} from "./AppContext.jsx"
// ...

function App() {
  const [state, dispatch] = React.useReducer(reducer, initialState);

  return (
    <AppContext.Provider value={{ state, dispatch }}>
      {/* ... */}
    </AppContext.Provider>
  );
}

export default App;
```

Follow the documentation provided [how to use React context](./how-to/REACT-CONTEXT.md) to know how to use the `useAppContext` hooks.

If for some of your component you need local state management, see the [How to use React.useState documentation provided in this workshop](./how-to/REACT-USE-STATE-HOOK.md)

> This documentation explains how to implement the `Add to basket` functionality; which is the objective of the [Challenge 1](#challenge-1-add-to-basket)

## Challenge 1: Add to basket

**Objective** Integrate the `Add to basket` functionality describe in the [example section of the how to use React context](./how-to/REACT-CONTEXT.md#example-add-to-basket-feature)

**Important**: For the grading tool, you will add the following `props` to the following React elements:

- `socra="add-to-basket"` on the `<button></button>` element that is use to add an item to the basket
- `socra="basket-nav-link"` on the `<NavLink></NavLink>` (or equivalent) menu link of the basket in the main navigation
- `socra="basket"` on the element that renders `items` in the basket, when navigating to the basket view

> Important: verify that those `socra="..."` tags appears in the html rendered by inspecting the web page and look at the html document.

## Challenge 2: Remove from basket

Implement the following feature.

**Feature**: When a user selects the `delete` icon of the basket table:

- the item gets removed from the basket table or list (however you've decided to display items in basket)
- the main navigation's item number is decreased by 1 (e.g. from `(2)` to `(1)` or from `(1)` to nothing)
- the product becomes enabled again to be put in the basket

**Important**: For the grading tool, you will add the following `props` to the following React elements:

- `socra="remove-item"` to the `delete` icon / button in the basket view.

> Important: verify that those `socra="..."` tags appears in the html rendered by inspecting the web page and look at the html document.
