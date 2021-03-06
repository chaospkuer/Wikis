











# class #
GENERATED_BODY会自动生成构造函数MyClass::MyClass(const FObjectInitializer& ObjectInitializer):Super(ObjectInitilizer){}
GENERATED_UCLASS_BODY()不会自动生成构造函数，需要自己写
```c++
UCLASS()
class MyActor : public AActor{
GENERATED_UCLASS_BODY()	// 没有;
};
MyActor::MyActor(const FObjectInitilizer& objectInitializer) {
PrimaryActorTick.bCanEverTick = true;
}
```

# interface #
[蓝图定义Interface](https://docs.unrealengine.com/latest/INT/Engine/Blueprints/UserGuide/Types/Interface/index.html)
[蓝图实现Interface](https://docs.unrealengine.com/latest/INT/Engine/Blueprints/UserGuide/Types/Interface/UsingInterfaces/index.html)
## Interface的用法思想 ##
下边这段程序，是运行时多态。AActor本身没有继承IStrategyTeamInterface接口，但传进来的参数可能会继承这个接口。
```c++
bool AStrategyGameMode::OnEnemyTeam(const AActor* A, const AActor* B) {
// 注意这里必须是const，const基类转化const父类
const IStrategyTeamInterface* TeamA = Cast<const IStrategyTeamInterface>(A);
const IStrategyTeamInterface* TeamB = Cast<const IStrategyTeamInterface>(B);

return TeamA && TeamB && TeamA->GetTeamNum() != TeamB->GetTeamNum();
}
```


GENERATED_UINTERFACE_BODY()不会自动生成构造函数，需要自己写
先用类生成器生成继承Object的类
## 接口类 ##
```c++
UINTERFACE()
class UMyInterface : public UInterface {
GENERATED_UINTERFACE_BODY()	// 没有;

// 这里什么都不写
}
class IMyInterface {
GENERATED_IINTERFACE_BODY() // 没有;

// 这里写所有的接口

// 接口函数的第一种写法
UFUNCTION(BlueprintNativeEvent)
void MyInterface();

// 接口函数的第二种写法
virtual void MyInterface2() = 0;
}
// 在cpp中写UInterface的构造函数
UMyInterface::UMyInterface(const FObjectInitializer& ObjectInitializer)
: Super(ObjectInitializer) {

}
```

## 实现接口的类 ##
```c++
UCLASS()
class AMyClass : public IMyInterface {
GENERATED_BODY()

// 实现第一种接口函数，接口类不是virtual函数的，需要加_Implementation进行实现，注意实现接口类的实现函数都是virtual的
virtual void MyInterface_Implementation() override;

// 实现第二种接口函数
virtual void MyInterface2() override;
}

void AMyClass::MyInterface_Implementation() {

}

void AMyClass::MyInterface2() {

}
```

# Enum #
## 写法1 ##
```c++
// 定义
ENUM(BlueprintType)
namespace EMyEnum {
enum Type {
Type1,
Type2,
Type3,
}
}

// 使用
EMyEnum Type = EMyEnum::Type1;

// 蓝图化，在蓝图中会出现枚举的下拉选框
UPROPERTY(EditAnywhere)
TEnumAsByte<EMyEnum::Type> EnumType;
```

## 写法2 ##
```c++
// 定义
ENUM(BlueprintType)
enum EMoveMode {
MOVE_Walking UMETA(DisplayName="Walking");
}
// 使用
EMoveMode Type = MOVE_Walking;

// 蓝图化，在蓝图中会出现枚举的下拉选框
// 下拉选框的文字会自动会使用UMETA(DisplayName = "xxx")的
UPROPERTY(EditAnywhere)
TEnumAsByte<EMoveMode> MoveType;
```
