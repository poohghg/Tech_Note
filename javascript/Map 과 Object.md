`Map`과 `Object`는 둘 다 키-값 저장을 위한 데이터 구조이지만, 내부 동작 방식과 성능 면에서 중요한 차이점이 있다.
## **1. 주요 차이점 비교**

|특성|`Object`|`Map`|
|---|---|---|
|**키 타입**|문자열(`string`) 또는 심볼(`Symbol`)만 가능|모든 타입 가능 (`number`, `string`, `object`, `function` 등)|
|**키 순서 유지**|❌ 보장되지 않음|✅ 삽입된 순서 유지|
|**성능** (조회)|보통 `O(1)` (해시 테이블 기반)|`O(1)` (해시 테이블 기반)|
|**성능** (삽입/삭제)|`O(1)`, 하지만 해시 충돌 시 성능 저하 가능|`O(1)`, 더 최적화됨|
|**has() 지원**|`key in obj` 또는 `Object.hasOwn(obj, key)` 사용해야 함|`map.has(key)`로 간단하게 가능|
|**크기 확인**|`Object.keys(obj).length` 사용 (비효율적)|`map.size` 속성 제공 (빠름)|
|**반복(iteration)**|`Object.keys()`, `Object.values()`, `for...in` 등 사용해야 함|`map.forEach()`, `map.keys()`, `map.entries()` 등 기본 제공|
|**JSON 직렬화**|`JSON.stringify()`로 가능|직접 변환해야 함 (예: `Array.from(map.entries())`)|
|**GC (가비지 컬렉션)**|객체가 참조되지 않으면 자동 제거됨|`WeakMap` 사용 시 자동 관리 가능|

---

## **2. 성능 비교**

일반적으로 `Map`은 **객체보다 키 검색, 추가, 삭제에서 더 좋은 성능을 보입니다**.  
특히 키가 문자열이 아닌 경우 (`number`, `object` 등) `Object`보다 `Map`이 훨씬 유용합니다.

```ts
const obj: Record<number, string> = {};
const map = new Map<number, string>();

console.time("Object set");
for (let i = 0; i < 100_000; i++) {
  obj[i] = "value";
}
console.timeEnd("Object set");

console.time("Map set");
for (let i = 0; i < 100_000; i++) {
  map.set(i, "value");
}
console.timeEnd("Map set");

console.time("Object get");
for (let i = 0; i < 100_000; i++) {
  obj[i];
}
console.timeEnd("Object get");

console.time("Map get");
for (let i = 0; i < 100_000; i++) {
  map.get(i);
}
console.timeEnd("Map get");
```

💡 **결과 예시 (실행 환경에 따라 다름)**

```
Object set: 8ms
Map set: 4ms
Object get: 5ms
Map get: 2ms
```

👉 `Map`이 **삽입과 조회 속도에서 더 빠른 경향**이 있습니다.

---

## **3. 어떤 경우에 `Map`을 쓰는 게 더 좋은가?**

✅ **키가 숫자이거나 객체일 때** (`Object`는 키를 문자열로 변환하므로 `Map`이 적합)  
✅ **많은 데이터 저장 시** (`Map`이 더 성능이 좋고 메모리 관리가 효율적)  
✅ **데이터 순서가 중요할 때** (`Map`은 순서를 유지하지만 `Object`는 보장되지 않음)  
✅ **자주 키를 추가/삭제할 때** (`Map`이 `delete` 성능이 더 좋음)

---

## **4. 언제 `Object`를 쓰는 게 더 좋은가?**

✅ **JSON과 상호작용할 때** (`Object`는 JSON 변환이 간편함)  
✅ **단순한 key-value 저장용일 때** (`Object`가 기본 내장 타입이라 코드가 가벼움)  
✅ **필요한 키가 고정되어 있을 때** (`Object`는 프로토타입을 활용할 수도 있음)

---

## **5. 결론**

일반적으로 **빠른 키-값 저장/검색이 필요하면 `Map`을 쓰는 게 좋습니다**.  
하지만 JSON 변환이 자주 필요하거나 단순한 데이터 구조라면 `Object`도 충분히 유용합니다.

🚀 **추천 가이드**

- **동적인 키-값 저장 & 삭제가 많다면?** → `Map`
- **단순한 속성 저장 및 JSON 변환이 필요하다면?** → `Object`
- **숫자 키를 많이 사용한다면?** → `Map` (객체는 숫자를 문자열로 변환하기 때문)
- **순서가 중요한 데이터라면?** → `Map` (순서를 유지함)

👉 **큐(`SimpleQueue<T>`)의 경우 `Map`이 더 적합**합니다.

- `dequeue()` 시 `delete` 연산이 자주 발생 → `Map`이 더 빠름
- `Object`는 키 순서가 예측 불가능할 수도 있음 → `Map`이 더 안정적

따라서 현재 `SimpleQueue<T>` 구현에서는 **`Map`을 사용하는 게 좋은 선택**입니다! 🚀
