








# Location #
* 在初始化时设置USceneComponent的Loaction
```c++
SceneComponent->SetRelativeLocation(FVector(200f, 200f, 200f));
```

* 用SafeMoveUpdatedComponent
[PawnMovementComponent](PawnMovementComponent.md)

* 用AddMovementInput
[Character](Character.md)

* 在每个tick中设置位置
```c++
void AMyPawn::Tick( float DeltaTime )
{
if (!CurrentVelocity.IsZero()) {
FVector NewLocation = GetActorLocation() + (CurrentVelocity * DeltaTime);
SetActorLocation(NewLocation);
}
}

void AMyPawn::SetupPlayerInputComponent(class UInputComponent* InputComponent)
{
InputComponent->BindAxis("MoveForward", this, &AMyPawn::MoveForward);
InputComponent->BindAxis("MoveRight", this, &AMyPawn::MoveRight);
}

void AMyPawn::MoveForward(float AxisValue) {
CurrentVelocity.X = FMath::Clamp(AxisValue, -1.0f, 1.0f) * 100.0f;
}
void AMyPawn::MoveRight(float AxisValue) {
CurrentVelocity.Y = FMath::Clamp(AxisValue, -1.0f, 1.0f) * 100.0f;
}
```

* GetActorLocation
这个函数返回RootComponent的世界坐标

# Rotation #
* 初始化时设置USceneConponent的Rotation
```c++
SceneComponent->SetRelativeRotation(FRotator(45.0f, 0.0f, 250.0f));
```




# Scale #
* 在Tick中设置MeshComponent的scale
```c++
void AMyPawn::Tick( float DeltaTime ) {
float CurrentScale = USceneComponent->GetComponentScale().X;
if (bGrowing) {
// Grow to double size over the course of one second
CurrentScale += DeltaTime;
} else {
// Shrink half as fast as we grow
CurrentScale -= (DeltaTime * 0.5f);
}
// Make sure we never drop below our starting size, or increase past double size.
CurrentScale = FMath::Clamp(CurrentScale, 1.0f, 2.0f);
USceneComponent->SetWorldScale3D(FVector(CurrentScale));
}
```



[Rotating Component](https://wiki.unrealengine.com/Blueprint_Rotating_Movement_Component)