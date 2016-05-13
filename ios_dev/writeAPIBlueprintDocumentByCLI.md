##1。安裝gem
```
gem install apiaryio
```

##2。在`Apiary`新增專案

##3。設定token
先到[這裡](https://login.apiary.io/tokens)取得token，複製貼到下面指令的`<token>`
```
export APIARY_API_KEY="<token>"
```

##4。下載檔案
輸入下行指令下載檔案
```
apiary fetch --api-name="<API_NAME>" --output="apiary.apib"
```
1. `output`後面雙引號內的字不要改，否則之後上傳會很麻煩。
2. `<API_NAME>`裡面帶入專案名稱

3. 專案名稱看下圖劃線處
![Quickper_app Editor 2016-05-12 17-35-36.png](http://i.imgur.com/Kof8CP2.png)

便會將檔案下載到當前的位置

##5。寫 API 文件
用 vim 或 sublime 開啟它即可編輯寫 markdown

##6。上傳
寫完之後輸入以下指令直接上傳至網站
```
apiary publish --api-name="<API_NAME>"
```
重新整理即可看到，done



