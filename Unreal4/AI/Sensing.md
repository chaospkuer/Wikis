




# UPawnSensingComponent #
## 常用Properties ##
| 参数                    | 意义                                                                                                |
|-------------------------|-----------------------------------------------------------------------------------------------------|
| float SensingInterval   | 多长时间SensingUpdate一次，<=0禁止SensingUpdate                                                     |
| float SightRadius       | 最远可视距离                                                                                        |
| uint32 bOnlySensePlayer | 只感知Player-Controlled Pawns                                                                       |
| uint32 bSeePawns        | 允许视觉感知，在看到Pawn时，回调                                                                    |
| uint32 bHearNoises      | 允许听觉感知，在有噪音时被听到时回调。注意：当bSeePawns是true，并且可以看到那个Pawn，则不会触发这个 |

## 常用Function ##
| 函数                          | 意义       |
|-------------------------------|------------|
| void SetPeripheralVisionAngle | 可视角度？ |


