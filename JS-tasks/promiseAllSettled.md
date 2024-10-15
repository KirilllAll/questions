# Реализация Promise.allSettled

Функция `Promise.allSettled` возвращает промис, который разрешается, когда все промисы в переданном массиве завершаются (разрешаются или отклоняются). Возвращаемый промис разрешается массивом объектов, каждый из которых описывает результат соответствующего промиса.

## Пример использования

```javascript
const promise1 = new Promise((resolve, reject) =>
  setTimeout(resolve, 100, "one")
);
const promise2 = new Promise((resolve, reject) =>
  setTimeout(reject, 200, "two")
);
const promise3 = new Promise((resolve, reject) =>
  setTimeout(resolve, 300, "three")
);

Promise.allSettled([promise1, promise2, promise3]).then((results) => {
  console.log(results);
  // Выведет:
  // [
  //   { status: 'fulfilled', value: 'one' },
  //   { status: 'rejected', reason: 'two' },
  //   { status: 'fulfilled', value: 'three' }
  // ]
});
```

## Реализация

```javascript
function promiseAllSettled(promises) {
  return new Promise((resolve, reject) => {
    if (!Array.isArray(promises)) {
      return reject(new TypeError("Argument is not an array of promises"));
    }

    const results = [];
    let settledCount = 0;

    promises.forEach((promise, index) => {
      Promise.resolve(promise)
        .then((value) => {
          results[index] = { status: "fulfilled", value };
        })
        .catch((reason) => {
          results[index] = { status: "rejected", reason };
        })
        .finally(() => {
          settledCount += 1;
          if (settledCount === promises.length) {
            resolve(results);
          }
        });
    });
  });
}

// Пример использования
const promise1 = new Promise((resolve, reject) =>
  setTimeout(resolve, 100, "one")
);
const promise2 = new Promise((resolve, reject) =>
  setTimeout(reject, 200, "two")
);
const promise3 = new Promise((resolve, reject) =>
  setTimeout(resolve, 300, "three")
);

promiseAllSettled([promise1, promise2, promise3]).then((results) => {
  console.log(results);
  // Выведет:
  // [
  //   { status: 'fulfilled', value: 'one' },
  //   { status: 'rejected', reason: 'two' },
  //   { status: 'fulfilled', value: 'three' }
  // ]
});
```
