#all Function Squence After Load Nib

## 問題
我貼上 nib 之後，設定 delegate，在 nib 裡面要用 delegate 做事情，結果 `crash`
code 如下：

外面的 view
```swift
nib = loadFromNibNamed("NibScene") as! NibScene
nib.frame = view.bounds
view.addSubview(nib)
nib.delegate = self

```
nib
```swift
override func awakeFromNib() {
    initUI()
}

func initUI() {
  delegate?.doSomething()
}
```

### error message
```
EXC_BAD_INSTRUCTION (code=EXC_I386_INVOP, subcode=0x0)
```
網上解答為：
>Generally, `EXC_BAD_INSTRUCTION` means that there was an assertion failure in your code.

通常就是 load 到 nil 的物件。

查了一下發現上述的 code 中：
```swift
nib = loadFromNibNamed("NibScene") as! NibScene
```
這行已經產生 nib，所以會直接執行 `awakeFromNib()`這個 function。
此時如果便將要 delegate 實作 protocol 寫在這裡，對 nib 而言，根本尚未設定 delegate，出現錯誤也是合理。

## 解法

尚未找到比較聰明的做法，只能 `亡羊補牢地` 事後再執行要做的事情。
延續前例，我要執行的事情原本是寫在 nib 裡面的 `initUI()`
就拿到外面來寫：

```swift
nib = loadFromNibNamed("NibScene") as! NibScene
nib.frame = view.bounds
view.addSubview(nib)
nib.delegate = self
nib.initUI()
```

這樣順序就對了。

以上

