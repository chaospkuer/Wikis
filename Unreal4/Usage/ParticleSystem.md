










* ParticleSystemComponent
```c++
// particle system
OurParticleSystem = CreateDefaultSubobject<UParticleSystemComponent>(TEXT("Movement particles"));
OurParticleSystem->SetupAttachment(staticMeshComponent);
OurParticleSystem->bAutoActivate = false;
OurParticleSystem->SetRelativeLocation(FVector(-20, 0, 20));
static ConstructorHelpers::FObjectFinder<UParticleSystem> ParticleAsset(TEXT("/Game/StarterContent/Particles/P_Fire.P_Fire"));
if (ParticleAsset.Succeeded()) {
OurParticleSystem->SetTemplate(ParticleAsset.Object);
}
```
