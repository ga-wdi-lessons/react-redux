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

## State Methods

.getState()
`store.getState()`

.dispatch({})
`store.dispatch({ action:{ update:{ todo: completed }} })`

.subscribe()
`store.subscribe(this.render)`


# Dispatching Actions

An action is a garden-variety javascript object that describes what kind of change is to take place, specifying what change to make to what data.

The only 'expectation' that Redux has for an action is that it will have a `type` property that ***is not*** undefined. It is recommended that the value of `type` be a string. Strings are preferred for values of the `type` property of an action. Strings can be [serialized](https://en.wikipedia.org/wiki/Serialization).

[This serialization is important for Redux's time travel feature.](https://github.com/reactjs/redux/blob/master/docs/faq/Actions.md#actions-string-constants)

# The Great Reducer

The reducer specifies how actions update the state of the application, generating the next application-state.

# We do: Add Redux to Todos
