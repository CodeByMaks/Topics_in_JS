# Topics_in_JS

## Здравствуйте! Сегодня мы узнаем о таких темах:
 * Scope
 * Hoisting
 * TDZ

---
# SCOPE
Scope (область видимости) в JavaScript определяет доступность переменных, функций и объектов в разных частях вашего кода. 
Существуют три основные области видимости в JavaScript:

---

### Глобальная область видимости (Global Scope)
> Переменная объявляется вне всех функций или блоков. Она доступна везде в коде.

Пример:
```js
let globalVariable = "I am global";

function exampleFunction() {
  console.log(globalVariable); // "I am global"
}

exampleFunction();
console.log(globalVariable); // "I am global"
```

### Функциональная область видимости (Function Scope)
> Переменные, объявленные внутри функции, доступны только внутри этой функции.

Пример:
```js
function exampleFunction() {
  let localVariable = "I am local";
  console.log(localVariable); // "I am local"
}

exampleFunction();
console.log(localVariable); // Ошибка: localVariable не определена
```

### Блочная область видимости (Block Scope)
> Переменные, объявленные с помощью let или const внутри блока {}, доступны только внутри этого блока.
> Важно: Переменные, объявленные с помощью var, не поддерживают блочную область видимости, а привязываются к функции или глобальной области.

Пример:
```js
if (true) {
  let blockScoped = "I am block-scoped";
  console.log(blockScoped); // "I am block-scoped"
}

console.log(blockScoped); // Ошибка: blockScoped не определена
```

## Различия между var, let и const:

### var:
> Область видимости ограничена функцией, а не блоком.
> Переменная поднимается (hoisting), но ее значение становится доступным только после строки объявления.
```js
function test() {
  if (true) {
    var testVar = "var scoped";
  }
  console.log(testVar); // "var scoped"
}
test();
```

### let и const:
> Имеют блочную область видимости.
> Не поднимаются так же, как var.
```js
if (true) {
  let testLet = "block scoped";
  const testConst = "block scoped const";
}
console.log(testLet); // Ошибка: testLet не определена
```

## Замыкания и область видимости
> Замыкание (closure) возникает, когда функция запоминает область видимости, в которой она была создана.

Пример:
```js
function outerFunction() {
  let outerVariable = "I am from outer scope";

  function innerFunction() {
    console.log(outerVariable);
  }

  return innerFunction;
}

const closure = outerFunction();
closure(); // "I am from outer scope"
```

---

# HOISTING
Hoisting (поднятие) в JavaScript — это механизм, при котором переменные, функции и классы "поднимаются" (перемещаются) интерпретатором в начало своей области видимости, даже если они были объявлены позже в коде.

> Однако, несмотря на то что объявления поднимаются, инициализация переменных и классов остаётся на месте.

---

## Как работает hoisting?
### Переменные (var, let, const):
> Для переменных, объявленных с помощью var, поднимается только объявление, а значение остаётся на месте.
> Переменные, объявленные через let и const, также поднимаются, но они находятся в "временной мёртвой зоне" (Temporal Dead Zone, TDZ), пока не будут достигнуты строкой объявления.

### Функции:
> Объявления функций через function поднимаются целиком (и объявления, и реализации).
> Функции, определённые через function expression или стрелочные функции (=>), ведут себя как переменные.

### Классы:
> Классы поднимаются, но не инициализируются, что приводит к ошибке при их использовании до строки объявления.

## Примеры:
### Переменные с var:
```js
console.log(myVar); // undefined (объявление поднято, но значение не присвоено)
var myVar = 10;
console.log(myVar); // 10
```

### Переменные с let и const:
```js
console.log(myLet); // Ошибка: Cannot access 'myLet' before initialization
let myLet = 20;

console.log(myConst); // Ошибка: Cannot access 'myConst' before initialization
const myConst = 30;
```
> Переменные let и const находятся в TDZ, пока не достигнут строки объявления.

