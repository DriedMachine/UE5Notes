# debug 指令

 ```
showdebug abilitysystem
 ```

# const_cast

```C++
UAuraAttributeSet* MutableAuraAttributeSet = const_cast<UAuraAttributeSet*>(AuraAttributeSet);
```

# PlayerController

PlayerController：自己为主机的时候，也就是Server， 自己的PlayerController是有效的，其他玩家的PlayerController可能为空，不用
用check来
检查PlayerController是否为null，只用if 判断。

## Init Ability Actor Info

AbilitySystemComponent是由Enemy Character创建的
AbilitySystemComponent是有PlayerState创建的

|            Class            | Owner Actor | Avatar Actor |
|:---------------------------:|:-----------:|:------------:|
|       Enemy Character       |    Pawn     |     Pawn     |
| Player Controlled Character | PlayerState |     Pawn     |

| ReplicationMode | Use Case  |                             描述                             |
|:---------------:|:---------:|:----------------------------------------------------------:|
|      Full       |    单人     |                  Gameplay Effects复制到所有客户端                  |
|      Mixed      | 多人游戏，玩家控制 | Gameplay Effects只复制到拥有的客户端。Game Cues和Gameplay Tags复制到所有客户端 |
|     Minimal     | 多人游戏，AI控制 |    Gameplay Effects不会只复制到拥有的客户端。Game Cues和游戏标签复制到所有客户端     |

### Player Controlled Character

ASC Lives on the Pawn

|        | Class |         函数位置          | OwnerActor | AvatarActor |
|:------:|:-----:|:---------------------:|:----------:|:-----------:|
| Server | Pawn  |      PossessedBy      |    Pawn    |    Pawn     |
| Client |       | AcknowledgePossession |    Pawn    |    Pawn     |

ASC Live on the PlayerState

|        | Class |       函数位置        | OwnerActor  | AvatarActor |
|:------:|:-----:|:-----------------:|:-----------:|:-----------:|
| Server | Pawn  |    PossessedBy    | PlayerState |    Pawn     |
| Client |       | OnRep_PlayerState | PlayerState |    Pawn     |

### AI Controlled Character

ASC Lives on the Pawn

|        | Class |   函数位置    | OwnerActor | AvatarActor |
|:------:|:-----:|:---------:|:----------:|:-----------:|
| Server | Pawn  | BeginPlay |    Pawn    |    Pawn     |
| Client | Pawn  | BeginPlay |    Pawn    |    Pawn     |

|        Server        |        Client        |
|:--------------------:|:--------------------:|
|       GameMode       |                      |
|     PlayerState      |     PlayerState      |
|         Pawn         |         Pawn         |
|                      |     Own Widgets      |
|                      |       Own HUD        |
| All PlayerController | Own PlayerController |

GE
Coefficient * (Modifier + Pre) + Post

# Tags

[/Script/Engine.Engine]
AssetManagerClassName = /Script/Aura.AuraAssetManager
原生 Gameplay 标签的单例

# 杂项

+ ThisClass.Get() 返回ThisClass的对象引用