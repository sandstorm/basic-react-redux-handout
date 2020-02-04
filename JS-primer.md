## JS Primer

* Important to know: [**statements** vs **expressions**](https://2ality.com/2012/09/expressions-vs-statements.html)
* `var` vs `let` & `const`:  [perfect explanation in TS Docs]('https://www.typescriptlang.org/docs/handbook/variable-declarations.html')
  * Bottom lines: 
    * **never** use `var` 
    * use `const` over `let` if possible
* Equality in JS
  * Rule of thumb: always use `===`
  * [JavaScript-Equality-Table](https://dorey.github.io/JavaScript-Equality-Table/)
  * Deep dive: [MDN docs](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Equality_comparisons_and_sameness)
* Arrow Functions
  * [MDN docs](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Functions/Arrow_functions)
* Template Literal
  ```js
  const interpolation = 'interpolation';
  const expressions = () => 'expressions';
  const text = `It's ${true ? 'easy' : 'hard'} string ${interpolation} with ${expressions()}!`;
  ```
* Destructuring
  * unpack values from arrays, or properties from objects
  * [MDN docs](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Destructuring_assignment)
* Spread operator
  * `...[1, 2, 3]` becomes `1, 2, 3`
  * [MDN docs](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Spread_syntax)
  * spreading arrays:
    ```js
        const list = [1, 2, 3, 4];
        const listCopy = [...list]; // creates a shallow copy

        console.log(list === listCopy); // false - different references
        console.log(list[0] === listCopy[0]); // true - shallow copied values & references are _the same_

        // order matters - array is filled in order
        console.log([0, ...list, 5]); // [0, 1, 2, 3, 4, 5]
    ```
  * spreading objects:
    ```js
        const obj = {
            a: 1,
            b: 2,
        };
        const objCopy = {...obj};

        console.log(obj === objCopy); // false - different references
        console.log(obj.a === obj.a); // true - shallow copied values & references are _the same_

        // oder matters - last assignment overrides previous ones
        console.log({{a: 5}, ...obj, {b: 3, c: 5}}); // {a:1, b:3, c:5}
    ```
* Rest parameter
  * _"reverse"_ of spread operator
  * [MDN docs](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Functions/rest_parameters)
* Optional Chaining aka _Elvis Operator_
  * `?.`
    ```js
        const user = {
            address: {
                street: 'nested-value-boulevard'
            }
        };

        // old way -> very verbose
        let street = undefined;
        if (user && user.address && user.address.street) {
            street = user.address.street;
        }

        // with optional chaining
        console.log(user?.address?.street); // 'nested-value-boulevard'
        console.log(user?.name?.firstName); // undefined - but no exception!
    ```
* Nullary coalescing operator aka _Safe Navigation_
  * `??`
    ```js
        const that = 'that';

        // logical 'or' works, but is dangerous! -> falsy values can introduce errors
        const thisOrThat = this || that;

        // safer with ternary
        const thisOrThat = this != null ? this : that;

        // with safe navigation -> checks if _this_ is nullary (null or undefined) 
        // like ternary expression above
        const thisOrThat = this ?? that;
    ```
