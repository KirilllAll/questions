# Реализация Promise.any

Функция `Promise.any` возвращает промис, который разрешается первым разрешенным промисом из переданного массива промисов. Если все промисы отклоняются, возвращаемый промис отклоняется с ошибкой `AggregateError`, содержащей все ошибки отклоненных промисов.

## Пример использования

```javascript
const promise1 = new Promise((resolve, reject) =>
  setTimeout(reject, 100, "one")
);
const promise2 = new Promise((resolve, reject) =>
  setTimeout(resolve, 200, "two")
);
const promise3 = new Promise((resolve, reject) =>
  setTimeout(reject, 300, "three")
);

Promise.any([promise1, promise2, promise3])
  .then((value) => {
    console.log(value); // Выведет 'two'
  })
  .catch((error) => {
    console.error(error); // Не будет вызвано, так как promise2 разрешается
  });
```

## Реализация

```javascript
function promiseAny(promises) {
  return new Promise((resolve, reject) => {
    if (!Array.isArray(promises)) {
      return reject(new TypeError("Argument is not an array of promises"));
    }

    let resolved = false;
    let errors = [];

    promises.forEach((promise, index) => {
      Promise.resolve(promise)
        .then((value) => {
          if (!resolved) {
            resolved = true;
            resolve(value);
          }
        })
        .catch((error) => {
          errors[index] = error;
          if (errors.length === promises.length) {
            reject(new AggregateError(errors, "All promises were rejected"));
          }
        });
    });
  });
}

// Пример использования
const promise1 = new Promise((resolve, reject) =>
  setTimeout(reject, 100, "one")
);
const promise2 = new Promise((resolve, reject) =>
  setTimeout(resolve, 200, "two")
);
const promise3 = new Promise((resolve, reject) =>
  setTimeout(reject, 300, "three")
);

promiseAny([promise1, promise2, promise3])
  .then((value) => {
    console.log(value); // Выведет 'two'
  })
  .catch((error) => {
    console.error(error); // Не будет вызвано, так как promise2 разрешается
  });
```
