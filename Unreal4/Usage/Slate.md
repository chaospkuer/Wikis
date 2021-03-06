











# HUD 与 Widget #
* 不要使用Slate，而使用UMG

* SCompoundWidget写法
这个类包含了MainMenuHUD的整个界面布局
```c++
// 这个类不是UClass，不会展示给蓝图
// SCompoundWidget 与 SLeafWidget对应，顾名思义，是有其他widget组合而成的Widget，SLeafWidget不容纳其他Widget
class SMainMenuUI : public SCompoundWidget {
public:
// 这里到SLATE_END_ARGS将生成一个结构体
SLATE_BEGIN_ARGS(SMainMenuUI)
SLATE_ARGUMENT(TWeakObjectPtr<class AMainMenuHUD>, MainMenuHUD)
SLATE_END_ARGS()

// 这个参数就是上边那个结构体
// 这个函数将在这个widget被创建的时候调用
// 结构体包含了这个widget所需的所有参数
void Construct(const FArguments& args);

// 回调
FReply PlayGameClicked();
FReply QuitGameClicked();

// 存储GameMode的HUD对象
TWeakObjectPtr<class AMainMenuHUD> MainMenuHUD;
}

// BEGIN_SLATE_FUNCTION_BUILD_OPTIMIZATION
//	END_SLATE_FUNCTION_BUILD_OPTIMIZATION
// 这两个宏要包含在Construct两边
BEGIN_SLATE_FUNCTION_BUILD_OPTIMIZATION
void SMainMenuUI::Construct(const FArguments& InArgs) {
MainMenuHUD = InArgs._MainMenuHUD;
ChildSlot
// 用[划分子控件
[
// Populate the widget
// 用SNew新建新子Widget
SNew(SOverlay)
// 用+在新建的子Widget中加入
+ SOverlay::Slot()
.HAlign(HAlign_Center)
.VAlign(VAlign_Top)
[
SNew(STextBlock)
.ColorAndOpacity(FLinearColor::White)
.ShadowColorAndOpacity(FLinearColor::Black)
.ShadowOffset(FIntPoint(-1, 1))
.Font(FSlateFontInfo("Arial", 26))
.Text(FText::FromString("Main Menu"))
]
+ SOverlay::Slot()
.HAlign(HAlign_Right)
.VAlign(VAlign_Bottom)
[
SNew(SVerticalBox)
+ SVerticalBox::Slot()
[
SNew(SButton)
.Text(FText::FromString("Play Game!"))
// 绑定了点击回调事件到 FReply PlayGameClicked
.OnClicked(this, &SMainMenuUI::PlayGameClicked)
]
+ SVerticalBox::Slot()
[
SNew(SButton)
.Text(FText::FromString("Quit Game"))
// 绑定了点击回调事件到 FReply QuitGameClicked
.OnClicked(this, &SMainMenuUI::QuitGameClicked)
]
]
];
}
END_SLATE_FUNCTION_BUILD_OPTIMIZATION

FReply SMainMenuUI::PlayGameClicked() {
if (GEngine) {
GEngine->AddOnScreenDebugMessage(-1, 0.5, FColor::Green, TEXT("Play"));
}

// 如果想让蓝图处理这个函数，可以调用HUD的UFUNRCTION(BlueprintImplementableEvent)方法。
// 不可能在本控件类中让蓝图处理点击事件，因为本控件不是UClass
// MainMenuHUD->PlayGameClicked();

// 告诉引擎回调事件已经处理完了
return FReply::Handled();
}

FReply SMainMenuUI::QuitGameClicked() {
if (GEngine) {
GEngine->AddOnScreenDebugMessage(-1, 0.5, FColor::Green, TEXT("Quit"));
}

// 如果想让蓝图处理这个函数，可以调用HUD的UFUNRCTION(BlueprintImplementableEvent)方法。
// 不可能在本控件类中让蓝图处理点击事件，因为本控件不是UClass
// MainMenuHUD->QuitGameClicked();

// 告诉引擎回调事件已经处理完了
return FReply::Handled();
}
```

* HUD加载SCompoundWidget
HUD是蓝图与Widget的中间层，Widget没有UClass，只有通过HUD才能让蓝图处理点击事件
具体方式是在Widget中存放HUD指针，在Widget中调用HUD中的UFUNCTION(BlueprintImplementableEvent)方法
```c++
class AMainMenuHUD : public HUD {
virtual void PostInitializeComponents() override;

// 为了让Widget点击事件回调给蓝图处理
// 因为Widget没有UClass所以没法直接回调给蓝图
// HUD系统就是游戏与Widget的桥梁
UFUNCTION(BlueprintImplementableEvent)
void PlayGameClicked();

// 为了让Widget点击事件回调给蓝图处理
// 因为Widget没有UClass所以没法直接回调给蓝图
// HUD系统就是游戏与Widget的桥梁
UFUNCTION(BlueprintImplementableEvent)
void QuitGameClicked();

// 在HUD系统中子Widget指针
TSharedPtr<class SMainMenuUI> MainMenuUI;
}

void AMainMenuHUD::PostInitializeComponents() {
Super::PostInitializeComponents();

// 将HUD中Widget变量初始化并赋值，并填充Widget的SLATE_ARGUMENT的值
SAssignNew(MainMenuUI, SMainMenuUI).MainMenuHUD(this);

// 将Widget加载到屏幕上
if (GEngine->IsValidLowLevel()) {
GEngine->GameViewport->AddViewportWidgetContent(
SNew(SWeakWidget).PossibleNullContent(MainMenuUI.ToSharedRef())
);
}
}

```

# Widget Style #
* 非常繁琐
https://wiki.unrealengine.com/Slate_Style_Sets_Part_2

# Widget Binding #
```c++
class SMainMenuUI : public SCompoundWidget {
TAttribute<FText> Title;
FText GetTitle() const;
}

void SMainMenuUI::Construct(const FArgument& InArgs) {
Title.Bind(this, &SMainMenuUI::GetTitle);
//上一句bind一定要写在ChileSlot之前，否则不能每帧调用GetTitle()
// ChildSlot[

//	]
}

FText SMainMenuUI::ZGetTitle() const {
return FText::FromString(TEXT("Binding Title"));
}
```
