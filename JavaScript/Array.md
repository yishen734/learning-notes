# Array
## 添加新元素
```js
const numbers = [3, 4];

// End
numbers.push(5);

// Begin
numbers.unshift(1, 2);

// Middle
numbers.splice(2, 0, 'a', 'b');
```

## Finding Elements (Value Types)
```js
const numbers = [1, 2, 3, 1, 4];
numbers.indexOf(1, 2);  // 3
numbers.lastIndexOf(1); 
numbers.includes(1);
```


## Finding Elements (Reference Types)
```js
const courses = [
  { id: 1, name: 'a'},
  { id: 2, name: 'b'}
]

const course = courses.find(function(course) {
  return course.name === 'a';
})

const course = courses.findIndex(function(course) {
  return course.name === 'a';
})
```

## Arrow functions
```js
const courses = [
  { id: 1, name: 'a'},
  { id: 2, name: 'b'}
]

const course = courses.find(course => {
  return course.name === 'a';
});

const course = courses.find(course => course.name === 'a');

```

## Removing Elements
```js
const numbers = [1, 2, 3, 4];

// End
const last = numbers.pop();

// Start
const first = numbers.shift();

// Middle
numbers.splice(2, 1);
```

## Emptying an Array
```js
let numbers = [1, 2, 3, 4];
let another = numbers;

// Solution 1 *
numbers = []

// Solution 2 *
numbers.length = 0;

// Solution 3
numbers.splice(0, number.length);
```

## Combining and Slicing Arrays
```js
const first = [1, 2, 3];
const second = [4, 5, 6];

// Combine
const combined = first.concat(second);  // 123456

// Slice
combined.slice(2, 4)  // 34

combined.slice()  // copy
```
