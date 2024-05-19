# 常见问题

## Q：一帧内具体可以执行什么操作，如何结算一帧内操作的效果？

所有可以执行的操作可以参考Python或C++ SDK文档。每tick内的操作按照先到先处理，譬如玩家在血量为1时，在同一tick内，服务端先收到对手攻击自己的操作，再收到使用急救包的操作，那么会死亡；反之如果服务端先收到使用急救包的操作则不会死亡。

## Q：武器 CD 是指被硬控在原地还是只是两次使用该武器的攻击的冷却？

两次使用该武器攻击的冷却。

## Q：对于指定位置，武器的攻击范围/弹道是怎么计算（到指定位置为止还是覆盖整条直线）？

指定位置会被换算为射击方向，然后发出射线，直到达到射程限制或首次击中物体为止。武器发射的是一条没有宽度的激光射线（霰弹枪会有多条射线），玩家在攻击时需给定一个发射方向，射线长度是该武器的攻击范围，在本 tick 内会计算这个射线的路径，若先穿过某玩家的碰撞箱，则该玩家扣血，如果先打到墙就不生效。

## Q：拾取物品的范围是多少？

选手可以拾取 当前脚下 1×1 方格上 的资源，若背包容量充足，资源会直接放入背包，拾取后资源消失。如何得到当前脚下是哪一块方格呢？选手可以通过 SDK 获取自己当前的位置，然后将其向下取整即可得到当前脚下方格的坐标。


## Q：在提交的代码中哪些库是可用的？是否会进行代码查重之类的工作？

理论上不需要系统权限的库都能用，需要注意不能使用windows或darwin平台限定的库。会进行选手间的代码查重。

## Q：运行后端可执行文件时为什么出现闪退无法运行的情况？

可能是因为您没有安装 .NET Core 运行时环境。请根据您的操作系统版本下载并安装 .NET Core 运行时环境，版本至少为 8.0，下载地址为：[https://dotnet.microsoft.com/zh-cn/download/dotnet/thank-you/runtime-8.0.4-windows-x64-installer?cid=getdotnetcore](https://dotnet.microsoft.com/zh-cn/download/dotnet/thank-you/runtime-8.0.4-windows-x64-installer?cid=getdotnetcore)。

您可以用 PowerShell 或 cmd 命令行运行GameServer.exe，以查看具体的错误信息。

## 操作
agent的所有操作都是按请求结算，即agent每发起一次请求都会【立即】结算。一帧内可以进行任意多次操作。

## 移动
agent的move和stop方法实际上是改变其目的地，这两个方法本身是按请求结算。
而玩家位置是按帧更新，具体而言，若目的地与当前距离大于一帧可移动距离，则会以最大速度移动；否则，速度会相应减小（在一帧内移动至目的地后停止）。若碰到墙壁，则以碰撞位置为终点。

## CD
武器的两次攻击之间存在CD。CD是武器的属性，除攻击之外的任何操作都不会对武器的CD产生影响。
其余任何操作都没有CD。

## 护甲
每个agent至多能同时拥有一件护甲。拾取新护甲会自动脱下旧护甲。护甲拾取后自动穿着。
