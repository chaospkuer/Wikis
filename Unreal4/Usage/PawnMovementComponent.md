


#蓝图

* MovementComponent
```c++
class UCollisionPawnMovementComponent : public UPawnMovementComponent {
public:
virtual void TickComponent(float DeltaTime, enum ELevelTick TickType, FActorComponentTickFunction* ThisTickFunction) override;
}

void UCollisionPawnMovementComponent::TickComponent(float DeltaTime, enum ELevelTick TickType, FActorComponentTickFunction* ThisTickFunction) {
Super:TickComponent(DeltaTime, TickType, ThisTickFunction);

if (!PawnOner || !UpdatedComponent || ShouldSkipUpdate(DeltaTime)) {
return;
}

FVector DesiredMovementThisFrame = ConsumeInputVector().GetClampedToMaxSize(1.0f) * DeltaTime * 150.0f;
if (!DesiredMovementThisFrame.IsNearlyZero()) {
FHitResult Hit;
SafeMoveUpdatedComponent(DesiredMovementThisFrame, UpdatedComponent->GetComponentRotation(), true, Hit);

// If we bumped into something, try to slide along it
if (Hit.IsValidBlockingHit()) {
SlideAlongSurface(DesiredMovementThisFrame, 1.f - Hit.Time, Hit.Normal, Hit);
}
}
}
```
