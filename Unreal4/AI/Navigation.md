











# Navigation #
## 介绍 ##
官方提供AAIController用于操作AI
AIController可以作为状态机，在StrategyGame中，StrategyAIController就作为状态机。在Tick中更新状态。

## 自动寻路和移动 ##
开始寻路，完成回调和停止移动。
TODO:研究怎么在地图上摆Nav的组件
```c++
// 开始移动函数
MyAIController->MoveToLocation(Destination, TargetAcceptanceRadius, true, true, true);

// 注册移动完成后的回调，在MyAIController中的虚函数
virtual void OnMoveCompleted(FAIRequestID RequestID, EPathFollowingResult::Type Result) override;

// 停止移动函数
MyAIController->GetPathFollowingComponent()->AbortMove(TEXT("abort brewery"));
```

## UPathFollowingComponent ##
AAIController自带PathFollowingComponent，跟随行走路线行走
[AIController](AIController.md)

常用Properties
| 参数 | 意义 |
|------|------|
|      |      |

常用Functions
| 函数      | 意义 |
|-----------|------|
| AbortMove |      |

## EPathFollowingStatus ##
* Idle:没有request
* Waiting:request了但路径不完整，在UpdateMove()后开始
* Paused:request暂停了，在ResumeMove()后继续
* Moving:跟随路径
