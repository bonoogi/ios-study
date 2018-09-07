# 함수

### In-Out Parameters
```
func someFunction(param: inout Int) {
    param = 15
}

var someInt = 13
someFunction(param: &someInt) // now someInt is 15
```

* 일반 파라메터: Call by Value
* inout 파라메터: Call by Reference

#### 제약
* var(변수)만 가능
* 파라메터 기본값/variadic(Type...) 불가
* 함수에 변수를 넣을 시 변수 앞에 &를 붙여야 함