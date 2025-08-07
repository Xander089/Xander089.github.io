---
layout: page
title: "Menu"
permalink: "/course/user-interface"
---

## Inventory
- creata struct inventory item
- creato inventory actor component
- aggiunto come component al character
- ha funzioni crud per aggiungere rimuovere leggere items.

## UI
- creato unniform grid per l'inventario
- il singolo slot è nella classe InentorySlotWidget
- la uniform grid ha un native construct che:
    - legge items dal component inventario
    - popola n slot quanti sono gli item
    - tuttavia sto caricando il game async --> potrebbe non servire più

- MySave Game ha TArray<FInventoryItem> InventoryItems;
- il character salva il game in asincrono nell'EndPlay salvando gli item
- il game mode ha un evento bp:

```c++
	UFUNCTION(BlueprintImplementableEvent, Category = "GameMode")
	void OnGameInitialized();
```
che viene chiamato dal inventory component con una callback quando finisce nel begin play il load async

-----------------

### Usare un HUD

- Crea un custom game mode ASurvivalGameMode
- Crea un custom game state ASurvivalGameState
- Character:
    - UPlayerStatsComponent* (float health\max health, void TakeDamage(float damage), void RestoreHealth(float amount))
    - UInventoryComponent*: 
        - struct Ivnentory Slot (FString ItemName, int32 Quantity)
        - struct FItemData: public FTableRowBase(string name, int32 quantity, healAmount, UTexture2d* Icon, MaxStack int32)
        - InventoryComponent : public UActorComponent
            - TArray<FInventorySlot> InventorySlots;
            - int32 MaxSlots = 20;
            - bool AddItem(const FString& ItemName, int32 Quantity = 1); UFUNCTION(BlueprintCallable)
            - bool ConsumeItem(const FString& ItemName, int32 Quantity = 1); UFUNCTION(BlueprintCallable)
            - bool HasItem(const FString& ItemName, int32 MinQuantity = 1); UFUNCTION(BlueprintCallable)
    - Combat System*: UCombatComponent : public UActorComponent
        - float AttackDamage = 25.0f; UPROPERTY(EditAnywhere, BlueprintReadWrite)
        - float AttackRange = 150.0f; UPROPERTY(EditAnywhere, BlueprintReadWrite)
        - float AttackCooldown = 1.0f; UPROPERTY(EditAnywhere, BlueprintReadWrite)
        - void PerformAttack(); UFUNCTION(BlueprintCallable)
        - bool CanAttack() const; UFUNCTION(BlueprintCallable)
        - float LastAttackTime = 0.0f; //private:
    - Multiple attacks:
        - Enum Atk Types
        - Input Combination -> trigger attack type
        - Attack() => Try Hit + Animation
            if Hit => OnTakeDamage

- Gatherable Item : public AActor
    - UStaticMeshComponent* MeshComponent; UPROPERTY(VisibleAnywhere, BlueprintReadOnly)
    - USphereComponent* InteractionSphere;UPROPERTY(VisibleAnywhere, BlueprintReadOnly)
    - FString ItemName = "Food";UPROPERTY(EditAnywhere, BlueprintReadWrite)
    - int32 ItemQuantity = 1;UPROPERTY(EditAnywhere, BlueprintReadWrite)
    - void OnGathered();UFUNCTION(BlueprintImplementableEvent)