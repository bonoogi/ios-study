# Properties

* stored property: '=' 할당 연산자로 값을 할당한 프로퍼티
* computed prroperty: {} 로 감싸 값을 계산 후 반환하게 되어있는 프로퍼티
* stored property는 override할 수 없다. 대신 computed property는 가능.
* static property의 경우 static 대신 class 키워드를 붙여서 override를 허용할 수 있음. 물론 class 키워드는 computed property에 한정해서 사용 가능.
* stored property는 observer(`didSet`, `willSet`)을 override할 수 있다.

`var`, `func`, `class`, `subscript` 등의 override를 막기 위해서는 `final` 키워드를 부여하면 된다.