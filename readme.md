# React Redux

# Learning Objectives
  - Identify how Redux leverages unidirectional data-flow and immutability
  - Explain the role of the store in a React app and how it influences the architecture of a React app
  - Explain the role of the store
  - Explain the roles of actions and the reducer
  - Explain what problem Redux Solves

# What is Redux? (0:05, 5 min)

Redux is a state management library. It solves the problem of having a bunch of localized component states by funneling them into a central hub. This really begins to become an issue for developers as applications scale, increasing in complexity and size. Redux is extremely opinionated and entails writing applications with heavy limitations.

The limits Redux imposes don't necessarily restrict what you write so much as how you write it.

With the [Redux Devtools Chrome Extension](https://chrome.google.com/webstore/detail/redux-devtools/lmhkpmbekcpmknklioeibfkpmmfibljd?hl=en), we have the ability to see and interact with the data in our programs as if it were a movie we could pause, play, rewind, and fast-forward.

> #### *Take a moment to install the [Redux Devtools Chrome Extension](https://chrome.google.com/webstore/detail/redux-devtools/lmhkpmbekcpmknklioeibfkpmmfibljd?hl=en)*

# Use Case (0:15, 10min)

Let's take 5 minutes to read through a blog post written by the creator of Redux, Dan Abramov.

 From [***You Might Not Need Redux***](https://medium.com/@dan_abramov/you-might-not-need-redux-be46360cf367#.47e1bpmhj):

> Redux offers a tradeoff. It asks you to:
- Describe **application state as plain objects and arrays**.
- Describe **changes** in the system as **plain objects**.
- Describe **the logic for handling changes as pure functions**.


Dan Abramov, creator of Redux and author of the blog post above. It is definitely not necessary to involve in every app. The blog post lists use cases for Redux, features that Redux provides to the application developer, and the implementation limitations that Redux imposes.

>However, if you’re just learning React, don’t make Redux your first choice.
Instead learn to [think in React](https://facebook.github.io/react/docs/thinking-in-react.html). Come back to Redux if you find a real need for it, or if you want to try something new. But approach it with caution, just like you do with any highly opinionated tool.

For this lesson, we're throwing caution (or a regard for our own momentary sanity) to the wind. There is a formidable learning curve with Redux, but the exposure is valuable for a few reasons:

1. It is being increasingly adopted by React developers, especially teams working on very large applications.

2. Working with Redux exposes us to very elegant applications of functional programming and convey its power and potential. Functional programming presents us with the challenge of having to think in new ways about how our programs are composed and pass data.

3. Its limitations force the developer to very carefully consider the structure of their application.

# Immutable State Tree

  - Redux encapsulates the state of an application in a single object
  - This object is called the Immutable State Tree
    - What does immutable mean? Why immutable? Why would it make sense for the tree to be read-only?
    - What is a tree? Why would state be stored in tree form?

# Application state

Simply put, state is a representation of your application's data. Redux manages your application's state, encapsulating the data stored in your variables, in something called **The Store**.

# The Store

The store is a kind of hub that all the information (or the **state**) in a program flows through. **The store** encapsulates not only the data in the program, but also controls the flow of program data, storing each change in a separate state. Redux even gives us the ability to time travel through our application's history of application states. It's like the principles of git applied to application-state rather than file-state.

In the store, all the principles of Redux are embodied-- the application's **state object**, **the dispatcher**, and **the reducer**, which specifies how actions update the state object.

Since state is being represented as an immutable data-structure, we cannot directly modify it. Changing state in the program requires dispatching an action that modifies a copy of the state, this becomes the next state of the program, spat out by the reducer.

## A Model of the Store

```js
class Store {
  constructor(){
    this.subscriptions = []
    this.state = null
  }

  getState(){
    return this.state
  }

  //the ONE FUNCTION that handle any and all updates to our application-state
  reducer(action, state){
    //decides what type of state change
    switch (action.type) {
      case 'ADD_TODO':
        return {
          id: action.id,
          text: action.text,
          completed: false
        }
      case  "TOGGLE_TODO_COMPLETED":
        return {
          ...state,
          completed: !state.completed
        }
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

## Store Methods (30/30+?)

### `.getState()`
`store.getState()`

### `.dispatch({})`

`store.dispatch({ type: "ACTION_TYPE" })`

### `.subscribe()`

`store.subscribe(this.render)`

Every time an action has been dispatched via the reducer, we want to update the UI. So we subscribe the render method to any changes taking places to the application's state object.


# Actions

An action is a garden-variety javascript object that describes what kind of change is to take place, specifying what change to make to what data.

The minimum requirement for an action is that the action must have a type property that **is not** `undefined.`
<details>
  <summary>
  Unless you are using middleware, the value of `type` be a string in order for Redux's time travel features to function.
  </summary>
  Strings are preferred for values of the `type` property of an action since they can be [serialized](https://en.wikipedia.org/wiki/Serialization). String instances are simple, modular data that are stored in memory and recalled from memory in a straight-forward manner; this is not necessarily the case with more complex data types like objects, especially ones involving references.

  [This serialization is important for Redux's time travel feature.](https://github.com/reactjs/redux/blob/master/docs/faq/Actions.md#actions-string-constants)
</details>


# The Great Reducer

The reducer specifies how actions update the state of the application, generating the next application-state.

# Don't Mutate State!

## Techniques for Avoiding the Mutation of State

ES6 gives us quite a few useful tools for dealing with immutable data. `Object.assign()` in particular gives us a powerful way of copying objects. This along with destructuring assignments give us a nice set of tools for immutable object handling. For Arrays, we have the good old reliable `.slice()` (first implemented in ES3) for copying arrays, and spread operators for combining arrays or parts of arrays.

<!-- TODO: Create immutability technique cheatseet! -->

### Arrays
  - `.concat()` and `...` the ES6 spread operator
  - `.slice()` not `.splice()`

### Objects
  - Object.assign({}, obj, {props})
  - `...` the spread operator
  - Destructuring Assignments

<!-- TODO: Create todos app -->

# We do: Add Redux to Todos

## Using .getState with Component composition props

Redux is managing our application's state via the store. Since that is the case, we're going to be obtaining the state from the store via `getState` constantly. This might seem like a pain in the neck, but in reality it gives incredible control over our app. This is what gives us the ability to see the data in our application playing like a movie that we can pause, rewind, and fast-forward.

We are going to be passing information about the application's state from the store in to a component's props.

# Appendix

[Immaculately curated topically-organized list of links on React, Redux, and other topics like functional programming](https://github.com/markerikson/react-redux-links)

[Thinking 'in React': Breaking an app down into components and thinking in terms of components](https://facebook.github.io/react/docs/thinking-in-react.html)
