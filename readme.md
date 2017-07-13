# Intro to Redux

## Learning Objectives
  - Identify when there might be a true need for redux
  - Use React-Redux to centralize and manage application-state
  - Explain the importance of unidirectional data-flow and immutability for Redux's event history
  - Explain the role of the store, reducers, and actions
  - Describe how Redux influences the architecture of a React app
  - Describe application-state in terms of plain JavaScript objects `{}`

# What is Redux? (0:05, 5 min)

We've begun to think in React, breaking down a UI into components. We've also been introduced to the idea of different types of components--often, components will have `state`, though not all components will.

However, if we had a lot of stateful components, we can easily imagine the stress of having to be aware of the state of say even a dozen components. This really begins to become an issue particularly for teams of developers, as their applications scale, or increase in complexity and size.

Each component could potentially have its own state, creating a stressful scenario where we might be tempted to litter our applications with `console.log()` statements. This approach does not scale very well, is cumbersome, gives us limited insight into our applications, and creates a lot of noise in the browser console. Using `console.log()` has its place but Redux offers us a much better solution to the problem of having fragmented or decentralized application-state.

Redux solves this problem by centralizing and managing state. It does so by funneling component states into a central hub.

It's essentially a state management library that can be used in pretty much any JavaScript front-end views library or framework. Redux is extremely opinionated and entails writing applications with significant limitations. These limits imposed by Redux don't restrict what you write, so much as *how* you write it.

