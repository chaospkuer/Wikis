









* pawn自带collisionComponent,

* 遍历世界中的Pawn
```c++
for (FConstPawnIterator Iterator = GetWorld()->GetPawnIterator(); Iterator; ++Iterator) {
AMyPawn* Pawn = Cast<AMyPawn>(*Iterator);
}
```
