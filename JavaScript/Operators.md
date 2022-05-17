# Operators
## Arithmetic Operators
- Addition: ```+```
- Substraction: ```-```
- Multiplication: ```*```
- Division: ```/```
- Remainder: ```%```
- Exponent: ```**```
- Increment: ```++```:
```js
console.log(x++); // 先 x 后 ++ => 10
console.log(++x): // 先 ++ 后 x => 11
```
- Decrement: ```--```

## Assignment Operators
- 赋值: ```=, +=```

## Comparison Operators
```>, >=, <, <=, ==, !==, === (same type and value)```
```js
console.log('1'==1) // true
```

## Ternary Operators
```js
let points = 100;
let type;

// 全写
if (points > 100) {
  type = 'gold';
}
else {
  type = 'solver';
}

// 简写
let type = condition ? value1 : value2
```


## Logicial Operators
- AND: ```&&```
- OR: ```||```
- NOT: ```!```

```js
// Falsy (false)
// undefined
// null
// 0
// false
// ''
// NaN

// Anything that is not falsy => Truthy
// false || 'Mosh' => 'Mosh'
// false || 1 => 1
// false || 1 || undefined => 1

// let userColor = 'red';
// let defaultColor = 'blue';
// let currentColor = userColor || defaultColor;
// 先判断 operator 的类型, 再从左往右判断每一个 element 是 truthy 还是 falsy, 碰到第一个 truthy element 的时候将其返回 
```

## Bitwise Operators
- Bitwise OR: ```|```
- Bitwise AND: ```&```

