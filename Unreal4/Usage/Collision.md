








# collision #
* collision profile principle
1、碰撞分为3种：ignore, overlap, block，草对于子弹是ignore，对于visibility是block各种volumn对于物体是overlap的
2、traceChannel：比如子弹(weapon)，visibility是两个traceChannel，他们对于草，玻璃和墙的ignore/overlap/block的属性不一样
objectChannel：比如草和墙就是ObjectChannel，ObjectChannel与ObjectChannel发生的碰撞也有这三种情况
unreal有一些内置的traceChannel如：visibility,Camera和ObjectChannel如：WorldStatic,WorldDynamic...，可以在projectSetting中看到他们
3、一个对于WorldStatic处理是block的遇到另一个对于WorldStatic处理是Overlap的相撞结果是overlap
ignore与ignore/overlap,block相撞都是ignore
overlap与除ignore外的所有相撞都是overlap，会发出overlap事件
只有block与block相撞是block，会发出hit事件
4、每个物体有两级碰撞区域：简单、复杂。可以设定简单碰撞和复杂碰撞。子弹与其他碰撞为复杂碰撞，因为要指哪儿打哪儿。人与车碰撞为简单碰撞，不用太细。
总结：bool GetHitResultAtScreenPosition(const FVector2D ScreenPosition, const ETraceTypeQuery TraceChannel, bool bTraceComplex, FHitResult& HitResult) const;
??代码里有句SetCollisionEnabled，类型分：无碰撞、查询碰撞、查询及物理碰撞、物理碰撞四种，指的是啥

* custom self collision profile
[custom self collision profile](https://docs.unrealengine.com/latest/INT/Programming/Tutorials/FirstPersonShooter/3/3/index.html)


* code
{{https://docs.unrealengine.com/latest/INT/Programming/Tutorials/FirstPersonShooter/3/4/index.html}}
1、写了下边代码之后，还必须在蓝图MyActor蓝图的Collision组件中设置presets为
MyCollisionComponent->BodyInstace.SetCollisionProfileName(TEXT("Projectile"));
上句话中"projectile"的内容选项
2、需要在被撞物体的Physics Category中，将Simulate Physics设置为true
3、如果还是不能实现预期效果，需要被撞物体的Collision Category中修改presents选项
```c++
class MyActor : public AActor {
CollisionComponent* MyCollisionComponent;

UFUNCTION()
void OnHit(UPrimitiveComponent* HitComponent, class AActor* OtherActor, UPrimitiveComponent* OtherComponent, FVector NormalImpulse, const FHitResult& Hit);

}

MyActor::MyActor() {
MyCollisionComponent->BodyInstace.SetCollisionProfileName(TEXT("Projectile")); // set collision profile name
MyCollisionComponent->OnComponentHit.AddDynamic(this, &MyActor::OnHit);
}

void MyActor::OnHit(UPrimitiveComponent* HitComponent, AActor* OtherActor, UPrimitiveComponent* OtherComponent, FVector NormalImpulse, const FHitResult& Hit) {
if (OtherActor != this && OtherComponent->IsSimulatingPhysics()) {
OtherComponent->AddImpulseAtLocation(MyCollisionComponent,/*ProjectileMovementComponent->Velocity*/ 100 * 100.0f, Hit.ImpactPoint);
}
}
```

* USphereComponent
```c++
USphereComponent* sphereComponent = CreateDefaultSubobject<USphereComponent>(TEXT("RootComponent"));
//RootComponent = sphereComponent;
sphereComponent->AttachToComponent(GetCapsuleComponent(), FAttachmentTransformRules::KeepRelativeTransform)
sphereComponent->SetSphereRadius(40.0f);
sphereComponent->SetCollisionProfileName("Pawn");
```

# Trace #
```c++
FHitResult Hit;
FCollisionObjectQueryParams ObjectParams(FCollisionObjectQueryParams::AllStaticObjects);
GetWorld->LineTraceSingleByObjectType(Hit, FromLocation, ToLocation, ObjectParams);
// Hit.Actor是个TWeakObectPtr<>，非空证明发生了碰撞。用IsValid判断Hit.Actor非空
if (Hit.Actor.IsValid()) {
// 说明碰撞了
}
```


# FBox #
FBox就是AABB盒

* 初始化
```c++
// 用三个点的坐标和参数3进行初始化
FBox(const FVector* Points, int32 Count)
// 用最小顶点坐标和最大顶点坐标初始化
FBox(FVector Min, FVector Max)
```

