## @synthesize 用法
<br />

在 Objective-C 中，`@synthesize` 是用來` 自動產生 property 對應的實例變數（instance variable）`  和 ` getter/setter 方法` 的指令。



### 基本說明


```objc
@property (nonatomic, strong) NSString *name;

// 上面那一行宣告了一個 property，編譯器會自動為你產生：
- (NSString *)name;
- (void)setName:(NSString *)name;
```

```objc
// 這時候如果你寫：
@synthesize name;
// 表示讓編譯器幫你對應這個 property 到一個實例變數 _name（或你自訂的名稱）並產生 getter/setter。

// 自訂實例變數名稱
@synthesize name = _customName;

```

### 實例

```objc
// Person.h
@interface Person : NSObject
@property (nonatomic, strong) NSString *name;
@end
```

```objc
// Person.m
@implementation Person
@synthesize name = _personName;


- (instancetype)init {
    self = [super init];
    if (self) {
        _personName = @"Default"; // OK，因為此時不要觸發 setter
    }
    return self;
}

// getter
- (NSString *)name {
    return _personName;
}

// setter
- (void)setName:(NSString *)name {
    _personName = @"NewName"; 
    
    // 這裡不能用 self.name，否則會無限遞迴呼叫自己。
    // self.name = @"NewName"; 
}
@end
```


### 注意

從 Xcode 4.4 以後（LLVM 4.0）開始：
- 若你沒有寫 `@synthesize`，編譯器會自動幫你合成（變數名為 `_propertyName`）。
- 所以除非你要 ` 手動控制變數名稱`  或 ` 自定義 getter/setter 行為` ，通常可以省略。


</br>
</br>

