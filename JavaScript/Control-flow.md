# Control Flow
## If-Else
```js
let hour = 10;
if (hour >= 6 && hour < 12) {
  console.log('Good Morning');
}
else if (hour >= 12 && hour < 18) {
  console.log('Good Afternoon');
}
else {
  console.log('Good Evening');
}
```

## Switch...case
```js
let role = 'guest';
swtich (role) {
  case 'guest':
    console.log('Guest User');
    break;
  case 'moderator':
    console.log('Moderator User');
    break;
  default:
    console.log('Unknown User');
}
```

## For
```js
for (let i = 0; i < 5; i++) {
  console.log('Hello World');
}
```

## While
略

## Do...While
略

## For...in
```js
const person = {
  name: 'Eason',
  age: 30
}
for (ley key in person) {
  console.log(key, person[key]);
}

const colors = ['r', 'b', 'g']
for (let index in colors) {
  console.log(index, colors[index]);
}
```

## For...of
```js
const colors = ['r', 'b', 'g']
for (let color of colors) {
  console.log(color);
}
```

## Break && Continue
略
