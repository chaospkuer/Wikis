




# Add Component #

* staticMesh component normal setting
```c++
// 注意区分StaticMesh 和HierarchicalInstancedStaticMesh，如果设置mesh时总不出现在屏幕上，很可能勿用了HierarchicalInstancedStaticMesh
UStaticMeshComponent* staticMeshComponent = CreateDefaultSubobject<UStaticMeshComponent>(TEXT("VisualSphere"));
staticMeshComponent->SetupAttachment(RootComponent);
static ConstructorHelpers::FObjectFinder<UStaticMesh> sphereAsset(TEXT("/Engine/BasicShapes/Sphere.Sphere"));
if (sphereAsset.Succeeded()) {
staticMeshComponent->SetStaticMesh(sphereAsset.Object);
staticMeshComponent->SetRelativeLocation(FVector(0, 0, 0));
staticMeshComponent->SetWorldScale3D(FVector(0.8f));
}
```



# Get Component #
* Actor::FindComponentByClass
```c++
UPawnMovementComponent* APawn::GetMovementComponent() const
{
return FindComponentByClass<UPawnMovementComponent>();
}
```

