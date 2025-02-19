## SwiftUI 學習筆記
<br />


### ObservableObject 用法

`ObservableObject` 用於 class，並且把它當作一個觀察對象，並在想要監聽的參數補上 `@Published`，當有監聽參數有被設定時 (值非變化)，便會通知外部更新。

```swift
final class ViewModel: ObservableObject {
    @Published var value = 1

    func randomValue() {
        value = Int.random(in: 1...10)
    }
}

struct TestView: View {
    @ObservedObject var viewModel = ViewModel()

    var body: some View {
        VStack {
            Text("Value is: \(viewModel.value)")
            Button("Click") {
                viewModel.randomValue()
            }
        }
    }
}
```

不用 `@Published` 寫法，可直接使用 `objectWillChange.send()`，也可以達到相同效果。

```swift
final class ViewModel: ObservableObject {
    private(set) var value = 1

    func randomValue() {
        value = Int.random(in: 2...10)
        objectWillChange.send() // 主要這句
    }
}
```
<br />

### StateObject 用法

直接看下面範例：

```swift
struct TestView: View {
    @StateObject var viewModel = ViewModel() // ObservedObject -> StateObject

    var body: some View {
        VStack {
            Text("Value is: \(viewModel.value)")
            Button("Click") {
                viewModel.randomValue()
            }
        }
    }
}

//最外層 View
struct ContentView: View {

    @State private var backgroundColor: Color = .blue

    var body: some View {
  
        VStack {
            // 測試介面
            TestView()
            // 點擊按鈕，單純改變 ContentView.Button 背景顏色。
            Button("Change Color") {
                backgroundColor = Color(
                    red: Double.random(in: 0...1),
                    green: Double.random(in: 0...1), 
                    blue: Double.random(in: 0...1)
                )
            }
            .foregroundColor(.black)
            .background(backgroundColor)
            .frame(width: 100, height: 50)
        }
        .padding()
    }
}
```

ObservedObject
<br />
`TestView.randomValue() -> 參數隨機 (假如是 2) -> ContentView.Button("Change Color").click -> 參數會被重置為 1`

<br />

StateObject／State
<br />
`TestView.randomValue() -> 參數隨機 (假如是 2) -> ContentView.Button("Change Color").click -> 參數不變`

<br />




