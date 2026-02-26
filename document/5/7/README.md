## Swift 與 Objective-C 對於空值 (nil / Optional / NSNull) 的差異
<br />

| 概念 | Objective-C | Swift |
| :----: | :----: | :----: |
| 空物件 | `nil` | `nil` |
| 是否是型別的一部分 | 不是型別 | 是 `Optional<T>` |
| 呼叫方法 | 傳訊給 `nil` 不會 crash | 對 `nil` 強制解包會 crash |
| 是否可放進 Array | 不行 | 可以（如果是 Optional） |


| Objective-C 型別 | 能不能放 nil |
| :----: | :----: |
| NSArray | ❌ 不行 |
| NSMutableArray | ❌ 不行 |
| NSDictionary | ❌ 不行 |
<br />



### Array 行為差異
#### Objective-C

如果 ObjC 也允許 [nil]，就會遇到跟 Swift 一樣的問題，只是 ObjC 沒有型別系統，沒辦法分層表示。
Apple 的解法：禁止 Collection 放 nil，用 `[NSNull null]` 表示「空值」。


```objc
NSMutableArray *arr = [NSMutableArray array];
[arr addObject:nil];   // ❌ 直接 crash
```

##### 如果需要表示「空值」：
```objc
[arr addObject:[NSNull null]];
```

##### 之後取出判斷：
```objc
id obj = array[index];
if (obj == [NSNull null]) {
    // 表示原本是空
}
```


#### Swift

Swift 把「是否允許 nil」交給型別系統決定。

##### 一般 Array（不能放 nil）
```swift
var arr: [String] = []
arr.append(nil) // ❌ 編譯錯誤
```
##### Optional Array（可以放 nil）
```swift
var arr: [String?] = []
arr.append(nil) // ✅ 合法
```
<br />



### Dictionary 行為差異


#### Objective-C
```objc
NSDictionary *dict = @{ @"key": nil };           // ❌ crash
NSDictionary *dict = @{ @"key": [NSNull null] }; // ✅ OK
```

#### Swift

##### 非 Optional 類型：
```swift
var dict: [String: String] = [:]
dict["key"] = nil  // ✅ 這是移除 key，不是存一個 nil。
```

##### Optional 類型：
```swift
var dict: [String: String?] = [:]
dict["key"] = nil   // ✅ 現在是值為 Optional.none
dict2.removeValue(forKey: "key") // 移除 key 
```
<br />



### 方法呼叫差異
#### Objective-C

傳訊給 nil 安全，回傳 0 / nil / 0.0。
```objc
NSString *str = nil;
NSUInteger len = [str length]; // ✅ 不 crash，回傳 0
```

#### Swift
```swift
let str: String? = nil
let len = str!.count   // ❌ crash
let len = str?.count   // ✅ 回傳 nil (Swift 不允許「靜默忽略 nil」)
```
<br />


### 關鍵差異

- Swift 用型別把語意分層。
- Objective-C 沒有型別層次，只能用同一個 nil。

#### Objective-C
```objc
nil = 沒有物件
```

#### Swift
```swift
Optional.none = 沒有值
Optional.some(nil) = 有值，但裡面是 nil
```
<br />



### 陣列為空 / 第一個元素為 nil / 第一個元素有值


#### 陣列是空的

Objective-C：
```objc
NSArray *arr = @[]; 
id obj = arr.firstObject; // obj = nil
```

Swift：
```swift
var arr: [String?] = [] 
let value = arr.first // value = nil
```

#### 第一個元素是 nil

Objective-C：
```objc
NSArray 不能放 nil
```

Swift：
```swift
var arr: [String?] = [nil, "B"] 
let value = arr.first // value = .some(nil)
```

#### 第一個元素有值

Objective-C：
```objc
NSArray *arr = @[@"A", @"B"]; 
id obj = arr.firstObject; // obj = @"A"
```

Swift：
```swift
var arr: [String?] = ["A", "B"] 
let value = arr.first // value = .some(.some("A"))
```

<br />
透過以上範例，整理 Swift 一些重點：

```swift
let arr1: [String?] = []
arr1.first          // nil → 陣列空

let arr2: [String?] = [nil, "B"]
arr2.first          // .some(nil) → 第一個元素是 nil

let arr3: [String?] = ["A", "B"]
arr3.first          // .some(.some("A")) → 第一個元素是 A


// 要使用有以下三個方式
// 1. 解兩層 Optional。
if let outer = arr3.first {
    if let inner = outer {
        print(inner) // "A"
    }
}

// 2. 一層解包 + ??
if let inner = value ?? nil {
    print(inner) // inner: String?
}

// 3. pattern matching
switch value {
case .none:
    print("陣列是空的")
case .some(nil):
    print("第一個元素是 nil")
case .some(.some(let str)):
    print("第一個元素是 \(str)") // str: "A"
}

```
<br />



### Objective-C nil 返回值

- Swift 強制 Optional 安全：對 nil 強制 unwrap 會 crash
- ObjC 傳訊給 nil 安全 （可能 silently pass → 導致 bug 隱藏）

| 傳給 nil 的方法 | ObjC 返回值 |
| :----: | :----: |
| 物件 | nil |
| NSInteger / CGFloat | 0 |
| BOOL | NO |
| struct | 全 0 |


<br />

1. 物件方法 → 回傳 nil
```objc
NSString *s = [nil getString]; // s = nil
```

2. NSInteger / CGFloat → 回傳 0
```objc
NSInteger i = [nil getInt]; // i = 0
CGFloat f = [nil getFloat]; // f = 0.0
```

3. BOOL → 回傳 NO（0）
```objc
BOOL b = [nil getBool]; // b = NO
```

4. struct / C struct → 全部字段都是 0
```objc
CGRect r = [nil getRect]; // {{0,0},{0,0}}
```
<br />



<br />
<br />

