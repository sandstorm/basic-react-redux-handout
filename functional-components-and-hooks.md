## Functional Components and Hooks

### Functional Components

* JS-Functions which accept props as arguments and return JSX
* Return is equivalent to *render()* method in classes

```js
import React from 'react'

const Header = (props) => {
  return (
    <div className='header'>
      <Logo />
      <Navigation />
    </div>
  )
}

export default React.memo(Header); // => Memoize Component for performance improvements
```


#### React.memo() helper

* [React.memo()](https://reactjs.org/docs/react-api.html#reactmemo) is a helper to wrap and optimise functional components
* Should almost always be used
* Lets you memoize functional component results, so that the function will not be reevaluated unless its props change

### Hooks
* Functions that allow us to:
  * Add component state to functional components
  * Trigger side effects
  * Extract reusable logic (so that it can be used by multiple different components)
  * Perform optimisations
* [Take me to the docs](https://reactjs.org/docs/hooks-intro.html)

