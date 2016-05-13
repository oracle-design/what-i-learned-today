## 前言
有時候需要顯示 `Alert Controller`，來提示用戶。而 `Alert Controller` 作為一個 view controller，只能顯示在view controller上面.

## 使用 nib 開發的情境
這時候如果開發是以 `nib` 為主，就還要找到該 `nib` 底下的view controller，然後再寫方法，然後再貼上去，wtf...

自己目前習慣的做法是直接將方法寫在 `singletons` 裡面.code如下：

```swift
func showAlertViewWith(msg: String) {
  let alert:UIAlertController = UIAlertController(title: "提示", message: msg, preferredStyle: .Alert)
  alert.addAction(UIAlertAction(title: "OK", style: .Default, handler: { action in
      alert.dismissViewControllerAnimated(false, completion: nil)
    }))
  UIApplication.sharedApplication().delegate!.window!!.rootViewController?.presentViewController(alert, animated: true, completion: nil)
}
```

直接找到當前畫面的 `rootViewController` 然後貼上去，因為貼的是 `viewController`，會凌駕於所有的 `nib` 上面。不用緊張。

## 使用 storyboard 開發的情境
然後另外一種情況是：直接在storyboard上面作業，而且還有 `embed in Navigation controller`，這樣如果再用上面的code，會看不到，因為此時的 `rootViewController` 是 `Navigation Controller`。解法code如下：

```swift
func showAlertViewWith(msg: String) {
  let alert:UIAlertController = UIAlertController(title: "提示", message: msg, preferredStyle: .Alert)
  alert.addAction(UIAlertAction(title: "OK", style: .Default, handler: { action in
      alert.dismissViewControllerAnimated(false, completion: nil)
    }))
  UIApplication.sharedApplication().delegate!.window!!.rootViewController?.presentedViewController!.presentViewController(alert, animated: true, completion: nil)

  }
```

照理來說，如果有`Navigation Controller`，其實當前的 viewController，也不過就是在上去一層而已，在後面加上 `presetedViewController` 即為當前頁面，即可顯示。寫在 `singletons` 裡面，也是到處都可以 call。方便。


以上。


