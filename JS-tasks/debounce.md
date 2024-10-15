# Debounce

```javascript
/**
 * Функция для дебаунсинга вызова другой функции.
 * @param {Function} callback - Функция, которую нужно дебаунсировать.
 * @param {number} delay - Задержка в миллисекундах, через которую функция будет вызвана.
 * @returns {Function} - Возвращаемая функция, которая выполняет дебаунсинг.
 */
const debounce = (callback, delay) => {
  let timer; // Таймер для отслеживания задержки.

  return function (...args) {
    clearTimeout(timer); // Очистка предыдущего таймера.

    timer = setTimeout(() => {
      callback.apply(this, args); // Вызов функции с текущим контекстом и переданными аргументами.
    }, delay);
  };
};

// Пример использования
const log = debounce((message) => {
  console.log(message);
}, 1000);

log("Hello"); // Не выведет ничего, так как функция находится в состоянии ожидания.
log("World"); // Не выведет ничего, так как функция находится в состоянии ожидания.

setTimeout(() => {
  log("Hello again"); // Выведет "Hello again" через 1 секунду после последнего вызова.
}, 1100);
```
