









* TWeakObjectPtr<>
```c++
TWeakObjectPtr<AMyActor> MyActor;

// 判断是否为空（野）指针
MyActor.IsValid();

// 解引用，.运算符是调用TWeakObjectPtr的方法，->运算符是解引用
MyActor.Get();
MyActor->SomeMethod()
```
