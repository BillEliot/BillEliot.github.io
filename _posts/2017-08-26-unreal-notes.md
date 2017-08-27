---
layout: post
title: "Unreal杂烩总结"
date: 2017-08-26
tags: [Game, Unreal]
categories: [Game]
bgp: "bg_text_9"
---

## 随写随记, 易错点, 注意事项, 重要笔记

---

**写多人游戏(联机)的时候, 要在 `DefaultEngine.ini` 中定义 :**  

> LAN

```
[OnlineSubsystem]
DefaultPlatformService=Null
```

> Steam

```
[/Script/Engine.GameEngine]
+NetDriverDefinitions=(DefName="GameNetDriver",DriverClassName="OnlineSubsystemSteam.SteamNetDriver",DriverClassNameFallback="OnlineSubsystemUtils.IpNetDriver")

[OnlineSubsystem]
DefaultPlatformService=Steam

[OnlineSubsystemSteam]
bEnabled=true
SteamDevAppId=480
bVACEnabled=0

[/Script/OnlineSubsystemSteam.SteamNetDriver]
NetConnectionClassName="OnlineSubsystemSteam.SteamNetConnection"
```
[更多参考Unreal官方文档](https://docs.unrealengine.com/latest/CHN/Programming/Online/Steam/index.html)

---

**设置第三人称Character Blueprint的几点注意**
1.添加Spring Arm Component后Attach摄像机, 并勾选Use Pawn Control Rotation   
2.取消勾选Pawn下的Use Controller Rotation Yaw   
3.勾选CharacterMovementComponent中Rotation Settings下的Orient Rotation to Movement   

---

* CreateSession 写在GameInstance蓝图中才会有效果
* 设置GameMode后, PlayerStart可能不会加载默认的Character, 重启编辑器就好了
