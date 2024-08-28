# Stacking

## Stacking Type

Aggregate(总计) by Source
Stack 限值计数 2 : 目标在同一时间只能应用2个 GE 进栈出栈

# GE Component

## Target Tags Gameplay Effect Component

给 TargetActor 授予 Tags

# Tags

|       Gameplay Tag        | 描述                                                                                   |
|:-------------------------:|--------------------------------------------------------------------------------------|
|       Ability Tags        | 这种能力具有这些标记                                                                           |
| Cancel Abilities with Tag | 当执行此能力时，带有这些标记的能力会被取消                                                                |
| Block Abilities with Tag  | 当此异能处于激活状态时，带有这些标记的异能会被阻挡                                                            |
|   Activation Owned Tags   | 标记适用于激活所有者，而这种能力是积极的。这些都是复制的，如果ReplicateActivationOwnedTags 启用在 AbilitySystemGlobals |
| Activation Required Tags  | 只有当激活的 actor/component 具有所有这些标记时，才能激活此功能                                             |
|  Activation Blocked Tags  | 如果激活的 actor/component 具有以下任何标记，则此功能将被阻止                                              |
|   Source Required Tags    | 只有当 源(Source) actor/component 具有所有这些标记时，才能激活此能力                                      |
|    Source Blocked Tags    | 如果 源(Source) actor/component 具有以下任何标记，则此能力将被阻止                                       |
|   Target Required Tags    | 只有当 target actor/component 具有所有这些标记时，才能激活此功能                                         |
|    Target Blocked Tags    | 如果 target actor/component 具有以下任何标记，则此能力将被阻止                                          |

# Instancing Policy(策略)

|          实例化策略          |           描述            |                    详细                    |
|:-----------------------:|:-----------------------:|:----------------------------------------:|
|   Instanced Per Actor   | 为该能力创建一个实例。每次激活时都会重复使用它 |          可以存储持久性数据。每次都必须手动重置变量           |
| Instanced Per Execution |      每次激活时都会创建新实例       |      在激活之间不存储持久性数据。性能低于每个执行组件的实例化性能      |
|      Non-Instanced      |    仅使用类默认对象，不创建任何实例     | 无法存储状态，无法绑定到Ability Tasks上的委托。三个选项中的最佳性能 |

# Net Execution Policy

|  New Execution Policy   |                  描述                  |
|:-----------------------:|:------------------------------------:|
|       Local Only        |         仅在本地客户端上运行。服务器不运行能力          |
|   Local Predicted(预测)   | 在本地客户端上激活，然后在服务器上激活。利用预测。服务器可以回滚无效更改 |
|       Server Only       |               仅在服务器上运行               |
| Server Initiated(服务器启动) |       首先在服务器上运行，然后在拥有的本地客户端上运行       |