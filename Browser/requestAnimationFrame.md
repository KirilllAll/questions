# Что такое `requestAnimationFrame`?

`requestAnimationFrame` — это метод браузера, который оптимизирует анимации. Он указывает браузеру, что нужно выполнить анимацию, и просит его запланировать перерисовку перед следующей перерисовкой кадра. Это делает анимации плавными и синхронизирует их с частотой обновления экрана (обычно 60 FPS).

## Пример работы с `requestAnimationFrame`

Для использования `requestAnimationFrame` в React, например, для плавного изменения стейта, можно воспользоваться хуком `useEffect`.

### Пример кода:

```jsx
import React, { useState, useEffect } from "react";

const AnimationFrameComponent = () => {
  const [position, setPosition] = useState(0);
  const [isAnimating, setIsAnimating] = useState(false);

  useEffect(() => {
    let animationFrameId;

    const animate = () => {
      setPosition((prev) => prev + 1);
      animationFrameId = requestAnimationFrame(animate);
    };

    if (isAnimating) {
      animationFrameId = requestAnimationFrame(animate);
    }

    return () => cancelAnimationFrame(animationFrameId);
  }, [isAnimating]);

  return (
    <div>
      <div style={{ transform: `translateX(${position}px)` }}>🚀</div>
      <button onClick={() => setIsAnimating(!isAnimating)}>
        {isAnimating ? "Stop" : "Start"} Animation
      </button>
    </div>
  );
};

export default AnimationFrameComponent;
```

**Описание:**

- Когда пользователь нажимает кнопку "Start Animation", начинается цикл вызовов `requestAnimationFrame`, который плавно увеличивает значение позиции `position`, перемещая элемент по горизонтали.
- Анимация синхронизирована с частотой обновления экрана, что позволяет делать её плавной.
- При каждом рендере мы отменяем предыдущую анимацию с помощью `cancelAnimationFrame`, чтобы избежать утечек памяти.

## Оптимизация с помощью requestAnimationFrame

### Почему это важно:

- В `React`, особенно с часто изменяющимся состоянием, изменение компонентов может происходить слишком часто, что перегружает браузер.
- Используя `requestAnimationFrame`, можно "группировать" обновления, чтобы они выполнялись синхронно с рендерингом браузера, что снижает нагрузку на процессор.

**Пример использования с React Hook `useRef` для ещё большей оптимизации:**

```jsx
import React, { useState, useEffect, useRef } from "react";

const OptimizedAnimationFrameComponent = () => {
  const [position, setPosition] = useState(0);
  const requestRef = useRef();

  const animate = (time) => {
    setPosition((prev) => prev + 1);
    requestRef.current = requestAnimationFrame(animate);
  };

  useEffect(() => {
    requestRef.current = requestAnimationFrame(animate);
    return () => cancelAnimationFrame(requestRef.current);
  }, []);

  return <div style={{ transform: `translateX(${position}px)` }}>🚀</div>;
};

export default OptimizedAnimationFrameComponent;
```
