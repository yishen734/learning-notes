# Object
## 定义 object
```js
const circle = {
  // Attributes
  radius: 1
  location: {
    x: 1,
    y: 1
  },
  isVisible: true,
  
  // Behaviors
  draw: function() {}
  move: function() {}
};

circle.draw();
```

## Factory Function
```js
function createCircle(radius) {  
  return {
    // Attributes
    radius

    // Behaviors
    draw() {}
  };
}

const circle1 = createCircle(1)
```

## Constructor Function
```js
function Circle(radius) {
  this.radius = radius;
  this.draw = function() {}
}

const circle1 = new Circle(1)
```

## Dynamic
```js
const circle = {
  radius: 1
}

circle.color = 'yellow';
corcle.draw = function() {}
delete circle.color
```

### 方程自身也是 object

## Value vs Reference Types
```js
let x = 10;
let y = x;
x = 20;
// y: 10, x: 20

let x = { value: 10 };
let y = x;
x.value = 20;
// y: { value: 20 }, x: { value: 20 }
// 对于 reference type, { value: 10 } 存在内存的某一个地方, 然后 x 和 y 同时指向这个地方
// 总结: primitives are copied by their value, objects are copied by their reference

let number = 10;
function increase(number) {
  number++;
}
increase(number);
consile.log(number) // 10 => pass 的是 value
```

## Enumerating Properties of an Object
```js
const circle = {
  radius: 1,
  draw() {
    console.log('draw');
}

for (let key in circle) {
  console.log(key, circle[key]);
}

for (let key of Object.keys(circle)) {
  console.log(key);
}

for (let entry of Object.entries(circle)) {
  console.log(entry);
}

if ('radius' in circle) console.log('yes');
```

## 克隆 Object
```js
const circle = {
  radius: 1,
  draw() {
    console.log('draw');
}

// Old way
const another = {}
for (let key in circle) {
  another[key] = circle[key]; 
}

// Modern way 1
const another = Object.assign({}, circle);

// Modern way 2
const another = { ...circle };
```

## Math Oject
看文档

## String
```js
// String primitive
const message = 'hi';
message.length  // 有一个底层运作的 wrapper 把 message warp 成 String object
// String object
const another = new String('hi');
```

## Template Literals
```js
const message = 'This is my \n first message';
const another = `This is my 'first' message ${name}` ${2 + 3};
```

## Date
```js
const now = new Date();
const date1 = new Date('May 11 2018 09:00');
const date2 = new Date(2018, 4, 11, 9);
```
