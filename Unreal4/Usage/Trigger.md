




* TriggerBox
```c++
class AMyActor : public AActor {
private:
UPROPERTY(VisibleAnywhere, BlueprintReadOnly, Category = Touch, meta = (AllowPrivateAccess = "true"))
UBoxComponent* TriggerBox;
}

AMyActor::AMyActor(const FObjectInitializer& ObjectInitializer)
:Super(ObjectInitializer) {
TriggerBox = CreateDefaultSubobject<UBoxComponent>(TEXT("Box"));
TriggerBox->bVisible = true;
TriggerBox->bHiddenInGame = true;
TriggerBox->CastShadow = false;
TriggerBox->InitBoxExtent(FVector(512, 128, 128));
TriggerBox->RelativeLocation = FVector(512, 0, 128);
TriggerBox->BodyInstance.SetCollisionEnabled(ECollisionEnabled::QueryOnly);
TriggerBox->BodyInstance.SetResponseToAllChannels(ECR_Ignore);
TriggerBox->BodyInstance.SetResponseToChannel(ECC_Pawn, ECR_Overlap);
TriggerBox->SetupAttachment(RootComponent);
}
```
