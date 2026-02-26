## id vs instancetype
<br />


`id` 和 `instancetype` 在 Objective-C 中都代表「物件類型」，但它們的使用目的和行為不同，這點對理解程式碼正確性和維護性非常關鍵。
<br />

| 特性 | id | instancetype |
| :----: | :----: | :----: |
| 表示 | 任何物件類型 | 呼叫該方法所屬的「實際類別」 |
| 型別檢查 | 編譯時 `不會` 檢查具體類型 | 編譯時 `會` 根據實際類別檢查 |
| 主要用途 | 用在彈性回傳任何物件 | 用在工廠方法或初始化方法中 |
| 常見用法 | `- (id)init;` | `+ (instancetype)person;` |
| 型別安全性 | 較低 | 較高 |


### 範例說明
#### `id` 範例（較不安全）
```objc
// XXX.m
// 此處回傳 NSString 類型
- (id)createObject {
    return [[NSString alloc] initWithString:@"Hi"];
}
```
```objc
// Main.m
id obj = [self createObject];

// 外部不會知道具體是什麼型別，編譯器不會警告，但可能在執行時出錯。
// 在 swift 會顯示 Any?
[obj length]; 

```



#### `instancetype` 範例（較安全）：
```objc
// MyPerson.m
+ (instancetype) person {
    return [[self alloc] init];
}
```
```objc
// Main.m
// 編譯器知道這是 MyPerson*，可以自動補全、型別檢查等。
MyPerson *p = [MyPerson person]; 

```

### 使用建議


| 使用情境 | 建議使用 |
| :---------------- | :------: |
| 不確定會回傳哪一類別 | `id` |
| 建構子（如 `init` 或工廠方法） | `instancetype` |
| 要保持型別安全性與補全支援 | `instancetype` 優先 |


### `id<XXX>` 是什麼意思？

表示「任何符合 XXX 協定的物件」，一個物件可以符合多個協定。
> < XXX >：限定這個物件必須符合某個 protocol（協定）

```objc
// 協議一
@protocol A
-(void) methodA;
@end

// 協議二
@protocol B <A>
-(void) methodB;
@end


// 使用 id<A, B>
- (void)testMethod:(id<A, B>)test {
    // ✅ 安全，因為已保證實作了 methodA 與 methodB 方法
    [test methodA]; 
    [test methodB];
}

```



