## Immutable / Mutable、copy 踩坑
<br />
<br />

## `Mutable` vs `Immutable`

請參考下圖：

| 不可變 (Immutable) | 可變 (Mutable) | 說明 |
| :---: | :------: | :----------------- |
| NSString | NSMutableString | 字串 |
| NSArray | NSMutableArray | 陣列 |
| NSDictionary | NSMutableDictionary | 鍵值對 |
| NSSet | NSMutableSet | 集合 |


#### Immutable (不可變)
建立之後 `不能改內容`，只能讀取。
```objc
NSString *s = @"Hello";
// [s appendString:@" world"]; ❌ 不能改

NSDictionary *dic = @{@"a": @"1"};
// dic[@"b"] = @"2"; ❌ 編譯錯誤，不能改
```
#### Mutable (可變)
可以在原物件上 `修改內容`。
```objc
NSMutableString *ms = [NSMutableString stringWithString:@"Hello"];
[ms appendString:@" world"]; // ✅ 變成 "Hello world"

NSMutableDictionary *mDic = [@{@"a": @"1"} mutableCopy];
mDic[@"b"] = @"2"; // ✅ OK
```

## `copy` vs `mutableCopy`

#### copy
- 回傳一個 `不可變 (immutable)` 版本。
- 就算原本是可變物件 (`NSMutableArray`、`NSMutableString`)，也會變成不可變版本。
- 常用於「保護內部狀態」。

```objc
NSMutableString *mStr = [NSMutableString stringWithString:@"Hello"];
NSString *strCopy = [mStr copy];     
// strCopy 是 NSString，不可變
```

#### mutableCopy
- 回傳一個 `可變 (mutable)` 副本。
- 不管原本是 immutable 還是 mutable，都會得到一個新的 mutable 物件。

```objc
NSString *str = @"Hello";
NSMutableString *mStrCopy = [str mutableCopy]; 
// mStrCopy 是 NSMutableString，可以修改
```

#### 差異對照表
| 物件類型 | copy | mutableCopy |
| :---: | :------: | :----------------- |
| NSString | NSString | NSMutableString |
| NSMutableString | NSString | NSMutableString |
| NSArray | NSArray | NSMutableArray |
| NSMutableArray | NSArray | NSMutableArray |
| NSDictionary | NSDictionary | NSMutableDictionary |
| NSMutableDictionary | NSDictionary | NSMutableDictionary |


## 防禦性拷貝

當你拿到一個 `外部傳進來的物件`，但你不確定它是不是 mutable，或不希望它被外部修改而影響你的邏輯，這時候就要用 copy。

### 範例一
#### 不 copy
```objc
// DataStore.h
@interface DataStore : NSObject
@property (nonatomic, strong) NSMutableArray *items;
- (NSArray *)getItems;   
@end
```
```objc
// DataStore.m
@implementation DataStore
- (NSArray *)getItems {
    return _items; // 直接回傳，不 copy
}
@end
```
```objc
// Main.m
// 使用
DataStore *store = [DataStore new];
store.items = [NSMutableArray arrayWithArray:@[@"A", @"B"]];

NSArray *data = [store getItems];  
NSLog(@"初始: %@", data); // (A, B)

// 外部修改內部陣列
[store.items addObject:@"C"];
NSLog(@"修改後: %@", data); // (A, B, C) <- 被影響的部分
```

#### 加上 copy

```objc
// DataStore.h
@interface DataStore : NSObject
@property (nonatomic, strong) NSMutableArray *items;
- (NSArray *)getItems;   
@end
```
```objc
// DataStore.m
@implementation DataStore
- (NSArray *)getItems {
    return [_items copy]; // 回傳副本 <- 改這行
}
@end
```
```objc
// Main.m
// 使用
DataStore *store = [DataStore new];
store.items = [NSMutableArray arrayWithArray:@[@"A", @"B"]];

NSArray *data = [store getItems];  
NSLog(@"初始: %@", data); // (A, B)

[store.items addObject:@"C"];
NSLog(@"修改後: %@", data); // (A, B) ✅ 不受影響
```

### 範例二
#### 不 copy
```objc
// User.h
@interface User : NSObject
@property (nonatomic, strong) NSString *name; // 注意這裡用 strong，不是 copy
@end
```
```objc
// User.m
@implementation User
@end
```
```objc
// Main.m
// 使用
NSMutableString *mStr = [NSMutableString stringWithString:@"Mike"];
User *u = [User new];
u.name = mStr;

NSLog(@"初始: %@", u.name); // Mike

// 外部修改 mStr
[mStr appendString:@" Lin"];
NSLog(@"修改後: %@", u.name); // Mike Lin <- 被影響的部分
```

#### 加上 copy

```objc
// User.h
@interface User : NSObject
@property (nonatomic, copy) NSString *name; // 改成 copy  <- 改這行
@end
```
```objc
// User.m
@implementation User
@end
```
```objc
// Main.m
// 使用
NSMutableString *mStr = [NSMutableString stringWithString:@"Mike"];
User *u = [User new];
u.name = mStr;

NSLog(@"初始: %@", u.name); // Mike

[mStr appendString:@" Lin"];
NSLog(@"修改後: %@", u.name); // Mike ✅ 不受影響
```

## 方法回傳值是否要用 copy

#### 回傳內部屬性/資料 (要 copy)
- 如果你直接把內部的 `NSMutableArray`、`NSMutableDictionary`、`NSMutableString` 回傳給外部，外部有可能透過那個 reference 改掉你的內部資料。
- 解法：回傳 `copy` (通常是 immutable 版本)。
```objc
- (NSArray *)allItems {
    return [_items copy];  // ✅ 外部無法改動內部
}
```

#### 回傳新建的物件 (不用 copy)
- 如果方法裡面本來就 `alloc/init` 一個新物件，或組了一份新的不可變物件，直接回傳即可，不需要 `copy`。
```objc
- (NSArray *)makeSampleItems {
    return @[@"A", @"B", @"C"]; // ✅ 不用 copy，本來就是新建立的
}
```

#### 回傳 Immutable 常量 (不用 copy)
- 如果回傳的是 `@"字串常量"` 或 `@{}`、`@[]` 這種本來就 immutable 的東西，不需要 `copy`。
```objc
- (NSString *)defaultTitle {
    return @"Hello"; // ✅ 字串常量不可變，不用 copy
}
```

#### 回傳需要保證外部不可變的資料 (要 copy)
- 如果內部存的是 `NSMutableArray`，但你要保證 API 回傳的永遠是 `NSArray`，就要 `copy`。
```objc
- (NSArray *)userList {
    return [_mutableUserList copy]; // ✅ 保證回傳 NSArray
}
```

#### 性能考量 (大量資料時酌量用)
- `copy` 會產生新物件，大量資料時有成本。
- 如果方法是高頻呼叫（例如 UI 刷新），要小心效能。
- 替代做法可改回傳 `readonly` 的 immutable 屬性。

### copy 重點
- 要保護內部狀態 → copy
- 自己新建的、不會被外部影響 → 不用 copy

<br />
<br />




