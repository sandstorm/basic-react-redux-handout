## [Redux](https://redux.js.org/)

### Overview

* Stand alone library which is not directly tied to react
* Application State vs. Component State
* State which is accessible inside multiple components (Application state)
* No “Prop drilling” necessary
* Functional approach to state management fitting the mental model of react
* Native solutions in React exist (e.g. Context API), but lack a few very powerful features
* Powerful middlewares and enhancers available (redux-saga, redux-observable etc.)
* Powerful [Redux-DevTools](https://github.com/reduxjs/redux-devtools), that allow deep state inspection as well as time travel debugging
* Recommendation: use [Redux-Toolkit](https://redux-toolkit.js.org/) for most projects to reduce boilerplate
* Alternatives exist (e.g. mobx)

![Redux-Architecture-Overview](../redux-architecture-overview-middleware.jpg)


### Store (state)

* Just a single plain javascript object (basically a model without setters)
* The state is read only
* A new state can only be created by Actions describing how the state should change
* Such a state transformation is made with pure functions called reducers

### Reducer

* Just a pure function
* Signature of basic form: `(state: State, action: Action): State`


```ts
const Reducer = (state: State, action: Action): State => {
  switch (action.type) {
    case 'SOME_TYPE': {
      // ... some business logic here ...
      const newState = // ... assignment logic

      return newState
    }

    default: {
      return state
    }
  }
}
```


### Action

* Javascript object
* Always has to have a type property
* Might contain additional data (which by convention is usually condensed into a payload property)


```ts
const simpleAction = { type: 'SOME_TYPE' };

const actionWithPayload = {
  type: 'SOME_OTHER_TYPE',
  payload: {
    id: 1,
    name: 'Matthilda'
  }
};
```


### ActionCreators

* Usually we create Actions by calling functions
* An ActionCreator is simply a function returning a specific Action

```ts
const doSomeAction = (id) => {
  return {
    type: 'SOME_ACTION',
    payload: {
      id,
      name: 'Matthilda'
    }
  }
};
```


### Selectors

* Basically getters to retrieve slices of the state
* Pure functions which can receive state and component props
* Because they are pure functions they can be easily composed with one another
* Because they are pure functions their calculations can be memoized (usually done by using [reselect](https://github.com/reduxjs/reselect))

```ts
const primitiveSelector = (state) => state.something;
const primitiveSelectorWithProps = (state, id) => state.else[id];


const getSomeList = (state) => state.someList;
const getId = (_, id) => id;
const getElementById = createSelector(
  [getSomeList, getId],
  (someList, id) => someList.find((element) => element.id === id);
)
```
