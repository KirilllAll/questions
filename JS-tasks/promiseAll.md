# Реализовать метод Promise.all как будто его нет в API Promise

## Promise.all принимает массив promises и возвращает все успешные promise или reject 1го promise

### Реализация

```javascript
const myPromiseAllNew = (allPromises) => {
  return new Promise((resolve, reject) => {
    if (allPromises.length === 0) {
      // function guard
      resolve([]);
      return;
    }

    let countPromises = 0; // счетчик
    let result = new Array(allPromises.length); // массив определенной длинны

    allPromises.forEach((promise, index) => {
      // итерации
      Promise.resolve(promise) // resolve каждого промис
        .then((val) => {
          result[index] = val; // запись в массив
          countPromises += 1; // счетчик

          if (countPromises === allPromises.length) {
            resolve(result); // resolve результата
          }
        })
        .catch((err) => {
          reject(err); // reject
        });
    });
  });
};
```

## Заключение

### Данная задача рассчитана на понимание promise и promise all
