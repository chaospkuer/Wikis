








# Spawn Actor #
* spawn一个Actor，然后传参数给它的方法
只能自己写个Init函数，在SpawnActor之后调用

* SpawnActor函数
如果这个函数执行了，没有产出，很有可能是ActorSpawnParamters.SpawnCollisionHandlingOverride是默认值，当它是默认值时，由于collision可能就不产出了
```c++
// 返回AActor的版本
AActor* TheActor = GetWorld()->SpawnActor(ActorClass, LocationVector, RatationVector, ActorSpawnParamters);
AActor* TheActor = GetWorld()->SpawnActor(ActorClass, Transform, ActorSpawnParamters);
AActor* TheActor = GetWorld()->SpawnActorAbsolute(ActorClass, Transform, ActorSpawnParamters);

// 返回指定派生类的版本
AMyActor* TheMyActor = GetWorld()->SpawnActor<AMyActor::StaticClass()>(LocationVector, RatationVector, ActorSpawnParamters);
AMyActor* TheMyActor = GetWorld()->SpawnActor<AMyActor::StaticClass()>(Transform, ActorSpawnParamters);
AMyActor* TheMyActor = GetWorld()->SpawnActorAbsolute<AMyActor::StaticClass()>(ActorSpawnParamters);

// 允许用SubActorClass Spawn然后尝试转化成父类的版本
AMyActor* TheMyActor = GetWorld()->SpawnActor<AMyActor::StaticClass()>(SubActorClass, ActorSpawnParamters);
AMyActor* TheMyActor = GetWorld()->SpawnActor<AMyActor::StaticClass()>(SubActorClass, LocationVector, RatationVector, ActorSpawnParamters);
AMyActor* TheMyActor = GetWorld()->SpawnActor<AMyActor::StaticClass()>(SubActorClass, Transform, ActorSpawnParamters);
AMyActor* TheMyActor = GetWorld()->SpawnActorAbsolute<AMyActor::StaticClass()>(SubActorClass, Transform, ActorSpawnParamters);

// SpawnActorDeferred 与 UGameplayStatics::FinishSpawnActor连用的方法
```

* FActorSpawnParameters
| 参数                                                             | 意义                                                                  |
|------------------------------------------------------------------|-----------------------------------------------------------------------|
| FName FName                                                      | 在编辑器中的显示，如果不指定，则是[class]_[number]                    |
| AActor* Template                                                 | 如果指定，则用Template的值初始化，否则用CDO                           |
| AActor* Owner                                                    | 设置Owner                                                             |
| APawn* Instigator                                                | 当这个Actor Spawn的时候对Instigator造成伤害                           |
| ULevel* OverrideLevel                                            | 产生在哪个Level                                                       |
| ESpawnActorCollisionHandingMethod SpawnCollisionHandlingOverride | 如果碰撞了怎么处理，如忽视，永远Spawn。微调Spawn。碰撞了就不Spawn等等 |
| uint16 bRemoteOwned                                              | 是否remoted owned                                                     |
| uint16 bNoFail                                                   | 发生错误时不舍弃，静态或TemplateActor与Spawn类不同时，不会舍弃        |
| uint16 bDeferConstruction                                        | 是否在Spawn时执行Constructure，只在蓝图中起作用                       |
| uint16 bAllowDuringConstructScript                               | 如果正在Spawn则舍弃本次生成                                           |
| EObjectFlags                                                     | 其他Flag                                                              |

# NewObject #
```c++
T* UMyObject = NewObject<T>(Outer, NewObjectClass, NewObjectName);
```


