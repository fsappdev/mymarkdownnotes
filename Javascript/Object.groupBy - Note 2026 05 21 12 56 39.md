# Object.groupBy - Note 2026 05 21 12 56 39

 

```javascript

const inventory = [
  { name: "asparagus", type: "vegetables", quantity: 9 },
  { name: "bananas", type: "fruit", quantity: 5 },
  { name: "goat", type: "meat", quantity: 23 },
  { name: "cherries", type: "fruit", quantity: 12 },
  { name: "fish", type: "meat", quantity: 22 },
];

const result = Object.groupBy(inventory, ({ quantity }) =>
  quantity < 6 ? "restock" : "sufficient",
);
console.log(result);
//[Object: null prototype] {
//  sufficient: [
//    { name: 'asparagus', type: 'vegetables', quantity: 9 },
//    { name: 'goat', type: 'meat', quantity: 23 },
//    { name: 'cherries', type: 'fruit', quantity: 12 },
//    { name: 'fish', type: 'meat', quantity: 22 }
//  ],
//  restock: [ { name: 'bananas', type: 'fruit', quantity: 5 } ]
//}
```

 