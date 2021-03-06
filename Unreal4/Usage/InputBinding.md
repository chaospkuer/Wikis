






* 使用InputComponent作为输入处理（探查）类
Pawn中的SetupPlayerInputComponent自带输入处理组件，在SetupPlayerInputComponent中自定义InputBinding
```c++
AMyPawn::AMyPawn() {
// set MyPawn to recevie player input
AutoPossessPlayer = EAutoReceiveInput::Player0;
}

void AMyPawn::SetupPlayerInputComponent(class UInputComponent* InputComonent) {
Super:SetupPlayerInputComponent(InputComponent);

// bind to movement
InputComponent->BindAction("Grow", IE_Pressed, this, &AMyPawn::StartGrowing);
InputComponent->BindAction("Grow", IE_Released, this, &AMyPawn::StopGrowing);
InputComponent->BindAxis("MoveForward", this, &MyPawn::MoveX);
InputComponent->BindAxis("MoveRight", this, &MyPawn::MoveY);

// bind to rotation
InputComponent->BindAxis("Turn", this, &AddControllerYawInput);
InputComponent->BindAxis("LookUp", this, &AddControllerPitchInput);
}

void AMyPawn::StartGrouwing() {}
void AMyPawn::StopGrowing(){}
void AMyPawn::MoveX(float AxisValue){}
void AMyPawn::MoveY(float AxisValue){}
```

* 自定义输入处理（探查）类
PlayerController中的SetupPlayerInputComponent不自带输入处理类，自己可以定制输入处理类，当然也要自己定制InputBinding
```c++
class AMyPlayerController : public APlayerController {
virtual void SetupInputComponent() override;
virtual void ProcessPlayerInput(const float DeltaTime, const bool bGamePaused) override;

// 这个InputHandler用于识别用户输入了如TapPressed, HoldPressed等等操作
// 然后回调给下边的这些函数
MyInputHandler* InputHandler;

void OnTapPressed(const FVector2D& ScreenPosition, float DownTime);
void OnHoldPressed(const FVector2D& ScreenPosition, float DownTime);
void OnHoldReleased(const FVector2D& ScreenPosition, float DownTime);
void OnSwipeStarted(const FVector2D& AnchorPosition, float DownTime);
void OnSwipeUpdate(const FVector2D& ScreenPosition, float DownTime);
void OnSwipeReleased(const FVector2D& ScreenPosition, float DownTime);
}

void AMyPlayerController::SetupInputComponent() {
// 在这个函数中实例化自己定制的输入处理类
InputHandler = CreateInputHandler();
// 下边这种初始化用InputHandler的Outer存储这个类，在InputHandler中获得PlayerController只需要GetOuter()即可
// InputHandler = NewObject<AMyInputHandler>(this);

// 在这个函数中绑定输入处理类的回调函数
BIND_1P_ACTION(InputHandler, EGameKey::Tap, IE_Pressed, &AMyStrategyPlayerController::OnTapPressed);
BIND_1P_ACTION(InputHandler, EGameKey::Hold, IE_Pressed, &AMyStrategyPlayerController::OnHoldPressed);
BIND_1P_ACTION(InputHandler, EGameKey::Hold, IE_Released, &AMyStrategyPlayerController::OnHoldReleased);
BIND_1P_ACTION(InputHandler, EGameKey::Swipe, IE_Pressed, &AMyStrategyPlayerController::OnSwipeStarted);
BIND_1P_ACTION(InputHandler, EGameKey::Swipe, IE_Repeat, &AMyStrategyPlayerController::OnSwipeUpdate);
BIND_1P_ACTION(InputHandler, EGameKey::Swipe, IE_Released, &AMyStrategyPlayerController::OnSwipeReleased);
}

// 引擎每帧会调用这个函数，在这里每帧调用InputHandler的探查函数
// 也是在探查函数的执行过程中InputHandler会回调给相应的函数
void AMyPlayerController::ProcessPlayerInput(const float DeltaTime, const bool bGamePaused) {
InputHandler->DetectInput(DeltaTime);
}

// 输入处理类会在用户有相应输入时回调给相应函数，相应函数再根据具体逻辑处理
void AMyPlayerController::OnTapPressed(const FVector2D& ScreenPosition, float DownTime){}
void AMyPlayerController::OnHoldPressed(const FVector2D& ScreenPosition, float DownTime){}
void AMyPlayerController::OnHoldReleased(const FVector2D& ScreenPosition, float DownTime){}
void AMyPlayerController::OnSwipeStarted(const FVector2D& AnchorPosition, float DownTime){}
void AMyPlayerController::OnSwipeUpdate(const FVector2D& ScreenPosition, float DownTime){}
void AMyPlayerController::OnSwipeReleased(const FVector2D& ScreenPosition, float DownTime){}
```
