# One Run

```javascript
// Необходимо написать функцию-декоратор, которая бы позволяла исполнялась функцию только один раз.

/**
 * Decorator for once run
 * @param {Function} callback The fn
 */

function fn(cb) {
  let countCall = false;
  let res;
  return function (...args) {
    if (!countCall) {
      res = cb.apply(this, args);
      countCall = true;
      return cb.apply(this, args);
    } else {
      return res;
    }
  };
}
```
