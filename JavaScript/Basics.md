# 变量声明, 常数, Primitives, 对象, 数组, 方程

## 变量声明
```js
let name = 'Eason';
console.log(name);
```

## 常数
```js
const interestRate = 0.3;
```

## Primitives / Value Types
- String: ```'Eason'```
- Number (integer 和 float): ```30```
- Boolean: ```true, false```
- Undefined:
```js
let testVar;
alert(testVar); // Shows undefined
alert(typeof testVar); // Shows undefined
```
- Null

## 动态语言
一个变量的类型可以发生改变
```js
let name = "Eason";
typeof name // string
name = 1;
typeof name // number
```

## Reference Types
- 对象 (Object)
- 数组 (Array)
- 方程 (Function)

## 对象 (Object)
```js
let person = {
  name: 'Eason',
  age: 30
};

// Change the property
person.name = 'John'; // Dot notation -> 默认用法
person['name'] = 'John'; // Bracket notation

// Read the property
console.log(person.name)
console.log(person['name'])
```

## 数组 (Array)
```js
let selectedColors = ['red', 'blue'];
selectedColors[2] = 1  // 数组里面的元素的类型可以不一样 
console.log(selectedColors);
console.log(selectedColors[0]);
console.log(typeof selectedColors);  // object, 内置很多方程
console.log(selectedColors.length);  // 显示数组的长度
```

## 方程 (Function)
```js
function greet(name) {
  console.log('Hello ' + name);
}

function square(nunmber) {
  return nunmber * nunmber;
}

greet('Eason');
greet('Mary');
let number = square(2);
```

