## .m、.h、.mm 檔差別
<br />


- `.h` `宣告`給別人看（告訴別人你的類別可以做什麼）
- `.m` `實作`給自己寫（把功能實際做出來）
- `.mm` .mm 是 Objective-C++ 檔案，當你想要在 Objective-C 裡使用 C++ 的語法時，可以把檔案副檔名改成 .mm。


### 實作


`.h` 檔案（Header File，標頭檔）
- 用途：用來宣告類別的介面（interface）。
- 內容：
   * 類別名稱與繼承關係
   * 屬性（Properties）
   * 方法的宣告（Method declarations）
   * 常數、enum、typedef 等其他公開資訊
```objc
// MyClass.h
#import <Foundation/Foundation.h>

@interface MyClass : NSObject

@property (nonatomic, strong) NSString *name;

- (void)sayHello;

@end
```

</br>

`.m` 檔案（Implementation File，實作檔）
- 用途：用來實作 `.h` 檔中所宣告的方法。
- 內容：
   * 方法的實作（Method implementations）
   * 私有變數或方法（可以透過類別擴展 Category 實作）
```objc
// MyClass.m
#import "MyClass.h"

@implementation MyClass

- (void)sayHello {
    NSLog(@"Hello, my name is %@", self.name);
}
@end
```

### 私有變數

1. 類別擴充 (Class Extension)（最推薦）
```objc
// MyClass.h
@interface MyClass : NSObject
// 公有屬性
// @property (nonatomic, strong) NSString *secret;
- (void)logSecret;
@end
```
```objc
// MyClass.m
#import "MyClass.h"

@interface MyClass ()
// 私有屬性（只有 .m 裡能看見）
// @property (nonatomic, strong) NSString *secret;
@end


@implementation MyClass
- (instancetype)init {
    self = [super init];
    if (self) {
        self.secret = @"這是私有的秘密！";
	// secret 不能與 setter 方法名稱相同，下述寫法也可以。
	// _secret = @"這是私有的秘密！"; 
    }
    return self;
}
- (void)logSecret {
    NSLog(@"%@", self.secret);
    // secret 不能與 setter 方法名稱相同，下述寫法也可以。
    // NSLog(@"%@", _secret);
}
@end
```

2. instance variable（成員變數）
你也可以在 @implementation 區塊用大括號 {} 宣告變數，這是比較「傳統 C 語法」的方式。
(傳統私有變數，不用 @property)
```objc
// MyClass.m
@implementation MyClass {
    NSString *title; // 私有變數
}
- (void)setTitle:(NSString *) newTitle {
    title = [newTitle uppercaseString]; // 自訂邏輯
    // 這邊無法使用 _title。
}
@end

```


### setter/getter

1. 公開參數
```objc
// MyClass.h
@interface MyClass : NSObject
@property (nonatomic, strong) NSString *title;
@end
```
```objc
// MyClass.m
#import "MyClass.h"
@implementation MyClass
// 這裡什麼都不用寫，系統自動生成 getter/setter
@end
```
```objc
// 使用方式
MyClass *obj = [[MyClass alloc] init];
obj.title = @"Hello";          // 呼叫 setter：setTitle:
NSLog(@"%@", obj.title);       // 呼叫 getter：title
@end
```

2. 私有參數 + 自訂setter/getter

```objc
// MyClass.h
@interface MyClass : NSObject
@property (nonatomic, strong) NSString *title;
@end
```
```objc
// MyClass.m
#import "MyClass.h"

@interface MyClass()
// 私有屬性（只有 .m 裡能看見）
@property (nonatomic, strong) NSString *tempTitle;
@end

// set (不寫走預設)
@implementation MyClass
- (void)setTitle:(NSString *) newTitle {
    _tempTitle = [newTitle uppercaseString]; // 自訂邏輯
}
// get (不寫走預設)
- (NSString *) title {
    return [NSString stringWithFormat:@"《%@》", _tempTitle];
}
@end

```


### 類別分類（Category）範例（通常用來加功能）

- 特點：
   * 用來擴充原本 class 的`方法`
   * 不能新增`屬性`
   * 可以分成很多 category 檔案，幫助整理程式碼模組

```objc
// User.h
#import <Foundation/Foundation.h>

@interface User : NSObject

- (void)printInfo;

@end
```

```objc
// User+Helper.h
#import "User.h"

@interface User (Helper)

- (NSString *)userType;

@end
```

```objc
// User+Helper.m
#import "User+Helper.h"

@implementation User (Helper)

- (NSString *)userType {
    return @"普通用戶";
}

@end
```


```objc
// Main.m
User *user = [[User alloc] init];
NSString *type = [user userType];
```


### 相關教學
1. [.h 檔宣告](document/1/README.md)







