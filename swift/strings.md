### Substring

```
let greeting = "Hello, world!"
let index = greeting.index(of: ",") ?? greeting.endIndex
let beginning = greeting[..<index]  // beginning is 'Substring'

let newString = String(beginning)   // convert to String
```
* String 객체를 쪼갰을 때 나오는 결과값은 Substring객체이다.
* Substring은 원본 String의 메모리 일부를 재활용하는 식으로 성능 최적화가 되어있다.
* 기존 String 역시 비슷한 최적화를 할 수 있지만 두 개의 String이 메모리를 공유한다면 그들은 동일(==)한 것이다.
* 하지만 원본 String의 메모리를 참조하는 것이기 때문에 오랫동안 사용하기에 좋지 않다(원본 String을 유지하고 있어야함).

    String과 Substring은 둘 다 StringProtocol을 따르고 있음


### 비교
문자열 값을 비교하는 방법에는 세가지가 있다.
* string & character
* prefix
* suffix

#### String & Character
== 와 != 를 사용한다.
```
let quot = "This is Same."
let quotSame = "This is Same."

if quot == quotSame {   // true
    print("Same Strings")
}
```
그 외에도 어큐트 억센트가 붙은 라틴 소문자 é(U+00E9)와 라틴 소문자 e(U+0065)와 어큐트 액센트를 합치는 문자(U+0301)는 동일하다고 간주된다.
`"\u{E9}" == "\u{65}\u{301}" // true`

하지만 라틴 대문자 A(U+0041, "A")과 키릴 대문자 A(U+0410, "A")는 동일하지 않다고 간주된다.
`"\u{41}" == "\u{0410}" // false`

#### Prefix & Suffix
접두사, 접미사를 비교하는 방식이다. 각각 `hasPrefix(_:)`와 `hasSuffix(_:)`를 통해 Bool값을 반환한다.

### String의 Unicode 표기
String값은 UTF-8, UTF-16, UTF-32(Unicode scalar)값으로 표기될 수 있다.
```
let dog = "Dog‼🐶"
for codeUnit in dog.utf8 {
    print("\(codeUnit) ", terminator: "")
    // Prints "68 111 103 226 128 188 240 159 144 182 "
}
for codeUnit in dog.utf16 {
    print("\(codeUnit) ", terminator: "")
    // Prints "68 111 103 8252 55357 56374 "
}
for scalar in dog.unicodeScalars {
    print("\(scalar.value) ", terminator: "")
    // Prints "68 111 103 8252 128054 "
}
for scalar in dog.unicodeScalars {
    print("\(scalar) "
    // D
    // o
    // g
    // ‼
    // 🐶
}
```
위와 같이 접근할 수 있다.

