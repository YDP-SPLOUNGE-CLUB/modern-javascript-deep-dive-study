# 37. Set 과 Map

## 37.1 Set

> Set 객체는 중복되지 않는 유일한 값들의 집합이다. Set 객체는 배열과 유사하지만 다음과 같은 차이가 있다.

| 구분                    | 배열  | Set 객체 |
|-----------------------|-----|--------|
| 동일한 값을 중복하여 포함할 수 있다. | O   | X      |
| 요소 순서에 의미가 있다.        | O   | X      |
| 인덱스로 요소에 접근할 수 있다.    | O   | X      |

## 37.1.1 Set 객체의 생성

> Set 객체는 Set 생성자 함수로 생성한다. Set 생성자 함수에 인수를 전달하지 않으면 빈 Set 객체가 생성된다.

```javascript
const set = new Set();
console.log(set); // Set(0) {}
```

> Set 생성자 함수는 이터러블 인수를 전달받아 Set 객체를 생성한다. 이때 이터러블의 중복된 값은 Set 객체에 요소로 저장되지 않는다.

```javascript
const set1 = new Set([1,2,3,4]);

console.log(set1);
```

```javascript
const uniq = array => array.filter((v, i, self) => self.indexOf(v) === i);
console.log(uniq([2,1,2,3,4,3,4])); // [2,1,3,4];

const uniq = array => [... new Set(array)];
```

### 37.1.2 요소 개수 확인

> Set 객체의 요소 개수를 확인할 때는 Set.prototype.size 프로퍼티를 사용한다.

```javascript
const { size } = new Set([1,2,3,3]);

console.log(size); // 3
```

### 37.1.3 요소 추가

> Set 객체에 요소를 추가할 때는 Set.prototype.add 메서드를 사용한다.

```javascript
const set = new Set();
console.log(set); // Set(0) {}

set.add(1);
console.log(set); // Set(1) {1}
```

> Set 은 NaN === NaN 을 true 라 판단하여 중복 추가를 해도 추가되지 않는다.

> Set 객체는 객체나 배열과 같이 자바스크립트의 모든 값을 요소로 저장할 수 있다.

### 37.1.4 요소 존재 여부 확인

```javascript
const set = new Set([1,2,3]);

console.log(set.has(1)); // true
console.log(set.has(55)); // false
```

### 37.1.5 요소 삭제

```javascript
const set = new Set([1,2,3]);

set.delete(1);
console.log(set); // [2,3];
```

### 37.1.6 요소 일괄 삭제

```javascript
const set = new Set([1,2,3]);

set.clear();
console.log(set); // Set(0) {}
```

### 37.1.7 요소 순회

> forEach 메서드를 사용하여 순회 가능하다.

> Set 객체는 이터러블이다. 따라서 for ... of 문으로 순회할 수 있으며, 스프레드 문법과 배열 디스트럭처링의 대상이 될 수도 있다.

```javascript
const set = new Set([1, 2, 3]);

console.log(Symbol.iterator in set); // true

const [a, ...rest] = set;
console.log(a, rest); // 1, [2,3]
```

### 37.1.8 집합 연산

> Set 객체는 수학적 집합을 구현하기 위한 자료구조다. 따라서 Set 객체를 통해 교집합 합집합 차집합 등을 구현 가능하다.

교집합

```javascript
Set.prototype.intersection = function (set) {
    const result = new Set();
    
    for (const value of set) {
        if (this.has(valye)) result.add(value);
    }
    
    return result;
}
```

합집합

```javascript
Set.prototype.intersection = function (set) {
    return new Set([...this, ...set]);
}
```

차집합

```javascript
Set.prototype.intersection = function (set) {
    return new Set([ ... this].filter(v => !set.has(v)));
}
```

## 37.2 Map

> Map 객체는 키와 값이 쌍으로 이루어진 컬렉션이다. Map 객체는 객체와 유사하지만 다음과 같은 차이가 있다.

| 구분            | 객체                      | Map 객체       |
|---------------|-------------------------|--------------|
| 키로 사용할 수 있는 값 | 문자열 또는 심벌 값             | 객체를 포함한 모든 값 |
| 이터러블          | X                       | O            |
| 요소 개수 확인      | Object.keys(obj).length | map.size     |

### 37.2.1 Map 객체의 생성

> Map 객체는 Map 생성자 함수로 생성한다.

> Map 생성자 함수는 이터러블을 인수로 전달받아 Map 객체를 생성한다. 이때 인수로 전달되는 이터러블은 키와 값의 쌍으로 이루어진 요소로 구성되어야 한다.

### 37.2.2 요소 개수 확인

> Map 객체의 요소 개수 Map.prototype.size 로 확인

### 37.2.3 요소 추가

> Map 객체의 요소를 추가할 때는 Map.prototype.set 메서드를 사용

### 37.2.4 요소 취득

```javascript
const map = new Map();

const lee = { name: 'Lee' };
const kim = { name: 'Kim' };

map.set(lee, "1");
map.set(kim, "asd");

console.log(map.get(lee)); // 1
```

### 37.2.5 요소 존재 여부 확인

> Map 객체에 특정 요소를 확인하려면 Map.prototype.has 메서드를 사용한다.

### 37.2.6 요소 삭제

> Map 객체의 요소를 삭제하려면 Map.prototype.delete 메서드를 사용한다. delete 메서드는 삭제 성공 여부를 나타내는 불리언 값을 반환한다.

### 37.2.7 요소 일괄 삭제

> Map 객체의 요소를 일괄 삭제하려면 Map.prototype.clear 메서드를 사용한다.

### 37.2.8 요소 순회

> Map 객체의 요소를 순회하려면 Map.prototype.forEach 메서드를 사용한다.

```javascript
const lee = { name: 'Lee' };
const kim = { name: 'Kim' };

const map = new Map([[lee, "developer"], [kim, "designer"]]);

map.forEach((v, k, map) => {
    console.log(v, k, map); 
});

// developer {name: 'Lee'} Map(2) {{…} => 'developer', {…} => 'designer'}
// developer {name: 'Kim'} Map(2) {{…} => 'desinger', {…} => 'designer'}
```

> Map 객체는 이터러블이다. for .. of 문으로 순회 가능하다. 스프레드 문법과 디스트럭처링 할당이 가능하다.

---

- [ ] Map 디스트럭처링 할당을 예시를 코드로 적어보세용
