# React Redux

# Learning Objectives
  - Explain the concept of state
  - Explain the advantages of having a single, immutable container-object managing your application's state
  - Explain the role of the store in a React app and how it influences the architecture of a React app
  - Explain the role of the store
  - Explain the roles of actions & dispatches
  - Explain what problem Redux Solves

# What is Redux?

Redux is a state management tool. It can time travel. It solves the problem of having state being stored in multiple places in a somewhat fragmented fashion.

# What is application state?

Simply put, state is a representation of your application's data. Redux manages your application's state, encapsulating the data stored in your variables.

# Immutable State Tree

  - Redux encapsulates the state of an application in a single object
  - This object is called the Immutable State Tree
    - What does immutable mean? Why immutable? Why would it make sense for the tree to be read-only?
    - What is a tree? Why would state be stored in tree form?


# The Store
The store is a kind of hub that all the information (or the **state**) in a program flows through. **The store** encapsulates not only the data in the program, but also controls the flow of program data, storing each change in a separate state. Redux even gives us the ability to time travel through our application's history of application states. It's like the principles of git applied to application-state rather than file-state.

In it, all the principles of Redux are unified-- the application's **state object**, **the dispatcher**, and **the reducer**, which specifies how actions update the state object.

Since state is being represented as an immutable data-structure, we cannot directly modify it. Changing state in the program requires dispatching an action that modifies a copy of the state, this becomes the next state of the program, spat out by the reducer.

## Store

```js
class Store {
  constructor(reducerFunc){
    this.subscriptions = []
    this.state = null
    this.reducer = reducerFunc
  }

  getState(){
    return this.state
  }

  dispatch(action, state){
    switch (action.type) {
      case "CREATE":
        return action.newTodo //TODO: fix action handling example
      case "UPDATE":
        return action.editTodo //TODO: fix action handling example
      case "DELETE":
        return action.deleteTodo //TODO: fix action handling example
      default:
        return state
    }

    //calls the reducer to apply state change
    this.reducer(action, state)
  }

  //called any time an action is dispatched
  subscribe(subscription){
    this.subscriptions.push(subscription)
  }
}
```

## Store Methods

### `.getState()`
`store.getState()`

### `.dispatch({})`

`store.dispatch({ type: "ACTION_TYPE" })`

### `.subscribe()`

`store.subscribe(this.render)`

Every time an action has been dispatched via the reducer, we want to update the UI. So we subscribe the render method to any changes taking places to the application's state object.


# Dispatching Actions

An action is a garden-variety javascript object that describes what kind of change is to take place, specifying what change to make to what data.

The only 'expectation' that Redux has for an action is that it will have a `type` property that ***is not*** undefined. It is recommended that the value of `type` be a string. Strings are preferred for values of the `type` property of an action. Strings can be [serialized](https://en.wikipedia.org/wiki/Serialization).

[This serialization is important for Redux's time travel feature.](https://github.com/reactjs/redux/blob/master/docs/faq/Actions.md#actions-string-constants)

# The Great Reducer

The reducer specifies how actions update the state of the application, generating the next application-state.

# Don't Mutate State!

## Techniques for Avoiding the Mutation of State

#### Arrays
  - `.concat()` and `...` the spread operator ES6
  - `.slice()` not `.splice()`

#### Objects
  - Object.assign({})
  - `...` the spread operator ES6
  - Destructuring Assignments


# We do: Add Redux to Todos

## Using .getState with Component composition props

Redux is managing our application's state via the store.

We are going to be passing information about the application's state from the store in to a component's props.
