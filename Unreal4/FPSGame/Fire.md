










{{https://docs.unrealengine.com/latest/INT/Programming/Tutorials/FirstPersonShooter/3/2/index.html}}
```c++
class AFPSCharacter : public Character {
UFUNCTION()
void Fire();

UPROPERTY(EditAnywhere, BlueprintReadWrite, Category = Gameplay)
FVector MuzzleOffset;

UPROPERTY(EditDefaultsOnly, Category = Projectile)
TSubclassOf<class AFPSProjectile> ProjectileClass;

}

void AFPSCharacter::Fire() {
// we need judge if user fill this property by a blueprint first
if (ProjectileClass) {
// get the camera transform
FVector CameraLocation;
FRotator CameraRotation;
GetActorEyesViewPoint(CameraLocation, CameraRotation);

// transform muzzle offset from camera space to world space
FVector MuzzleLocation = CameraLocation + FTransform(CameraRotation).TransformVector(MuzzleOffset);
FRotator MuzzleRotation = CameraRotation;

// skew the aim to be slightly upwards
MuzzleRotation.Pitch += 10.0f;
UWorld* World = GetWorld();

if (World) {
FActorSpawnParameters SpawnParam;
SpawnParam.Owner = this;
SpawnParam.Instigator = Instigator;
AFPSProjectile* Projectile = World->SpawnActor<AFPSProjectile>(ProjectileClass, MuzzleLocation, MuzzleRotation, SpawnParam);
if (Projectile)
{
FVector LaunchDirection = MuzzleRotation.Vector();
Projectile->FireInDirection(LaunchDirection);
}
}
}
}
```


```c++
class AFPSProjectile : public AActor {
// we only need specify direction, because speed is in projectileMovement component
void FireInDirection(const FVector& ShootDirection);
}

void AFPSProjectile::FireInDirection(const FVector& ShootDirection) {
ProjectileMovementComponent->Velocity = ShootDirection * ProjectileMovementComponent->InitialSpeed;
}
```
