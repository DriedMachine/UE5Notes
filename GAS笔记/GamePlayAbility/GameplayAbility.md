# Gameplay Tag

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
|    Target Blocked Tags    | 如果 targetS actor/component 具有以下任何标记，则此能力将被阻止                                         |

# Prediction

## GAS Prediction

GAS Automatically Predicts:

+ GameplayAbility Activation
+ Triggered Events
+ Gameplay Effect Application
    + Attribute Modifiers(not Execution Calculations)
    + GameplayTag Modification
+ Gameplay Cue Events
    + From within a predicted Gameplay Ability
    + Their own Events
+ Montages
+ Movement(UCharacterMovement)

## Ability Activation

TryActivateAbility:

+ Client calls TryActivateAbility
    + New FPreDictionKey "Activation Prediction Key"
+ Client continues
    + Calls ActivateAbility Activation Info
+ Client does things
    + Generates side effects
+ ServerTryActivateAbility
    + Server decides if valid
    + calls ClientActivateAbilityFailed
    + or ClientActivateAbilitySucceeded
+ Client receives the Server's response
    + If failure, kill the ability and undo side effects
    + If success, side effects are valid
+ ReplicatePredictionKey replicates
    + OnRep_PredictionKey

## Gameplay Effect Prediction

Gameplay Effects

+ Side effects
+ Only applied on Client if:
    + There is a valid prediction key
+ The following are predicted:
    + Attribute Modifications
    + Gameplay Tag Modifications
    + Gameplay Cues
+ When the FActiveGameplayEffect is created
    + Store the Prediction Key
+ On the Server, it gets the same key
+ FActiveGameplayEffect is replicated
    + Client checks the key
    + If they match, then "OnApplied" logic doesn't need to be done

## Execution Calculation

Execution Calculation
Snapshotting(Source)

+ Snapshotting captures the Attribute value when the Gameplay Effect Spec is Created
+ Not snapshotting capture the Attribute value when the gameplay Effect is applied
  优点
+ Capture Attributes
+ Can change more than one Attribute
+ Can have programmer logic
  缺点
+ No prediction
+ only instant or Periodic Gameplay Effects
+ Capturing doesn't run PreAttributeChange; any Clamping done there must be done again
+ Only executed on the Server from Gameplay Abilities with Local Predicted, Server Initiated, and Server only Net
  Execution Policies