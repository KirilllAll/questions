# Задача на умение работать со строками и массивами

## Написать функцию, которая получает на вход не отсортированны массив и возвращает строку с диапозоном чисел

```javascript
range([1, 4, 5, 2, 3, 9, 8, 11, 0])); // '0-5,8-9, 11'
range([1, 4, 3, 2])); // '1-4'
```

### Решение

```javascript
function range(arr) {
  if (!arr.length) return "";

  const sortedArr = [...arr].sort((a, b) => a - b);

  let result = [];
  let start = sortedArr[0];

  for (let i = 1; i <= sortedArr.length; i++) {
    const current = sortedArr[i];
    const previous = sortedArr[i - 1];

    if (current - previous !== 1) {
      if (start === previous) {
        result.push(`${start}`);
      } else {
        result.push(`${start}-${previous}`);
      }
      start = current;
    }
  }

  return result.join(",");
}
```

### Подробнее о том что происходит в коде:

```javascript
function range(arr) {
  if (!arr.length) return ""; // проверка на длинну массива

  const sortedArr = [...arr].sort((a, b) => a - b); // создадим копию что бы не мутировать массив по ссылке

  let result = [];
  let start = sortedArr[0]; // стартовая позиция 1 элемент отсортированного массива

  for (let i = 1; i <= sortedArr.length; i++) {
    const current = sortedArr[i]; // текущее
    const previous = sortedArr[i - 1]; // предыдущее

    if (current - previous !== 1) {
      // если не равна 1 разница то
      if (start === previous) {
        // они равны можем пушить 1 элемент
        result.push(`${start}`);
      } else {
        // иначе диапозон закончился
        result.push(`${start}-${previous}`);
      }
      start = current; // обновляем нашу стартовую позицию
    }
  }

  return result.join(","); // работали с массивом, а нужны строки
}
```
