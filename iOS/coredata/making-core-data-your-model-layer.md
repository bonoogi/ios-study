# 모델 계층의 코어 데이터 만들기

## 개요
코어 데이터는 앱 내 모델 계층 생성과 관리의 복잡함을 줄여준다. 코어 데이터를 사용해 데이터 구조를 정의함으로 인해 일반적으로 요구되는 반복적인 코드 대부분을 제거할 수 있다.

[`NSManagedObject`](https://developer.apple.com/documentation/coredata/nsmanagedobject) 인스턴스를 데이터 구조로 사용하고, 그들 객체 사이의 관계를 데이터 모델을 사용해 정의한다.

## 코어 데이터와 앱을 통합하기
전통적인 코어 데이터 스택을 만들거나 `NSPersistentContainer`를 초기화함으로 앱 안에 코어 데이터의 이니셜 통합을 수행할 수 있다. 두 방법 모두 앱 번들에서 관리 객체 모델을 찾고 해당 모델을 사용하여 앱에서 사용할 수있는 관리 객체 (데이터 객체)를 정의합니다.

일반적으로, 코어 데이터는 앱이 시작되면서 초기화되고 그로 인해 앱의 사용자 인터페이스가 코어 데이터에 접근할 수 있게 됩니다. 왜냐하면 당신이 이 초기화를 당신의 뷰 구조 밖에서 시작했고, 이는 앱 델리게이트에서 프로세스를 시작하는 것이 추천되기 떄문입니다.

## Persistent Container 초기화
`NSPersistentContainer`는 코어 데이터 스택을 만드는 것을 단순화하고 서브클래싱을 위한 편리한 클래스를 제공하며 코어 데이터에 변리한 커스텀 메소드를 추가시켜줍니다. 다음 예시는 persistent container의 초기화를 보여줍니다:
```
let container = NSPersistentContainer(name: "DataModel")
container.loadPersistentStores(completionHandler: { (description, error) in
    if let error = error {
        fatalError("Unable to load persistent stores: \(error)")
    }
})
```
코어 데이터와의 상호작용 대부분은 `NSPersistentContainer` 안에 있는 `NSManagedObjectContext`를 통하며, 유저 인터페이스에 컨테이너에 대한 참조를 전달하는 것을 추천합니다.

왜냐하면 `NSPersistentContainer`는 하위 클래스로 만들어지기 때문에 하위 집합을 반환하는 함수와 영구 데이터에 대한 호출을 디스크에 반환하는 함수와 같은 코어 데이터 관련 함수를 넣기에 좋은 곳이기 때문입니다.

## 코어 데이터 스택 만들기
만약 [`NSPersistentContainer`](https://developer.apple.com/documentation/coredata/nspersistentcontainer) 객체를 만들지 않기로 했다면, [`NSManagedObjectModel`](https://developer.apple.com/documentation/coredata/nsmanagedobjectmodel), [`NSPersistentStoreCoordinator`](https://developer.apple.com/documentation/coredata/nspersistentstorecoordinator)의 인스턴스, 아니면 적어도 [`NSManagedObjectContext`](https://developer.apple.com/documentation/coredata/nsmanagedobjectcontext)의 인스턴스 하나만이라도 초기화해야합니다.

[`NSManagedObjectModel`](https://developer.apple.com/documentation/coredata/nsmanagedobjectmodel)는 Xcode의 모델 에디터에서 생성되고 유지보수되는 '관리 객체 모델'의 프로그래밍적인 표현입니다. 객체 모델의 인스턴스화를 위해서는 Xcode에서 생성한 '관리 객체 모델'의 위치를 가리키고 있는 URL을 넘겨줘야 합니다. 모델 파일은 일반적으로 앱 번들의 일부분입니다.
```
guard let modelURL = Bundle.main.url(forResource: "DataModel", withExtension: "momd") else {
    fatalError("failed to find data model")
}
guard let mom = NSManagedObjectModel(contentsOf: url) else {
    fatalError("Failed to create model from file: \(url)")
}
```
'관리 객체 모델'을 만들고 나면, [`NSPersistentStoreCoordinator`](https://developer.apple.com/documentation/coredata/nspersistentstorecoordinator)와 모델을 연결할 수 있습니다.
```
let psc = NSPersistentStoreCoordinator(managedObjectModel: mom)
```

코어 데이터가 데이터 모델을 디스크에 저장하게 하고 싶다면, 다음 예제에 보이는 것 처럼 어떤 파일에 어떤 포맷으로 저장하고 싶은지를 [`NSPersistentStoreCoordinator`](https://developer.apple.com/documentation/coredata/nspersistentstorecoordinator)에게 알려줘야 합니다. 각각 사용가능한 저장 타입에는 이점과 단점이 있습니다. 이들 저장 타입에 대해 더 상세히 알고 싶다면 [`NSPersistentStoreCoordinator`](https://developer.apple.com/documentation/coredata/nspersistentstorecoordinator) 문서를 확인해보세요.
```
let dirURL = FileManager.default.urls(for: .documentDirectory, in: .userDomainMask).last
let fileURL = URL(string: "DataModel.sql", relativeTo: dirURL)
do {
    try psc.addPersistentStore(ofType: NSSqliteStoreType, configurationName: nil, at: fileURL, options: nil)
} catch {
    fatalError("Error configuring persistent store: \(error)")
}
```
persistent store coordinator가 한번 인스턴스화되고 나면 NSManagedObjectContext의 초기화로 코어 데이터 스택을 만드는 마지막 단계를 밟을수 있게 됩니다.
```
let moc = NSManagedObjectContext(concurrencyType: .mainQueueConcurrencyType)
moc.persistentStoreCoordinator = psc
```
코어 데이터의 상호작용 대부분은 [`NSManagedObjectContext`](https://developer.apple.com/documentation/coredata/nsmanagedobjectcontext) 객체와 함께 수행됩니다. 유저 인터페이스를 참조해 '관리되는 객체 콘텍스트'를 넘겨주세요.

## 참고자료
class [`NSManagedObject`](https://developer.apple.com/documentation/coredata/nsmanagedobject)

-> 코어 데이터 모델 객체에 요구되는 모든 기본 행위를 구현하는 generic 클래스