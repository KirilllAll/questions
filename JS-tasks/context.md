# Ниже будут приведены задачи на контекст с разбором

## 1. Какое значение примет this?

```javascript
const user = {
  name: "John",
  getName() {
    return this.name;
  },
};

const getName = user.getName;

console.log(user.getName());
console.log(getName());
```

### Ответ

- John
- undefined

## 2. Как привязать контекст?

```javascript
function greet() {
  return `Hello, ${this.name}`;
}

const user1 = { name: "Alice" };
const user2 = { name: "Bob" };

console.log(greet.call(user1)); // ???
console.log(greet.apply(user2)); // ???
```

### Вопросы:

    - Что выведут вызовы `greet.call(user1)` и `greet.apply(user2)`?
    - Чем отличаются методы `call` и `apply`?

### Ответы:

    - Alice
    - Bob

## 3. Использование bind

```javascript
function multiply(a, b) {
  return a * b;
}

const double = multiply.bind(null, 2);

console.log(double(3));
console.log(double(5));
```

### Ответы:

    - 6
    - 10

## 4. Проблема с потерей контекста

```javascript
const user = {
  name: "John",
  showName() {
    console.log(this.name);
  },
};

setTimeout(user.showName, 1000); // ???
```

### Вопросы:

    - Почему `this` теряется в `setTimeout`?
    - Как исправить проблему с потерей контекста?

### Ответы:

    - Это происходит потому, что setTimeout вызывает функцию в глобальном контексте (или в контексте `window` в браузере), а не в контексте объекта, к которому принадлежит метод.
    - Использование стрелочной функции; Использование метода bind;Использование переменной для хранения контекста;

## 5. Стрелочные функции и контекст

```javascript
const user = {
  name: "John",
  showName() {
    setTimeout(() => {
      console.log(this.name);
    }, 1000);
  },
};

user.showName(); // ???
```

### Вопросы:

    - Почему в этом случае `this` правильно ссылается на объект `user`?

### Ответ

    - Стрелочная функция смотрит на контекст выше

## 6. Вложенные объекты и контекст

```javascript
const user = {
  name: "John",
  friends: {
    name: "Alice",
    showFriend() {
      console.log(this.name);
    },
  },
};

user.friends.showFriend(); // ???
```

### Вопросы:

    - Какое значение выведется и почему?

### Ответ:

    - Когда вы вызываете user.friends.showFriend(), значение this внутри метода showFriend будет ссылаться на объект user.friends. Это происходит потому, что this внутри метода объекта всегда ссылается на объект, к которому принадлежит метод.

    - Таким образом, this.name внутри метода showFriend будет равно "Alice", так как name в объекте user.friends равно "Alice".

## 7. Как работает this в событиях DOM?

```javascript
const button = document.getElementById("myButton");

button.addEventListener("click", function () {
  console.log(this); // ???
});
```

### Ответ

    - Выведет <button id="myButton">Click me</button>
