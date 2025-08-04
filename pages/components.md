---
layout: page
title: "Actor Components"
permalink: "/course/components"
---


## Actor Components

svelato l'arcano:

https://chatgpt.com/share/6883d292-a6a8-8003-b6a3-bb99a3989407

### What are they
- They are the equivalent of sub objects in unity
- But you create them as sub component of the root component in a character BP
- alternatively you can create them from scratch by extending the UActorComponent declared in Components/ActorComponent.h

### How they interact with the parent --> nel begin play
- NBBBBB: Actor.h ha lui DELEGATE, ecco perché è definito qui, per rispettare quello in Actor
- using delegates, for example:
```c++

    //1. definito fuori la classe
    DECLARE_DYNAMIC_MULTICAST_DELEGATE_SixParams(
        FOnHealthChangedSignature, // Nome custom del delegate
        UHealthComponent*, HealthComponent, //puntatore a this component
        float, Health, //param
        float, DamageAmount,  //param
        const class UDamageType*, DamageType, //puntatore a damage type
        class AController*, InstigatedBy, 
        AActor*, DamageCauser);

       //2.nella classe custom component.h
        public:
        //notare bp assignable --> posso passare i param da BP
        UPROPERTY(BlueprintAssignable, Category = "Events")
        FOnHealthChangedSignature OnHealthChanged;

        UPROPERTY(EditDefaultsOnly, BlueprintReadWrite)
        float Health;

        UPROPERTY(EditDefaultsOnly, BlueprintReadWrite)
        float MaxHealth;

        //nella definizione chiamerà FOnHealthChangedSignature OnHealthChanged.broadcast()
        UFUNCTION(BlueprintCallable)
        void HandleTakeDamage(
            AActor* DamageActor,
            float Damage, 
            const class UDamageType* DamageType,
            class AController* InstigatedBy,
            AActor* DamageCauser
            );

        //3. nel custom component.cpp, begin play
        
        //il parent actor a cui il component è attached
        AActor* MyOwner = GetOwner();

        if(MyOwner)
        {
            //NBBBBB: Actor.h ha lui DELEGATE, ecco perché è definito qui, per rispettare quello in Actor
            MyOwner->OnTakeAnyDamage.AddDynamic(this,&UHealthComponent::HandleTakeDamage);
        }

		
        void UHealthComponent::HandleTakeDamage(
             AActor* DamageActor,
             float Damage, 
             const class UDamageType* DamageType, 
             class AController* InstigatedBy, 
             AActor* DamageCauser
            )
		{
            //logica interna al component
			if (Damage <= 0.0f)
			{
				//Damage amount was 0 or less.
				return;
			}
			Health = FMath::Clamp(Health - Damage, 0.0f, MaxHealth); 
            //passo i parametri aggiornati al DELEGATE
            OnHealthChanged.Broadcast(this, Health, Damage, DamageType, InstigatedBy, DamageCauser);
		}

```

### So the parent, what is it responsible for?
- create and attach the component in the constructor
```

```