There is also an extremely useful [Redux Devtools Chrome Extension](https://chrome.google.com/webstore/detail/redux-devtools/lmhkpmbekcpmknklioeibfkpmmfibljd?hl=en), which gives us the ability to see and interact with the data in our programs as if it were a movie we could pause, play, rewind, and fast-forward.

**Take a moment to install the [Redux Devtools Chrome Extension](https://chrome.google.com/webstore/detail/redux-devtools/lmhkpmbekcpmknklioeibfkpmmfibljd?hl=en)**

# Use Case (0:15, 10 min)

Let's take 5 minutes to read through a blog post written by the creator of Redux, Dan Abramov.

 From [***You Might Not Need Redux***](https://medium.com/@dan_abramov/you-might-not-need-redux-be46360cf367#.47e1bpmhj):

> Redux offers a tradeoff. It asks you to:
  - Describe **application state as plain objects and arrays**.
  - Describe **changes** in the system as **plain objects**.
  - Describe **the logic for handling changes as pure functions**.

It is definitely not necessary to involve in every app, especially when still learning React. The purpose of today's lesson is not to impress upon you that must and should use Redux to build your React application, but to show how Redux works through acquainting you with its concepts and components.

The blog post lists use cases for Redux, features that Redux provides to the application developer, and the implementation limitations that Redux imposes.

> However, if you’re just learning React, don’t make Redux your first choice.
Instead learn to [think in React](https://facebook.github.io/react/docs/thinking-in-react.html). Come back to Redux if you find a real need for it, or if you want to try something new. But approach it with caution, just like you do with any highly opinionated tool.

For this lesson, we're throwing caution (or a regard for our own momentary sanity) to the wind. There is certainly a learning curve with Redux, but the exposure is valuable for a few reasons:

1. It is being increasingly adopted by React developers, especially teams working on very large applications.

2. Working with Redux exposes us to very elegant applications of functional programming and convey its power and potential.
Functional programming presents us with the challenge of having to think in new ways about how our programs are composed and pass data.

3. Its limitations force the developer to very carefully consider the structure of their application.

---

# Functional Programming and the React Ecosystem (0:25, 10 min)

You may hear developers talking about how functional programming is revolutionizing JavaScript and wonder how this is so.
Let's revisit the concept of a pure function: given any input, a ***pure function*** will return the exact same output.
It will also ***have no side effects***.
This means that the **pure function** is *only concerned* with returning some output that is a function of its input.

Part of the power of React is this very idea applied to views. We pass in props to a component, and we get the same predictable component every time.

In short, applying concepts of functional programming to a front-end library ensure ***a high degree of predictability***.
It also creates a modular architecture that allows a library like Redux to intervene in how React manages state.

Redux will use functional programming's approach to function composition: reusing certain functions in the construction of other functions.
Ultimately, these aggregated functions will provide the functionality of Redux.

# Concepts of Redux

## Application State &  Immutability in Redux (0:40, 15 min)

Simply put, state is a representation of your application's data. Redux manages your application's state, encapsulating the data stored in your variables, in something called **The Store**.
When using Redux, we want to treat application state in such a way that it is always ***copied*** and never directly mutated.
The end result of this approach is that we get a history of the application's states over the course of its usage.
This affords a great resource for developers debugging a complex user-interface, for example.
A developer using Redux in this way could see the exact series of user actions that produces the bug.


### Techniques for Avoiding the Mutation of State

<!-- TODO: Create immutability technique markdown reference and transfer this info there -->

ES6 has a couple nice features that make dealing with immutable data easier: `Object.assign()` and the spread operator `...`.

`Object.assign()` in particular gives us a powerful way of copying objects.

> Example:
```js
let cat = {
  name: "Meowy Mandel",
  meowy: true
}
let copiedCat = Object.assign({}, cat)
```

`...` or the spread operator gives us a nice way of combining arrays.
It exposes or "unwraps" the values in an array.

```js
let fruits = ["Tomato", "Cucumber", "Pumpkin"]
let updatedFruits = [...fruits, "Avocado"]
// => ["Tomato", "Cucumber", "Pumpkin", "Avocado"]
```

The above code is equivalent to this:

```js
let fruits = ["Tomato", "Cucumber", "Pumpkin"]
let updatedFruits = fruits.concat("Avocado")
// => ["Tomato", "Cucumber", "Pumpkin", "Avocado"]
```

For arrays of primitives, we have the good, old reliable `.slice()` (first implemented in ES3) for copying arrays (either in whole or part).

```js
let fruits = ["Tomato", "Cucumber", "Pumpkin"]
let copiedFruits = fruits.slice()
// => ["Tomato", "Cucumber", "Pumpkin"]
let firstTwoFruits = fruits.slice(0, 2)
// => ["Tomato", "Cucumber"]
```


For arrays of objects, you can use `map()` in concert with `Object.assign()`

> Example:

```js
let todos = [
    {todo: "learn to thrash"},
    {todo: "learn redux"},
    {todo: "hang tight"},
    {todo: "stay loose"}
]
let todosCopy = todos.map((obj) => {
  return Object.assign({},obj)
})
```


##### Remember, `.slice()` not `.splice()`!


[Splice](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/splice)
[Slice](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/slice)

It's an easy mistake to make, since there is a one letter difference.

`.splice()` ***is a mutator method***, meaning it modifies whatever it is called on.

`.slice()` ***is not a mutator method***. Use it for ***copying*** all or part of an array!


---

# Elements of Redux (1:00, 20 min)

## The Store

The store is a kind of hub that all the information (**application state**) in a program flows through. **The store** encapsulates not only the data in the program, but also controls the flow of program data, storing each change in a separate state. Redux even gives us the ability to time travel through our application's history of application states. It's like the principles of git applied to application-state rather than file-state.

All the principles of Redux are embodied in the store. The store  holds an application's **states** (including current and previous states), **actions** which specify different changes to make on some part of the application state, and **the reducer**, which specifies which actions update the state object.

Since state is being represented as an immutable data-structure, we cannot directly modify it.
Changing state in the program requires dispatching an action that modifies a copy of the state.
This becomes the next state of the program, spat out by the reducer.

Every time an action has been dispatched via the reducer, we want to update the UI. So we subscribe the render method to any changes taking places to the application's state object.


## Actions

An action is a garden-variety JavaScript object that describes what kind of change is to take place, specifying what change to make to what data.

The minimum requirement for an action is that the action must have a type property that **is not** `undefined.`
<details>
  <summary>
  Unless you are using middleware, the value of `type` must be a string in order for Redux's time travel features to function.
  </summary>

  >Strings are preferred for values of the `type` property of an action since they can be [serialized](https://en.wikipedia.org/wiki/Serialization). String instances are simple, modular data that are stored in memory and recalled from memory in a straight-forward manner; this is not necessarily the case with more complex data types like objects, especially ones involving references.

  [This serialization is important for Redux's time travel feature.](https://github.com/reactjs/redux/blob/master/docs/faq/Actions.md#actions-string-constants)
</details>


## The Reducer

The reducer specifies how actions update the state of the application, generating the next application-state.

# A Model of the Store

This is an approximation of a store.

```js
class Store {
  constructor(){
    this.subscriptions = []
    this.state = null
  }

  getState(){
    return this.state
  }

  // the ONE FUNCTION that handle any and all updates to our application-state
  reducer(state = [], action){
    // decides what type of state change
    switch (action.type) {
      case 'ADD_TODO':
        return [
          ...state,
          {
            id: action.id,
            text: action.text,
            completed: false
          }
        ]
      case 'TOGGLE_TODO':
        return state.map((todo) => {
          if (todo.id === action.id) {
            return {...todo, completed: !todo.completed}
          } else {
            return todo
          }
        })
      default:
        return state
    }
  }

  //called any time the reducer applies an action to a state
  subscribe(subscription){
    this.subscriptions.push(subscription)
  }
}
```

## Additional Store Methods

#### getState()

Returns the current state of the application tracked in the store
> `store.getState()`

#### dispatch({})  
Sends an action to the store's reducer to be evaluated and applied to state
> `store.dispatch({ type: "ACTION_TYPE" })`

#### subscribe()
Attaches methods to the store to be called anytime the state tracked in the store changes
> `store.subscribe(this.render)`

# We Do: Building a Counter in Redux (30 min)

[Building a Counter in Redux](https://github.com/ga-wdi-exercises/react-redux-counter)


<!-- # We Do: Shopping Cart in Redux (45 min)

[Building a Shopping Cart in Redux](https://github.com/ga-wdi-exercises/react-redux-shopping-cart) -->


# Appendix

***[Selectively curated list of resources on React, Redux, and Functional Programming](https://github.com/markerikson/react-redux-links)***

[Dummy’s Guide to Redux](https://medium.com/@stowball/a-dummys-guide-to-redux-and-thunk-in-react-d8904a7005d3#.v4pwh76z1)

[Thinking 'in React': Breaking an app down into components and thinking in terms of components](https://facebook.github.io/react/docs/thinking-in-react.html)

[Redux Docs: Usage with React](http://redux.js.org/docs/basics/UsageWithReact.html)

[React Docs: Components and Props](https://facebook.github.io/react/docs/components-and-props.html)
  - Determining if your component should be defined by a function or a class

[React Docs: State and Lifecycle](https://facebook.github.io/react/docs/state-and-lifecycle.html)
  - Guidelines for class components (stateful components)

[react-redux API](https://github.com/reactjs/react-redux/blob/master/docs/api.md)
