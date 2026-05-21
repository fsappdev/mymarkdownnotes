# Note 2026 05 21 11 40 28 - Pass Children as a Prop

```javascript
// #react
const Parent = () => {
  return (
    <Child componentToPassDown={<SomeComp />} />
  )
}

const Child = ({ componentToPassDown }) => {
  return (
    <>
      {componentToPassDown}
    </>
  )
}
```

 