# Вопросы на знания методов массива и умения их реализовывать

## Reduce

```javascript
Array.prototype.myReduce = function (cb, initialState) {
  let values = this;
  values.forEach((item, index) => {
    initialState = cb(initialState, item, index, values);
  });
  return initialState;
};

const result = [1, 2, 3, 4].myReduce((acc, item) => (acc += item), 0);
```

## Map

```javascript

```

## Some

```javascript

```

## Every

```javascript

```
