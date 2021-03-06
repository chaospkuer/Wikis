













# UCLASS #
class 修饰符
https://docs.unrealengine.com/latest/INT/Programming/UnrealArchitecture/Reference/Classes/Specifiers/index.html
只要是UCLASS，在unreal类向导中就有了

# USTRUCT #
struct 修饰符
https://docs.unrealengine.com/latest/INT/Programming/UnrealArchitecture/Reference/Structs/Specifiers/index.html

# UINTERFACE #
interface 修饰符
https://docs.unrealengine.com/latest/INT/Programming/UnrealArchitecture/Reference/Interfaces/Specifiers/index.html

# UFUNCTION #
function 修饰符
https://docs.unrealengine.com/latest/INT/Programming/UnrealArchitecture/Reference/Functions/Specifiers/index.html
* UFUNCTION(BlueprintNativeEvent)
* 与BlueprintImplementableEvent的不同在于c++端有默认实现
* in c++
```c++
class AMyActor : public AActor {
UFUNCTION(BlueprintNativeEvent)
void Foo();
virtual void Foo_Implementation();
virtual void BeginPlay() override;
}
void AMyActor::Foo_Implementation() {
GEngine->AddOnScreenDebugMessage(-1, 5, FColor::Yellow, TEXT("this is c++'s implementation"));
}
virtual void AMyActor::BeginPlay() {
Foo();
}
```
* in blueprint
{{file:BlueprintNativeEvent.png}}

* UFUNTION(BlueprintImplementableEvent)
* 与BlueprintNativeEvent的不同在于c++端没有默认实现
* in c++
```c++
class AMyActor : public AActor {
UFUNCTION(BlueprintImplementableEvent)
void Foo();
virtual void BeginPlay() override;
}
virtual void AMyActor::BeginPlay() {
Foo();
}
```
* in blueprint
{{file:BlueprintImplementableEvent.png}}

# UPROPERTY #
property 修饰符
https://docs.unrealengine.com/latest/INT/Programming/UnrealArchitecture/Reference/Properties/Specifiers/index.html
只有组件中某个字段加上UPROPERTY不能在Editor中编辑。只有组件本身加上UPROPERTY后才可以在Editor中编辑
1. 编辑位置
* EditDefaultsOnly 只有原型
* EditInstanceOnly 只有实例
* EditAnywhere 原型和实例

# Metadata #
Metadata 修饰符
https://docs.unrealengine.com/latest/INT/Programming/UnrealArchitecture/Reference/Metadata/index.html

* DisplayName
如下边函数，原函数叫SetBuffModifier，用meta=(DisplayName = "Set Unit Properties")将蓝图中该函数名称改为"Set Unit Properties"
UFUNCTION(BlueprintCallable, Category=Pawn, meta=(DisplayName = "Set Unit Properties"))
void SetBuffModifier(AStrategyChar* Pawn, int32 AttackMin, int32 AttackMax, int32 DamageReduction, int32 MaxHealthBonus, int32 HealthRegen, float Speed, int32 DrunkLevel, float Duration, bool bInfiniteDuration, float CustomScale = 1.0, float AnimaRate = 1);

