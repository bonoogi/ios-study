# Collection Types

* Array - 정렬된 값 모음
* Set - 정렬되지 않은 유일값 모음
* Dictionary - 정렬되지 않은 키-밸류 쌍 모음

변수로 초기화하면 mutable, 상수로 초기화하면 immutable.

### Array
스위프트의 Array는 Foundation의 NSArray와 연결되어있다.
 
index와 값이 모두 필요할 때,
```
let someArr = ["aaa", "bbb", "ccc"]
for (index, value) in someArr.enumerated() {
    print("\(index + 1): \(value)")
}
```

### Set
Set는 Foundation의 NSSet과 연결되어있다.

set에 들어가기 위한 값은 `hashable`이어야 한다. 그 말인즉슨, 값의 타입이 `hash value`를 계산해서 제공할 수 있어야 한다는 뜻. `a == b`라면 `a.hashValue == b.hashValue`여야 한다.

기본 타입은 모두 hashable이며, 커스텀 타입의 경우에도 `Hashable` 프로토콜을 따르면서 Int값의 HashValue를 만들어 제공할 수 있다.

Hashable 프로토콜은 Equatable을 따르고 있어 비교연산자를 사용할 수 있음.

Set은 초기값으로 추론될 수는 없지만, Generic은 생략 가능하다.
```
var letters = Set<Character>()
var genres: Set<String> = ["Rock", "EDM"]
var gen: Set = ["HH", "AA"]
```

Array와 마찬가지로 remove를 수행할때 제거되는 값이 반환된다.

```
if let removedGenre = genres.remove("Rock") {
    print("\(removedGenre) is removed")
} else {
    print("없음")
}
```

순서는 없지만 for-in 루프가 가능하며, sortet()로 정렬된 for-in 루프가 가능.

#### New Set from two sets
* intersection(_:): 두 세트 사이의 공통값을 모은 새 세트를 반환
* symmetricDifference(_:): 두 세트 사이의 공통값을 제외하고 모든 값을 합친 새 세트를 반환
* union(_:): 두 세트를 모두 합친 새 세트 반환
* subtracting(_:): 주체가 되는 세트에서 대상이 되는 세트의 공통값을 제외한 새 세트를 반환

#### Compare Sets
* == : 동일한 값을 갖고있는 Set인지 판별
* isSubset(of:): 주체가 되는 세트가 대상이 되는 세트와 동일하거나 포함되는 세트인지 판별
* isSuperset(of:): 주체가 되는 세트가 대상이 되는 세트와 동일하거나 포함하는 세트인지 판별
* isStrictSubset(of:): subset에서 두 세트가 동일한 경우를 제외함
* isStrictSuperset(of:): superset에서 두 세트가 동일한 경우를 제외함
* isDisjoint(with:): 두 세트가 공통값이 없는 경우를 판별

### Dictionary
마찬가지로 Foundation의 NSDictionary와 연결되어있음.

* `Dictionary<Key, Value>`의 형태로 표기, 여기서 Key는 hashable이어야 함.
* [Key: Value]의 형태로 축약 표기할 수 있다.

```
var dict = [Int: String]()
dict[15] = "Fifteen"
dict = [:] // empty dictionary. 이미 Generic 형이 선언되어있기 때문에 타입을 생략.
```

초기화할때는 타입을 생략할 수 있다
```
var cases: [String: String] = ["A": "a", "B": "b"]
var cases = ["A": "a", "B": "b"]
```
위의 두 구문은 동일하다.

```
if let oldVal = cases.updateValue("c", forKey: "C") {
    print("oldVal is \(oldVal)")
} else {
    print("There is no key C") // 여기가 실행됨
}

cases["C"] = nil    // "C"는 삭제됨
```
