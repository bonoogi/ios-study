# Constol Flow

## 반복문
### For-in Loops

```
let minutes = 60
let minuteInterval = 5

// stride to
for tickMark in stride(from: 0, to: minutes, by: minuteInterval) {
    // render the tick mark every 5 minutes(0, 5, 10, 15 ... 55)
}

// stride through
for tickMark in stride(from: 0, through: minutes, by: minuteInterval) {
    // render the tick mark every 5 minutes(0, 5, 10, 15 ... 60)
}
```

### While Loops

while과 repeat-while문이 있다.

* while: 매 루프를 시작할 때 조건을 확인
* repeat-while: 매 루프가 끝날 때 조건을 확인

## 조건문

### API Availability Check

```
if #available(iOS 10, macOS 10.12, *) {
    // Use iOS 10 APIs on iOS, and use macOS 10.12 APIs on macOS
} else {
    // Fall back to earlier iOS and macOS APIs
}
```

if #available(platform-name version, ..., *)