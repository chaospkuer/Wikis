






# c++ 数组 #
```c++
// 求长度
FVector Touches[EKeys::NUM_TOUCH_KEYS];
uint32 Len = ARRAY_COUNT(Touches);

```

# TArray #
https://docs.unrealengine.com/latest/INT/Programming/UnrealArchitecture/TArrays/
```c++
TArray<Data> MyArray;

// 添加10个空项
MyArray.AddZeroed(10);

// 添加唯一项
MyArray.AddUnique(1234);

// 删除一个位置
MyArray.RemoveAt(1);

// 长度
MyArray.Num();

// 清空，区别见网址
MyArray.Clean();
MyArray.Reset();

```
