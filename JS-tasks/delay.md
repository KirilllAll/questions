# Delay

```javascript
/**
 * Функция для создания задержки перед выполнением другой функции.
 * @param {Function} callback - Функция, которую нужно выполнить с задержкой.
 * @param {number} delay - Задержка в миллисекундах перед выполнением функции.
 * @returns {Function} - Возвращаемая функция, которая выполняет переданную функцию с задержкой.
 */
const delay = (callback, delay) => {
  return function (...args) {
    setTimeout(() => {
      callback.apply(this, args);
    }, delay);
  };
};

// Пример использования
const log = delay((message) => {
  console.log(message);
}, 1000);

log("Hello"); // Выведет "Hello" через 1 секунду
log("World"); // Выведет "World" через 1 секунду
```
