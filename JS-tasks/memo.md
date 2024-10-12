# Задача на замыкание, контекст и умение писать декораторы

## Функция memo используется для мемоизации (запоминания) результатов вычислений функции.

### Написать функцию, которая запоминает результат своего вызова и с такими параметрами будет возвращать результат их store/cache

```javascript
const memo = (callback) => {
  const cache = {};
  const memoizedCall = (...args) => {
    const key = [...args];
    if (cache[key]) {
      return `${cache[key]} should be taken from cache`;
    }
    const result = callback(...args);
    cache[key] = result;
    return result;
  };

  memoizedCall.cache = cache;
  return memoizedCall;
};
const sum = memo((a, b) => a + b);

console.log(sum(1, 2)); // 3
console.log(`2: ${sum(2, 3)}`); // 2: 5
console.log(`2: ${sum(2, 3)}`); // 2: 5 should be taken from cache
console.log("cache", sum.cache); // cache {[1,2]: 3, [2,3]: 5}
```

## Подробнее о том что происходит в коде:

```javascript
const memo = (callback) => {
  // cache будет хранить результаты вызовов функции для разных аргументов.
  const cache = {};

  // в качестве хранилища можно использовать `new Map`

  // memoizedCall - это функция, которая обертывает исходную функцию и добавляет кэширование.
  const memoizedCall = (...args) => {
    const key = JSON.stringify(args); // Создаем ключ для кэша.

    // Проверяем, есть ли уже результат для этих аргументов в кэше.
    if (cache[key]) {
      return `${cache[key]} should be taken from cache`; // Если есть, возвращаем результат из кэша.
    }

    // Если результата в кэше нет, вычисляем результат, вызывая исходную функцию с переданными аргументами.
    const result = callback(...args);

    // Сохраняем результат в кэш по ключу args.
    cache[key] = result;

    // Возвращаем вычисленный результат.
    return result;
  };

  // Привязываем к функции свойство cache для доступа к сохраненным значениям.
  memoizedCall.cache = cache;

  // Возвращаем мемоизированную функцию.
  return memoizedCall;
};

// Пример использования: функция суммирования, которую мы мемоизируем с помощью memo.
const sum = memo((a, b) => a + b);

// Первый вызов функции sum(1, 2), результата еще нет в кэше, вычисляем и сохраняем.
console.log(sum(1, 2)); // 3

// Второй вызов sum(2, 3), результат не кэширован, поэтому вычисляется заново.
console.log(`2: ${sum(2, 3)}`); // 2: 5

// Третий вызов с теми же аргументами sum(2, 3). Теперь результат должен браться из кэша.
console.log(`2: ${sum(2, 3)}`); // 2: 5 should be taken from cache

// текущее состояние кэша.
console.log("cache", sum.cache); // cache {[1,2]: 3, [2,3]: 5}
```
