# Methods

* Instance Method: 클래스/구조체/ENUM 등의 인스턴스에서 호출할 수 있는 메소드
* Type Method: 클래스/구조체/ENUM의 인스턴스 없이 호출할 수 있는 메소드(static method 같은거)

structure와 enum은 밸류 타입이기 때문에 메소드 내에서 프로퍼티를 수정할 수 없다. 프로퍼티 수정을 하는 메소드의 경우에는 `mutating` 키워드를 표기한다.
```
struct Point {
    var x = 0.0, y = 0.0
    mutating func moveBy(x deltaX: Double, y deltay: Double) {
        x += deltaX
        y += deltaY
    }
}
```
이 때 Point를 상수로 선언하면 프로퍼티 역시 상수로 적용되기 때문에 `moveBy`를 호출하면 에러가 발생한다.

`mutating` 메소드는 자기자신에 새 인스턴스를 할당할 수 있다.
```
struct Point {
    var x = 0.0, y = 0.0
    mutating func moveBy(x deltaX: Double, y doubleY: Double) {
        self = Point(x: x + deltaX, y: y + deltaY)
    }
}
```

이를 통해 다음과 같은 응용이 가능하다.
```
enum TriStateSwitch {
    case off, low, high
    mutating func next() {
        switch self {
        case .off:
            self = .low
        case .low:
            self = .high
        case .high:
            self = .off
        }
    }
}
```

타입 메소드는 다음과 같이 구현한다.
```
class SomeClass {
    class func someTypeMethod() {
        // implementation
    }
}
SomeClass.someTypeMethod()  // call type method
```

타입 메소드에서 `self`를 호출하면 타입의 인스턴스가 아닌 타입 자체를 참조하게 된다.