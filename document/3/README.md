# VSCode/Cursor iOS 專案製作
<br />

## 安裝流程
1. [VSCode iOS 專案開發](https://dimillian.medium.com/how-to-use-cursor-for-ios-development-54b912c23941)
2. [SweetPad 編譯流程](https://sweetpad.hyzyla.dev/docs/debug)
3. [Inject 熱更新](https://rebeloper.com/blog/how-to-develop-ios-and-macos-apps-in-vs-code)

> [!WARNING]
> 由於 Inject 無法選取元件本身，所以推薦直接開 XCode 處理，可參考[彼得潘推薦做法](https://medium.com/%E5%BD%BC%E5%BE%97%E6%BD%98%E7%9A%84-swift-ios-app-%E9%96%8B%E7%99%BC%E5%95%8F%E9%A1%8C%E8%A7%A3%E7%AD%94%E9%9B%86/%E5%90%8C%E6%99%82%E4%BD%BF%E7%94%A8-vs-code-github-copilot-%E5%92%8C-xcode-preview-%E9%96%8B%E7%99%BC-app-55cff896d752)。

<br />

## sourcekit-lsp 多版本設定處理

<br />

> 可以從 Xcode -> settings -> Locations -> Command Line Tools 查看目前專案的路徑。
<br />
<img src="https://github.com/zserfvgy156/mike.documentation/blob/main/document/3/images/1.png" width="600" height="280">

<br />
<br />

> VSCode 設定 -> 搜尋 lsp -> Swift:Path
<br />
把剛剛的路徑 (/Applications/Xcode_16.1.app)，尾部再補 "/Contents/Developer/Toolchains/XcodeDefault.xctoolchain/usr/bin"。
<br />
完整路徑 (/Applications/Xcode_16.1.app/Contents/Developer/Toolchains/XcodeDefault.xctoolchain/usr/bin)

<br />
<br />

<img src="https://github.com/zserfvgy156/mike.documentation/blob/main/document/3/images/2.png" width="500" height="280">


<br />

## Cursor AI 功能
### Cursor Tab

會有`灰字／tab` 提示，點擊按鍵 tab 來完成自動輸入。

<br />
<img src="https://github.com/zserfvgy156/mike.documentation/blob/main/document/3/images/3.png" width="450" height="120">
<img src="https://github.com/zserfvgy156/mike.documentation/blob/main/document/3/images/4.png" width="500" height="280">

<br />

### Inline edit

分為`可選/不可選文字代碼`行間編輯功能，依照使用者給的建議，生成相對應的代碼，期間可以再補充細節建議。
<br />
<br />

1. 未選取代碼

<br />

浮標只要點擊一處，就會有 chat 與 Inline edit 快捷建議。

<br />

<img src="https://github.com/zserfvgy156/mike.documentation/blob/main/document/3/images/5.png" width="450" height="120">

<br />

呼叫後，可以直接輸入想要的規格／敘述，便會生成下方代碼，然後右下角可以補充詳細細節。
<br />
<img src="https://github.com/zserfvgy156/mike.documentation/blob/main/document/3/images/6.png" width="400" height="480">

<br />
<img src="https://github.com/zserfvgy156/mike.documentation/blob/main/document/3/images/7.png" width="400" height="90">

<br />
<img src="https://github.com/zserfvgy156/mike.documentation/blob/main/document/3/images/8.png" width="300" height="300">

<br />

2. 選取代碼

<br />

針對指定選取區域做改寫，其他流程跟未選取代碼差不多。

<br />

<img src="https://github.com/zserfvgy156/mike.documentation/blob/main/document/3/images/9.png" width="390" height="320">

<br />

<img src="https://github.com/zserfvgy156/mike.documentation/blob/main/document/3/images/10.png" width="370" height="320">

<br />

<img src="https://github.com/zserfvgy156/mike.documentation/blob/main/document/3/images/11.png" width="370" height="320">


### Chat

如 Inline edit 呼叫方法一樣，呼叫後 VSCode 右邊會有面板會顯示詢問結果，也可以直接在右側面板提問。
<br />
可以直接在右側面板選取多個檔案，或者是像 Inline edit 選取/不選取時提問 (此時以提問的檔案為主)。

<br />
<img src="https://github.com/zserfvgy156/mike.documentation/blob/main/document/3/images/12.png" width="400" height="680">

### The Composer
不常使用，待補。

<br />
<br />

