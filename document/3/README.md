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
- Cursor Tab

有時會用灰字／tab 提示，可直接點擊 tab 自動完成輸入。
<img src="https://github.com/zserfvgy156/mike.documentation/blob/main/document/4/images/3.png" width="500" height="280">
<img src="https://github.com/zserfvgy156/mike.documentation/blob/main/document/4/images/4.png" width="500" height="280">

<br />

- Inline edit
- Chat

- The Composer

