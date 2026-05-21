# function tag(descompone texto y variables) Note 2026 05 21 12 28 32

 

```javascript

function tag(strings, ...values) {
console.log("strings...", strings)
console.log("values...", values)

return strings[0] + values[0] * 2 + strings[1] + values[1] + strings[2];
}

const num1 = 5;
const num2 = 10;

const result = tag`Double ${num1} and add ${num2}!`;

console.log(result);
```

 