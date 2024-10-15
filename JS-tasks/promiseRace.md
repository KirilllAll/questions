# Реализация Promise.race

Функция `Promise.race` возвращает промис, который разрешается или отклоняется первым разрешенным или отклоненным промисом из переданного массива промисов.

## Пример использования

```javascript
const promise1 = new Promise((resolve, reject) =>
  setTimeout(resolve, 100, "one")
);
const promise2 = new Promise((resolve, reject) =>
  setTimeout(resolve, 200, "two")
);
const promise3 = new Promise((resolve, reject) =>
  setTimeout(resolve, 300, "three")
);

Promise.race([promise1, promise2, promise3]).then((value) => {
  console.log(value); // Выведет 'one'
});
```

## Реализация

```javascript
function promiseRace(promises) {
  return new Promise((resolve, reject) => {
    if (!Array.isArray(promises)) {
      return reject(new TypeError("Argument is not an array of promises"));
    }

    promises.forEach((promise) => {
      Promise.resolve(promise).then(resolve).catch(reject);
    });
  });
}

// Пример использования
const promise1 = new Promise((resolve, reject) =>
  setTimeout(resolve, 100, "one")
);
const promise2 = new Promise((resolve, reject) =>
  setTimeout(resolve, 200, "two")
);
const promise3 = new Promise((resolve, reject) =>
  setTimeout(resolve, 300, "three")
);

promiseRace([promise1, promise2, promise3]).then((value) => {
  console.log(value); // Выведет 'one'
});
```
