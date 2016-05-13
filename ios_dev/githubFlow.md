
## 名詞解釋
* local repo：專案在自己本地端開發
* origin repo：專案fork到自己github帳號
* upstream repo：專案在公司github帳號
* 頭兒：專案負責管理 PR 的工程師
* 夥伴：專案共同開發者

## 流程
1. 在 local create branch
2. 起個頭，發 commit， push 到 origin
3. origin 發 PR 到 upstream，PR title 寫 `[WIP] #977` （977為 task number ）
4. 之後的每次 commit，都接著 push 到 origin ， PR 會自動疊上去（這件事是方便讓夥伴了解自己的進度，一方面讓頭兒監督，一方面讓夥伴了解自己的開發會不會影響他自己的開發）基本上，看到 `[WIP]` 便不會真的將該 request 真的 merge 進來（因為尚未完成）
5. 如果與其他開發者共事，於開發過程中，需要時時做 pull 的動作，更新自己的 master。如果有更新，就要將目前自己的進度 rebase 上去，再繼續開發。
6. 等到 feature 完成之後，將 PR title 的 `[WIP]` 去掉，並通知頭兒做 code review 。如果有需要做修改的，再繼續修改，commit，push 到 origin，直到頭兒 code review 確認 feature 無誤，便 merge。
7. merge完之後，將 upstream repo pull下來。
8. push 到 origin。


