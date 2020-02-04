## Functional programming basics

### Pure functions

* No side effects (IO, database access, mutation of outside state…)
* Input defines output
* [Referential transparency](https://en.wikipedia.org/wiki/Referential_transparency)

```js
import { database } from "./stubs";

// Pure functions ?

const getUser = () => {
  return database.query("user");
}; // => impure, because of database access

const getUser2 = database => {
  return database.query("user");
}; // => still impure, because of database access

const add2 = (x) => {
  return x + 2;
}; // => pure

const add = (x, y) => {
  console.log(x, y);
  return x + y;
}; // => pure, because loggin is IO

const greetingPhrase = "Hi";
const greet = (name) => {
  return `${greetingPhrase}, ${name}!`;
}; // => impure, because greetingPhrase inside the outer scope could be changed, which makes output none deterministic

const greet2 = (name, greetingPhrase) => {
  return `${greetingPhrase}, ${name}!`;
}; // => pure
```

### Immutability

* Values can never be changed once created
* No re-assignments of variables
* No mutation of objects (they can’t change over time)

#### Why

* Makes code easier to reason about
* Helps to avoid race conditions and unintended or hard to debug value changes
* Allows for checks of reference equality of objects (which allows for powerful optimizations)
* Also allows for more performant equality checks where necessary
* Easier to do by using expressions instead of statements which leads to better composability

```js
  const compareSortedAndUnsorted = (numbers) => {
    const sortedNumbers = numbers.sort();

    // Compare first element
    return numbers[0] === sortedNumbers[0];
  } // => returns true, because both arrays refer to the same mutated reference

  const compareSortedAndUnsortedCorrectly = (numbers) => {
    const numbersCopy = [ ...numbers ];
    const sortedNumbers = numbersCopy.sort();

    // Compare first element
    return numbers[0] === sortedNumbers[0];
  } // => returns false, because we created a new array first


  // BUT: In the case below both console.log calls will show "true",
  // because 'numbers' will be mutated and sorted by compareSortedAndUnsorted.
  // Therefore compareSortedAndUnsortedCorrectly will receive the sorted array.
  const numbers = [3, 2, 1];
  console.log(compareSortedAndUnsorted(numbers));
  console.log(compareSortedAndUnsortedCorrectly(numbers))
```

#### Wait - so how am I supposed to do anything?

##### Iteration

* Use map, filter & reduce instead of for
* Performance will almost never be an issue (do not optimize prematurely!)

```js
// Mutable way
const sumList = (numberList) => {
  let sum = 0;

  for (let index = 0; index < numberList.length; index++) {
    sum = sum + numberList[index];
  }

  return sum;
}

// Immutable way
const sumListImmutably = (numberList) => {
  return numberList.reduce((sum, number) => {
    return sum + number
  }, 0)
}
```

##### Variable re-assignments

* Do not re-assign variables
* Create new variables with speaking names instead
* Use early returns
* Use higher order functions (map, filter, reduce)

```js
// Mutable way
const getUserNames = (users) => {
  let names = [];

  for (let index = 0; index < users.length; index++) {
    names.push(users[index].name);
  }

  return names;
}

// Immutable way
const getUserNamesImmutably = (users) => {
  return users.map(user => user.name);
}

// Mutable way
const getRoleLogoPath = (role) => {
  let logo;

  if (role === 'admin') {
    logo = '../somePath/adminLogo.png';
  } else if (role === 'mod') {
    logo = '../somePath/modLogo.png';
  } else {
    logo = '../somePath/userLogo.png';
  }

  return logo;
}

// Immutable way
const getRoleLogoPathImmutably = (role) => {
  if (role === 'admin') {
    return '../somePath/adminLogo.png';
  }

  if (role === 'mod') {
    return '../somePath/modLogo.png';
  }

  return '../somePath/userLogo.png';
}
```

##### Arrays & Objects

* Do not use push, pop etc. as they are mutable
* Create new Array/Object instances with Array/Object-spread or Object.assign instead
* Maybe use helper library for working with immutable data structures (e.g. [Immer](https://immerjs.github.io/immer/docs/introduction), [ImmutableJS](https://immutable-js.github.io/immutable-js/docs/#/))
* Caution: *Array.sort()* is also mutable (caught me off guard a few times ;) )

```js
// Arrays

// Mutable way to add to array
const myArray = [1, 2, 3, 4];
myArray.push(5);

// Immutable way to add to array
const arrayWithNewElement = [ ...myArray, 5 ];

// Mutable way to remove element from array
myArray.pop();

// Immutable way to remove element from array
const newArray = myArray.slice(0, myArray.length - 1);

// or use destructuring and the rest operator

const [head, ...newArray] = myArray;


// Objects

const person = {
  name: 'Dude Dudeson',
  age: 20
};

// Mutably change property (or even add property)
person.name = 'Matthilda Musterfrau';

// Immutably change property (or even add property)

const matthilda = {
  ...person,
  name: 'Matthilda Musterfrau'
}; // => will also have her age set to 20

// Mutably remove value from object
delete person.name;

// Immutably remove value from object
const {name, ...newObject} = person; // => newObject omits the name property
```

### Closures & Currying

#### Closures

* A closure is a function bundled with its lexical environment
* Remembers its surrounding scope it was defined in e.g. variable declarations

```js
// Closure examples

const greeting = 'Good morning' ;

const greet = (name) => {
  return `${greeting} ${name}`;
}

const greeterFactory = (salutation) => {
  const greeting = 'Good morning';

  return (name) => `${greeting} ${salutation} ${name}!`;
}


// => greetWoman still knows about 'greeting' even though greeterFactory() has
// already been called and might have been garbage collected
const greetWoman = greeterFactory('Mrs.');
const greetMan = greeterFactory('Mr.');
greetWoman('Matthilda M.'); // => 'Good morning Mrs. Matthilda M.'
```

#### Currying

* Named after Haskell Brooks Curry
* Describes a way to split a function with n arguments into n nested functions with a single argument each
* This in turn allows for easier composition

```js
// Currying
const regularAdd = (a, b) => a + b;

const curryiedAdd = (a) => (b) => a + b;

const add5 = curryiedAdd(5);
```

#### Higher-Order-Functions

* Definition: A function that either receives one or more functions as parameter(s) and/or returns another function
* Popular examples: *Array.map()*, *Array.filter()*, *Array.reduce()*, also basically any function expecting a callback of some sort

```js
// This is a HOF because its initial type signature looks like this:
// (a: number) => Function
// So it returns another function and is therefore a HOF
const add = (a) => (b) => a + b;

// This is a HOF because its first parameter is a Function
const calculate = (operatorFn, operandA, operandB) => {
  return operatorFn(operandA, operandB);
}
```
