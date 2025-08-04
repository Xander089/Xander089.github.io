---
layout: page
title: "Adding Abilities"
permalink: "/course/abilities"
---


## I learned Gameplay Ability System so you don't have to

### 1 Intro
GAS is a built in unreal engine framework to handle abilities in games like RPGs and MOBAs.

The framework is very complicated and requires you to work with both Blueprints and C++ classes.

This tutorial will guide you to the GAS, by creating a simple ability in our sandbox project:

(showing the final result is a good thing)

- The unreal engine third person template
- attach a niagara particle effect to the character whenever she or he moves
- use attribute: is running to trigger the ability only when the char speed is above a threshold
- apply a gameplay effect by reducing the character stamina level when the ability is active

### 2 How GAS framework actually works

- character or any actor has an ability system component attached to the root component
    - manually need to declare define in char class
- character also has an optional attribute set pointer, attached to the a.s.c in the constructor
- in char begin play: RunningAbilityHandle = AbilitySystem->GiveAbility(FGameplayAbilitySpec(RunningAbilityClass));
    - RunningAbilityHandle reference to an ability class that i want to store in the a.s. component
- in a whatever char method, use the method AbilitySystem->TryActivateAbility(RunningAbilityHandle); to activate the ability
- an attribute set is a class storing a reference to float structs: the attributes
- the float struct is FGameplayAttributesData and has a base & current value (like MaxHealth & current Health)
- a gameplay effect is a class that is responsible for modifying attributes. In attributeSet class:     void PostGameplayEffectExecute(const struct FGameplayEffectModCallbackData& Data) override;
    - [Attributes & Attribute Sets](https://dev.epicgames.com/documentation/en-us/unreal-engine/gameplay-attributes-and-attribute-sets-for-the-gameplay-ability-system-in-unreal-engine)
    - NB!!! effects can only be built as BP:
        - here can they modify attributes?
        - in AttributeSet class c++, we can use callback to be executed whenever an effect modify an attribute, such as void PostGameplayEffectExecute(const struct FGameplayEffectModCallbackData& Data) override;


### 3 Step by step C++ changes

- ProjectCharacter class

```c++
//.h
#include "AbilitySystemInterface.h"
#include "GameplayAbilitySpec.h"

public IAbilitySystemInterface

public:

	virtual UAbilitySystemComponent* GetAbilitySystemComponent() const override;

	UPROPERTY(EditAnywhere, BlueprintReadOnly, Category = "Abilities")
	TSubclassOf<class UGameplayAbility> RunningAbilityClass;

	FGameplayAbilitySpecHandle RunningAbilityHandle;

protected:

	UPROPERTY(VisibleAnywhere, BlueprintReadOnly)
    UAbilitySystemComponent* AbilitySystem;

    UPROPERTY()
    UMyAttributeSet* AttributeSet;

//.cpp

//nel costruttore, creare components e attaccarli al root component (lo fa di default):
AbilitySystem = CreateDefaultSubobject<UAbilitySystemComponent>(TEXT("AbilitySystem"));
AttributeSet = CreateDefaultSubobject<UMyAttributeSet>(TEXT("AttributeSet"));

//override dell'interfaccia
UAbilitySystemComponent *AMyProjectCharacter::GetAbilitySystemComponent() const
{
    return AbilitySystem;
}

//nel begin play
	if (AbilitySystem && RunningAbilityClass)
    {
        RunningAbilityHandle = AbilitySystem->GiveAbility(FGameplayAbilitySpec(RunningAbilityClass));
    }

//nel metodo doMove:
			if (AbilitySystem && RunningAbilityHandle.IsValid() && FMath::Abs(Forward) > 0.0f)
			{
				AbilitySystem->TryActivateAbility(RunningAbilityHandle);
			}
			else if (AbilitySystem && RunningAbilityHandle.IsValid() && FMath::Abs(Forward) == 0.0f){
				AbilitySystem->CancelAllAbilities();
			}

    // initialized its base and current value to 100.0
    UPROPERTY(EditAnywhere, BlueprintReadWrite, Category = "Attributes")
	FGameplayAttributeData Stamina;

```

### 4 Step by step BP changes

- creata GA bp
    - spawn particle system at actor location
    - successivamente, applica GE effect per diminuire la stamina attribute appartenente a attr. set
- creata GE bp
    - non ha nulla nel graph editor
    - ha un modifier associato a un attribute, che diminuisce di 5 la stamina quando applicato

### 5 Final result

- ho un character
- gli attacco ab sy comp e attr set
- nell'attr set definisco struct attribute stamina
- implemento interfaccia gameplay ability system
- inizializzo nel costruttore ab sy co and attr set
- quando mi muovo: ab sy co -> try activate ability
- definita bP ability:
    - nel graph: spawno particle system
    - nel graph: applico gameplay effect
- il gameplay effect:
    - è una BP
    - non ha graph
    - ha una serie di properties
    - tra cui una collection modifier: qui scelgo quale attributo modificare e come

- to do:
    - in attr set, inserire virtual function override post game play effect
    - i tag




Done:

- Inclusione di AbilitySystemInterface.h e GameplayAbilitySpec.h.
- Dichiarazione di un puntatore a UAbilitySystemComponent (AbilitySystem) e a UMyAttributeSet (AttributeSet).
- Aggiunta della proprietà RunningAbilityClass (TSubclassOf<class UGameplayAbility>) per assegnare una abilità da Blueprint.
- Aggiunta della variabile FGameplayAbilitySpecHandle RunningAbilityHandle per tenere traccia dell’abilità assegnata. (è una struct quindi no forward decl)
- Override di GetAbilitySystemComponent() e BeginPlay().

- creata una gameplay ability completamente in BP
    - nel metodo on ability activate: spawno niagara emitter at char location
    - nel char->DoMove, se forward speed = 0; cancelAllAbilities

To do:

- mi muovo
- se premo "q":
    - corro -> modifico la walking speed done
    - inoltre attivo l'abilità done
    - l'attività attiva il gameplay effect done
    - il gameplay effect diminuisce la stamina done
    - mettere post game effect in attribute set per clampare la stamina
    - cool down: pensare
    - tick ability in ability per diminuire la stamina in modo continuo applicando là l'effetto

- attributi?
- gameplay effects?
- Tags?

---

### [Ability System Component](https://dev.epicgames.com/documentation/en-us/unreal-engine/gameplay-ability-system-component-and-gameplay-attributes-in-unreal-engine)
Core component an Actor uses to access Gameplay Ability System features and functionality.

### [Gameplay Abilities](https://dev.epicgames.com/documentation/en-us/unreal-engine/using-gameplay-abilities-in-unreal-engine)
Active or passive abilities that can include gameplay logic, visual effects, animations, and sounds, implemented in C++ or Blueprints.

### [Attributes & Attribute Sets](https://dev.epicgames.com/documentation/en-us/unreal-engine/gameplay-attributes-and-attribute-sets-for-the-gameplay-ability-system-in-unreal-engine)
These store and manage values like health, stats, and resources that can be modified during gameplay.

### [Gameplay Effects](https://dev.epicgames.com/documentation/en-us/unreal-engine/gameplay-effects-for-the-gameplay-ability-system-in-unreal-engine)
Mechanisms for modifying attributes, such as applying buffs, debuffs, or direct value changes, with customizable behavior.

### [Ability Tasks (UAbilityTask)](https://dev.epicgames.com/documentation/en-us/unreal-engine/gameplay-ability-tasks-in-unreal-engine)
Specialized asynchronous tasks executed during Gameplay Abilities, allowing custom events and complex gameplay flows.

