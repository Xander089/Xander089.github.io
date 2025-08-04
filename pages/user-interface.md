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
