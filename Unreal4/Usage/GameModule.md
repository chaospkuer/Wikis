








# 简介 #
[架构介绍](https://docs.unrealengine.com/latest/INT/Programming/UnrealArchitecture/Reference/index.html)

[介绍和实例教程](https://docs.unrealengine.com/latest/INT/Programming/Plugins/)

* 创建类时public,private与module的关系
* 新建c++class时选的public结构、private结构和不选时的结构
* public和private文件夹结构是针对其他module定义的。文件夹结构可以在后边通过修改文件目录，重新生成uproject修改
* public将.h文件放在public文件夹中，.pp文件放在private文件夹中。

* module
* SWidgets一般是私有，可以理解，因为只是用于这个模块的展示，对于其他模块透明。
* 可以加入其它Module，可参考：https://wiki.unrealengine.com/Creating_an_Editor_Module
* 也可参考：官方策略游戏


* Slate Style对GameMode的运用
[Slate](Slate.md)


#Plateform
[FDesktopPlatformModule API](https://docs.unrealengine.com/latest/INT/API/Developer/DesktopPlatform/FDesktopPlatformModule/index.html)
[IDesktopPlatform API](https://docs.unrealengine.com/latest/INT/API/Developer/DesktopPlatform/IDesktopPlatform/index.html)
[FGenericPlatformProcess FWindowsPlatformProcess FLinuxPlatformProcess FMacPlatformProcess](http://docs.unrealengine.com/latest/INT/API/Runtime/Core/GenericPlatform/FGenericPlatformProcess/)

# Build
[UnrealPak使用方式](https://www.reddit.com/r/unrealengine/comments/3ou66r/using_unrealpak_to_open_pak_files/)
