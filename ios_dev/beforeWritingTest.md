# 寫 Swift 測試的前置作業
[photo1]: http://bit.ly/1sKjriI 
[photo2]: http://bit.ly/1PiaXJX
[photo3]: http://bit.ly/1PiaZle 
[photo4]: http://bit.ly/1Pi9bIJ 
[photo5]: http://bit.ly/1Pi9pzD 
[photo6]: http://bit.ly/1PibviY
[photo7]: http://bit.ly/1Pi9Ytl


## 前言

![][photo1]

如上圖，一般新建一個專案，如果預先確定會寫測試，會在新建的過程中，勾選圖中紅色框標示的位置，系統自會生出 `[專案名]Tests` 的資料夾。

![][photo2]

例如我這裡的專案叫做 `KimuraTest`，那我按照上述的步驟，就會有叫做 `KimuraTestTests` 的測試檔可以用。

## 問題

但如果正在撰寫的專案，當初新建專案時，沒有勾選 `Include Unit Tests` ，現在又想開始寫測試的話，怎麼辦？

## 解法

以下的範例，先建一個不帶測試的專案。底下沒有勾選。
![][photo3]

注意下圖中，沒有任何測試檔可以用。此時就算新建了 `Unit Test Case Class` 檔案，還是會報錯，因為尚未連結寫測試需要的 `XCUnit framework`。

![][photo4]

所以需要做連結的動作。

進到 `Target Setting`，點擊上圖底下紅色方框的按鈕。

![][photo5]

如上圖，左邊選 `iOS` 裡的 `Test`，右邊選 `iOS Unit Testing Bundle`。

![][photo6]
如上圖，輸入該 Test 的名稱。
![][photo7]

此時便出現需要的測試檔，就好像新建專案有點選 `Include Unit Tests` 的樣子。


**以上。**


## 參考
[這篇的最佳解答](http://stackoverflow.com/questions/29965397/cannot-load-underlying-module-for-xctest)

