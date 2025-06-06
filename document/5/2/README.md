# .h 檔宣告

<br>

## 一個基本的 `.h` 檔案範例
```objc
// MyClass.h

#import <Foundation/Foundation.h>

// 宣告 MyClass 類別，繼承自 NSObject
@interface MyClass : NSObject

// 屬性宣告
@property (nonatomic, strong) NSString *name;
@property (nonatomic, assign) NSInteger age;
@property (nonatomic, readonly) NSInteger score;


// 方法宣告
// - (return_type) method_name:( argumentType1 )argumentName1 
// joiningArgument2:( argumentType2 )argumentName2 ... 

// "-"：宣告 instance 方法（物件方法）
// "+"：宣告 class 方法（類別方法）

- (void)printInfo;
- (NSString *) testMethod: (NSString *)name1 and: (NSString *)name2;

@end
```
<br />

## @property 的常見屬性（attributes）

- `atomic`、`nonatomic` 擇一
   * `nonatomic`：非原子性（效能較好，最常用）
   * `atomic`：預設值，執行緒安全但效能較差（很少特別寫）
- 其他屬性
   * `strong`：預設用於物件型別（表示擁有該物件，如：NSString）
   * `weak`：弱引用，通常用於 delegate 與介面，避免 retain cycle
   * `assign`：用於基本資料型別（如：int, float, BOOL, NSInteger, CGFloat, CGSize）
   * `copy`：用於 NSString 或 block，保證是不可變的複製體

<br />

## 方法型態

| 符號 | 方法類型 | 說明 | 誰來呼叫？ |
| :---: | :------: | :----------------- | :----------------- |
| - | 實例方法 (Instance method) | 給「物件實例」使用的 | 透過物件呼叫 |
| + | 類別方法 (Class method) | 給「類別本身」使用的 | 透過類別名稱呼叫 |


#### `-` 用法 (實例方法)
這是最常見的情況，必須先建立物件實例才能呼叫。

```objc
// MyClass.h
@interface MyClass : NSObject
- (void)sayHello;
@end
```
```objc
// MyClass.m
@implementation MyClass
- (void)sayHello {
    NSLog(@"Hello from instance!");
}
@end
```
```objc
// Main.m
// 使用方式
MyClass *obj = [[MyClass alloc] init];
[obj sayHello];  // ✅ 正確：透過實例呼叫
```


#### `+` 用法 (類別方法)
這是靜態方法，不需要建立物件就能呼叫。常用來提供工廠方法、工具函式、預設值等等。

```objc
// MyClass.h
@interface MyClass : NSObject
+ (NSString *)defaultMessage;
@end
```
```objc
// MyClass.m
@implementation MyClass
+ (NSString *)defaultMessage {
    return @"Default Message";
}
@end
```
```objc
// Main.m
// 使用方式
NSString *msg = [MyClass defaultMessage];  // ✅ 正確：透過類別呼叫
```
<br />



## 前向宣告
「前向宣告（Forward Declaration）」是用來預先告訴編譯器某個類別或協定存在，但暫時不需要知道其詳細內容，這麼做能夠避免循環引用、加快編譯速度。


### 類別的前向宣告：`@class`

- 不需要 `#import "MyClass.h"`，除非你要使用 MyClass 的方法或創建實例。
- 常見於 `.h` 檔案中，避免檔案之間互相 `import（循環依賴）`。
```objc
// Person.h

#import <Foundation/Foundation.h>
// 這樣就夠用了，不需要 #import "Car.h"。

@class Car, A, B; // 前向宣告：我會用到 Car 類別，但現在不需要知道 Car 詳細內容

@interface Person : NSObject
@property (nonatomic, strong) Car *car; // 使用 Car 作為屬性型別
@end
```

#### 避免循環引用
如果兩邊都寫 `#import`，會造成循環引入（編譯失敗）。用 `@class` 可以避免這個問題。

```objc
// Car.h
@class Person;

@interface Car : NSObject
@property (nonatomic, strong) Person *owner;
@end
```
```objc
// Person.h
@class Car;

@interface Person : NSObject
@property (nonatomic, strong) Car *car;
@end
```

#### `#import`使用時機
- 當你要存取那個類別的方法、屬性或實作內容。
- 或者在 `.m` 檔案中真正要使用那個類別
```objc
// Person.m
#import "Car.h" // 在 .m 檔案中，你需要完整的 Car 類別

- (void)drive {
    [self.car startEngine]; // 需要知道 car 有這個方法
}
```


### 協定的前向宣告：`@protocol`

```objc
// MyButton.h

@protocol MyButtonDelegate, XXXDelegate; // 前向宣告協定，與 `@class` 相似。


@interface MyButton : NSObject
@property (nonatomic, weak) id<MyButtonDelegate> delegate;
@property (nonatomic, weak) id<XXXDelegate> delegateTwo;
- (void)press; // 模擬按下按鈕
@end

@protocol MyButtonDelegate <NSObject>
@required
- (void)buttonDidPress:(MyButton *)button;
@end

```







<br />


## `@protocol` 協定

用來定義一組方法的規範，讓其他類別可以「遵守（conform to）這個協定」，並實作這些方法。
它的作用類似於其他語言中的「interface」。

```objc
// ProtocolName.h
@protocol ProtocolName <需要繼承的 Class (可以不寫)>

// 可選的方法
@optional
- (void)optionalMethod;

// 必須實作的方法
@required
- (void)requiredMethod;

@end
```

### 範例
```objc
// Animal.h
@protocol Animal <NSObject>

@required
- (void)speak;

@optional
- (void)run;

@end
```

```objc
// Dog.h
@interface Dog : NSObject <Animal>
@end
```

```objc
// Dog.m
@implementation Dog

- (void)speak {
    NSLog(@"Woof!");
}

// run 是 optional，可以不實作

@end
```
### 限定類型

這樣的參數表示 `animal` 必須是遵守 `Animal` 協定的物件。

```objc
- (void)makeItSpeak:(id<Animal>)animal {
    [animal speak];
}
```


<br />
<br />

