##主題
怎麼更方便地將欲顯示的畫面貼到最頂層，無論底下是什麼畫面

##情境
我有一個顯示 Terms of Service 的 WebView，不僅登入頁要能顯示，也要在設定頁能顯示。
基本做法是：直接叫出 webview，貼到登入頁上；另外一邊再貼到設定頁上。
水！那現在要更改 URL 該怎麼辦？好，URL設定成全域變數，僅更改一處即可。
那現在要更改出現該頁面的動畫？要更改出現該頁面位置？
更改兩處？有幾個頁面就該幾個地方？

##前言
個人做 ios 開發，習慣將 view 切成 一張張 nib 好管理且好複用，但這樣的做法容易讓 view 離 viewcontroller太遠（如果疊了好幾層），往往遇到`只能在 view controller 才能顯示的畫面`（例如：`UIAlertView`），有時候就要為此寫 Delegate；寫 Delegate 還不打緊，如果是很多畫面都會出現，每個畫面都要為此寫就很笨。

所有的畫面都是建立在 UIWindow 上，所以 UIWindow 只會有一個，只要取得 UIWindow，就能直接將我想顯示的畫面，直接貼上即可，而不用理會底下是什麼view。

##做法

```swift
UIApplication.sharedApplication().delegate!.window?!.rootViewController?.view
```
一層層取到上述 code 為止，即該 view。

假設我要貼上的 view 叫 `nibView`：

```swift
UIApplication.sharedApplication().delegate!.window?!.rootViewController?.view.addSubview(nibView)
```

那接下來我就直接將這個 code 包成 function，實作在 singletons，我在任何地方都可以叫出該頁面，無論什麼頁面之下都能貼上。

Happy now

