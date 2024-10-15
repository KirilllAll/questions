# limit

```javascript
// Написать декоратор который принимает фукнцию лимит и колбэк
// - Функцию можно вызывать столько раз сколько указано в лимите
// - Колбэк вызывается опционально при последнем вызове функции
// - функция должна иметь метод ресет обнуляющий кол-во вызовов

function limit(fn, limit, callback) {
  let count = 0;

  function limited(...args) {
    count += 1;
    if (count <= limit) {
      fn.apply(this, args);
    }
    if (count >= limit && callback) {
      callback();
    }
  }
  limited.reset = function () {
    count = 0;
  };

  return limited;
}

const log = (title, name) => console.log(title, name);

const limitLog = limit(log, 3);
```
