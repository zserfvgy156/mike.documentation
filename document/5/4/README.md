## Class  `new`/`alloc`/`allocWithZone` 差別
<br />

在 Objective-C 中，`[NSObject new]`、`[[NSObject alloc] init]`、和 `[[NSObject allocWithZone:...] init]` 這三種語法最終目的都是建立一個物件實例，但在底層上稍有不同。以下是它們的差異解析：

<br />

### 範例說明


```objc
// MyObject 建構子無參數
@interface MyObject : NSObject
@end

@implementation MyObject
- (instancetype)init {
    self = [super init];
    if (self) {
        NSLog(@"MyObject initialized");
    }
    return self;
}
@end
```

```objc
// Person 建構子有自訂參數
@interface Person : NSObject

@property (nonatomic, strong) NSString *name;
@property (nonatomic, assign) NSInteger age;

// 自訂初始化方法
- (instancetype)initWithName:(NSString *)name age:(NSInteger)age;
@end


@implementation Person
- (instancetype)initWithName:(NSString *)name age:(NSInteger)age {
    self = [super init];
    if (self) {
        _name = name;
        _age = age;
        NSLog(@"Person initialized with name: %@, age: %ld", name, (long)age);
    }
    return self;
}
@end
```



#### `[[Class alloc] init]`（最常用、最標準）

```objc
// alloc：向類別分配一塊記憶體
// init：初始化該物件的內部狀態
//
// 這是大多數情況下你應該使用的寫法，清楚地分成「配置」與「初始化」兩步。
MyObject *obj = [[MyObject alloc] init];
Person *p = [[Person alloc] initWithName:@"Alice" age:30];
```

#### `[Class new]`（語法糖，但不推薦用）

```objc
// new 是 alloc + init 的簡寫
MyObject *obj = [MyObject new]; // 等同於 [[MyObject alloc] init]
[Person new]; // ❌ 不會觸發自訂初始化器
// 不推薦:
// - 沒有靈活性：如果你要用 initWith... 之類的初始化器，就無法用 new。
// - 閱讀性略差：在大專案中常見 alloc/init 寫法，new 可能讓人混淆。
```

#### `[[Class allocWithZone:...] init]`（很少用）

```objc
// allocWithZone: 是舊式的記憶體分配方式
// 
// 允許指定一個「zone」（記憶體區塊）給物件分配
MyObject *obj = [[MyObject allocWithZone:nil] init];
Person *p = [[Person allocWithZone:nil] initWithName:@"Alice" age:30];
// 舊式語法，現代開發很少需要它，幾乎不用。
```