### Функции, объявленные через function:
```js
greet(); // "Hello!"

function greet() {
  console.log("Hello!");
}
```
> Функции, объявленные через function, поднимаются вместе с реализацией.

### Функции, объявленные через var (Function Expression):
```js
console.log(sayHi); // undefined (переменная объявлена, но значение ещё не присвоено)
var sayHi = function () {
  console.log("Hi!");
};
sayHi(); // "Hi!"
```
> Для function expressions поднимается только объявление переменной.

### Классы:
```js
const obj = new MyClass(); // Ошибка: Cannot access 'MyClass' before initialization

class MyClass {
  constructor() {
    this.name = "Class Example";
  }
}
```
> Хотя класс поднимается, он недоступен до инициализации.

## Как избежать проблем с hoisting?
* Используйте let и const вместо var, чтобы избежать неявного поведения.
* Объявляйте переменные и функции в начале их области видимости.
* Избегайте обращения к переменным или функциям до их объявления.

---

# TDZ
TDZ (Temporal Dead Zone) — это временная мёртвая зона в JavaScript, которая существует для переменных, объявленных с помощью let, const, а также для классов. В этой зоне переменная считается недоступной до того момента, пока не будет достигнута строка её объявления.

---

## Что такое TDZ?
### Начало TDZ:
> TDZ начинается с начала области видимости переменной.
> Например, для переменной, объявленной в функции, TDZ начинается с начала тела функции.

### Окончание TDZ:
> TDZ заканчивается на строке, где переменная объявлена с помощью let, const или class.

### Доступ в TDZ:
> Попытка обратиться к переменной в TDZ вызывает ошибку ReferenceError.

### Почему TDZ существует?
> TDZ помогает разработчикам избежать доступа к переменным до их фактического объявления. Это делает поведение кода более предсказуемым и защищённым от ошибок.

## Примеры TDZ:

### Пример с let:
```js
{
  console.log(myLet); // Ошибка: Cannot access 'myLet' before initialization
  let myLet = 10;
  console.log(myLet); // 10
}
```
> Переменная myLet существует в области видимости с начала блока {}, но она находится в TDZ до строки объявления let myLet = 10;.

### Пример с const:
```js
{
  console.log(myConst); // Ошибка: Cannot access 'myConst' before initialization
  const myConst = 20;
  console.log(myConst); // 20
}
```
> Поведение const аналогично let, с той лишь разницей, что значение переменной const должно быть обязательно присвоено во время объявления.

### Пример с функцией:
```js
{
  console.log(myFunc); // Ошибка: Cannot access 'myFunc' before initialization
  let myFunc = function () {
    console.log("Hello!");
  };
  myFunc(); // "Hello!"
}
```
> Функция, объявленная как выражение (function expression), также находится в TDZ, пока не будет выполнена строка с её объявлением.

### Пример с классами:
```js
const instance = new MyClass(); // Ошибка: Cannot access 'MyClass' before initialization

class MyClass {
  constructor() {
    this.name = "Test";
  }
}
```
> Классы, как и переменные, находятся в TDZ до строки, где они объявлены.

## Как TDZ влияет на поведение кода?

### Без TDZ (var):
```js
{
  console.log(myVar); // undefined
  var myVar = 30;
  console.log(myVar); // 30
}
```
> Переменная var поднимается, но не попадает в TDZ. Это может приводить к неожиданному поведению.

### С TDZ (let и const):
```js
{
  console.log(myLet); // Ошибка: Cannot access 'myLet' before initialization
  let myLet = 30;
  console.log(myLet); // 30
}
```
> С let и const, TDZ предотвращает доступ до строки объявления.

Пока на этом всё. Вы можете найти другие ответы из интернета или документациях.

---

<div align="center">
  <img src="https://a.d-cd.net/fGkj61MC91njUbKjZUahzEm1-IA-960.jpg" width="600px" height="300px">
</div>

---
