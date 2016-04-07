#Ternary Operator (三元運算子)

##用途
將 code 再短一點

##用法
### before：
```swift
let a: Bool = true
var result = ""

if a {
  result = "AAA"
} else {
  result = "BBB"
}
```

### after

```swift
let result = a == true ? "AAA" : "BBB"
```

### 公式

[事情成立的條件] ? [成立的結果] : [未成立的結果]

### 好處
1. code 變短了
2. 邏輯變簡單，避開危險的if-else
3. 對swift而言，原先宣告的`變數`，可以變成`常數`處理，相對安全。

### 壞處
可讀性降低，寫法承襲了C語言。
畢竟swift的初衷便是要擺脫C語言的包袱...（連++都可以拿掉，我真的是...）

（過去初學objc時就知道的技巧，但一直沒用，因為這個原因，但這多少也牽涉主觀意識，現在倒是漸漸適應，因為它可以將代碼寫的更直覺，前提是要先習慣他）

以上
