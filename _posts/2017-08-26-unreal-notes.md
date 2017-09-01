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
* 需要碰撞的Actor蓝图里, 根组件不要设置为SceneComponent, 可以设置为某个Collision(SphereCollision, BoxCollision)
* 联机中, 要获取联机的Controller, 可以尝试将Get Player Controller换成Get Controller
* Lighting Build failed. Swarm Failed to kick off. 光照构建失败, 可能是Mesh或者Material存在重名(极大可能是由另外一个Project导出资源到当前Project造成的)
