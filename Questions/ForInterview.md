# LIST

### `.forEach` 와 `.map()` 루프 사이의 주요 차이점

이 두 함수의 차이점을 이해하기 위해 각 함수가 무엇을 하는지 살펴보겠습니다.

**`forEach`**

- 배열의 요소를 반복합니다.
- 각 요소에 대한 콜백을 실행합니다.
- 값을 반환하지 않습니다.

```js
const a = [1, 2, 3];
const doubled = a.forEach((num, index) => {
  // num 및 / 또는 index로 무엇이든 해보세요.
});

// doubled = undefined
```

**`map`**

- 배열의 요소를 반복합니다.
- 각 요소에서 함수를 호출하여 결과로 새 배열을 작성하여 각 요소를 새 요소에 매핑합니다.

```js
const a = [1, 2, 3];
const doubled = a.map(num => {
  return num * 2;
});

// doubled = [2, 4, 6]
```

`.forEach` 와 `.map()` 의 주된 차이점은 `.map()` 이 새로운 배열을 반환한다는 것입니다. 결과가 필요하지만 원본 배열을 변경하고 싶지 않으면 `.map()` 이 확실한 선택입니다. 단순히 배열을 반복할 필요가 있다면, forEach 가 좋은 선택입니다.
