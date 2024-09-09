## Structural Typing 예시

아래 코드에서 `isDisabled` 가 정의되지 않았는데 데이터를 생성할 때 `isDisabled` 가 추가되어도 타입 에러가 나지 않는 이유에 대한 설명

```tyepscript
// 타입 정의
export interface GroupOption {
  label: string;
  value: {
    id: number;
  };
}

// 데이터 생성
const options = useMemo(() => {
  const result: GroupOption[] | undefined = data?.items.map(({ name, id }) => ({
    label: name,
    value: {
      id,
    },
    isDisabled: entityIds.includes(id),
  }));

  return result ?? [];
}, [data?.items, entityIds]);
```

GroupOption 타입에 `isDisabled` 속성이 명시적으로 정의되어 있지 않지만, options 데이터를 만들 때 `isDisabled` 속성을 추가해도 타입상 문제가 되지 않는 이유는 TypeScript의 구조적 타이핑(Structural Typing) 때문입니다. TypeScript는 객체의 형태(shape)에 기반하여 타입을 검사합니다. 즉, 객체가 필요한 속성을 모두 가지고 있으면, 추가적인 속성이 있어도 타입 검사에 통과합니다. 

예시 설명

GroupOption 타입은 다음과 같이 정의되어 있습니다:

```typescript
const options = useMemo(() => {
  const result: GroupOption[] | undefined = data?.items.map(({ name, id }) => ({
    label: name,
    value: {
      id,
    },
    isDisabled: entityIds.includes(id), // 추가된 속성
  }));

  return result ?? [];
}, [data?.items, entityIds]);
```

하지만 options 배열을 생성할 때, `isDisabled` 속성을 추가하고 있습니다:

```typescript
const options = useMemo(() => {
  const result: GroupOption[] | undefined = data?.items.map(({ name, id }) => ({
    label: name,
    value: {
      id,
    },
    isDisabled: entityIds.includes(id), // 추가된 속성
  }));

  return result ?? [];
}, [data?.items, entityIds]);
```

이유
1. 구조적 타이핑: TypeScript는 구조적 타이핑을 사용합니다. 즉, 객체가 필요한 속성을 모두 가지고 있으면, 추가적인 속성이 있어도 타입 검사에 통과합니다. GroupOption 타입에 label과 value 속성이 있으므로, `isDisabled` 속성이 추가되어도 타입 검사에 문제가 없습니다.
2. 타입 확장: GroupOption 타입에 명시적으로 정의되지 않은 속성을 추가하는 것은 타입 확장의 한 형태입니다. 이는 TypeScript에서 허용됩니다.

해결 방법
만약 isDisabled 속성을 GroupOption 타입에 명시적으로 추가하고 싶다면, 타입 정의를 다음과 같이 수정할 수 있습니다:

```typescript
export interface GroupOption {
  label: string;
  value: {
    id: number;
  };
  isDisabled?: boolean; // 선택적 속성으로 추가
}
```

이렇게 하면 isDisabled 속성이 GroupOption 타입에 명시적으로 포함되며, 타입 정의와 실제 데이터 구조가 일치하게 됩니다.
