# TypeScript

Use the awesome, official [docs](https://www.typescriptlang.org/docs/handbook/basic-types.html)!

Also a good reference for everything JS & TS: [TypeScript Deep Dive](https://basarat.gitbook.io/typescript/)


## Typing a custom hook with array return

Sometimes you might want to build a custom React hook returning an array, similar to Reacts built in `useState()` hook.

```ts
const useCounter = () => {
  const [counter, setCounter] = useState(0)
  
  const increase = () => setCounter(counter + 1)
  const decrease = () => setCounter(counter - 1)

  return [counter, increase, decrease]
}

const myComponent = () => {
  const [counter, increase, decrease] = useCounter()
  
  return (
    ...
  )
}
```

However because we did not explicitely define the return type of our custom hook, typescript infers it as `Array<number | () => void`.
This means that typescript does not know how the type at any position inside the returned array looks like. It could always be either a number, or a dispatch function.

To remedy this, you have basically two options:

1. use an object as return type instead
2. explicitely type the return value of your custom hook, by using a tuple:

```ts
type UseCounterReturnType = [number, () => void, () => void]

const useCounter = (): UseCounterReturnType => {
   ...
}
```
