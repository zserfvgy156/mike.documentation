## .h 檔宣告
<br />


### 一個基本的 `.h` 檔案範例
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



### @property 的常見屬性（attributes）

- `atomic`、`nonatomic` 擇一
   * `nonatomic`：非原子性（效能較好，最常用）
   * `atomic`：預設值，執行緒安全但效能較差（很少特別寫）
- 其他屬性
   * `strong`：預設用於物件型別（表示擁有該物件）
   * `weak`：弱引用，通常用於 delegate，避免 retain cycle
   * `assign`：用於基本資料型別（如 int, float, BOOL）
   * `copy`：用於 NSString 或 block，保證是不可變的複製體



