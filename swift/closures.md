# Closures
람다 비슷한거 ㅡㅡ;;

* 선언된 스코프 내의 어떤 상수/변수든 참조를 capture하고 저장할 수 있다.
* 전역 함수, 혹은 중첩된 함수가 실제로는 클로져의 특별 케이스임.

#### 클로져 표현식
```
{(PARAMETER) -> RETURN_TYPE in
    STATEMENTS
}
```

```
let names = ["Chris", "Alex", "Ewa", "Barry", "Daniella"]

// 중첩 함수
func backward(_ s1: String, _ s2: String) -> Bool {
    return s1 > s2
}
var resersedNames = names.sorted(by: backward)

// 클로저 기본
reversedNames = names.sorted(by: { (s1: String, s2: String) -> Bool in
    return s1 > s2
})

// 한줄로 표현
reversedNames = names.sorted(by: { (s1: String, s2: String) -> Bool in return s1 > s2 })

// 타입 추론 표현
reversedNames = names.sorted(by: { s1, s2 in return s1 > s2 })

// 축약 표현
reversedNames = names.sorted(by: { s1, s2 in s1 > s2 })

// 파라메터 선언 생략
reversedNames = names.sorted(by: { $0 > $1 } )

// 연산자 함수 사용
reversedNames = names.sorted(by: >)
```

클로저는 스코프 내의 변수/상수를 캡처한다고 했는데, 클로저 자체는 참고 타입이기 때문에 인스턴스화 이후에 두 개의 다른 변수에 할당되었을때 두 변수는 클로저 내의 캡처된 변수/상수를 공유하게 된다.

## escape closure
#### `@escaping`
클로져 인자의 타입 명시 앞에 붙어 해당 인자를 클로져 밖에서도 호출 할 수 있게 한다.
```
var completionHandlers: [() -> Void] = []

// @escaping을 통해 completionHandler가 함수 외부에서도 불릴수 있게 된다.
func someFuncEscaping(completionHandler: @escaping () -> Void) {
    completionHandlers.append(completionHandler)
}

func someFuncNoneEscaping(closure: () -> Void) {
    closure()
}

class SomeClass {
    var x = 10
    func doSomething() {
        someFuncNoneEscaping { x = 100 }
        // 외부에서도 호출될 수 있기 때문에 x를 호출할 때 self 키워드가 필요함
        someFuncEscaping { self.x = 200 }
    }
}

let instance = SomeClass()
instance.doSomething()
print(instance.x) // print 100
completionHandlers.first?()
print(instance.x) // print 200
```

## Autoclosure
#### `@autoclosure`
클로져 인자의 타입 명시 앞에 붙어 해당 인자를 {}로 감싸지 않아도 알아서 클로져로 인식할 수 있게 한다.
```
var customersInLine = ["C", "A", "E", "B", "D"]

func serve(customer customerProvider: @autoclosure () -> String) {
    print("Now serving \(customerProvider())")
}
serve(customer: customersInLine.remove(at: 0))
```

너무 많이 쓰면 이해하기 힘든 코드가 된다 

`@escaping`과 `@autoclosure`는 동시에 쓰일 수 있다.