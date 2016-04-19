
add Nib by Swift

## 過去使用的方法：

```swift
let nibViewSetPrice: UIView = NSBundle.mainBundle().loadNibNamed("NibViewSetMoneyOnPriceTag", owner: self, options: nil)[0] as! UIView
```
## 剛剛學到的方法

`善用 extension `

```swift
extension UIView {
  func loadFromNibNamed(nibNamed: String, bundle : NSBundle? = nil) -> UIView? {
    return UINib(
      nibName: nibNamed,
      bundle: bundle
      ).instantiateWithOwner(nil, options: nil)[0] as? UIView
  }
}
```

這樣可以直接將上述的方法“擴充”到 UIView

今後直接
```swift
let nibViewSetPrice = loadFromNibNamed("NibViewSetMoneyOnPriceTag")
```

但如果需要對該 nib 執行 它自己的 function，必須轉型別為該 class

```swift
let nibViewSetPrice = loadFromNibNamed("NibViewSetMoneyOnPriceTag") as! NibViewSetMoneyOnPriceTag
```

否則它只會視為UIView。


## ref
http://stackoverflow.com/questions/24370061/assign-xib-to-the-uiview-in-swift


