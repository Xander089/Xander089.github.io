---
layout: page
# title: "Course"
permalink: "/course/gameplay"
---

## Gameplay Framework

### Actors

- Whatever object that can be placed in a level, or spawned at runtime.
- They can be considered as containers of components:
    - UActorComponents, like InputComponent, or any logic component that doesn't need to be visibile in the level
    - USceneComponents: Actor components with Transform relative to the parent, placed hierarchically under the root component
    - UPrimitiveComponents: all the scence components that require to be rendered graphically (like a mesh component).
- Example Gold Actor: Root --> Static Mesh --> Particle System, Audio, Collision Box
- Life Cycle and Ticking: to do
- Spawn & Destroy:
    - Spawn: AActor::GetWorld()->SpawnActor<CustomActorClass::StaticClass()>(FVector Location, FRotator Rotation)
    - Destroy: AActor::Destroy()


### Camera

- A camera actor (from Place Actpr panel) can be used to place a camera in a level:
    - it is a container for a camera component

- In a character:
    - Skeletal Mesh
        - Attach a Spring Arm Component
            - Attach a Camera Component

```c++
	    //Instantiating Class Components
         SpringArmComp = CreateDefaultSubobject<USpringArmComponent>(TEXT("SpringArmComp"));
		
         //Attach the Spring Arm to the Character's Skeletal Mesh Component
         SpringArmComp->SetupAttachment(GetMesh());
		
         //Attach the Camera to the SpringArmComponent
         CameraComp->AttachToComponent(SpringArmComp, FAttachmentTransformRules::KeepRelativeTransform);
		
         //Setting default properties of the SpringArmComp
         SpringArmComp->bUsePawnControlRotation = true;
         SpringArmComp->bEnableCameraLag = true;
         SpringArmComp->TargetArmLength = 300.0f;

```

### Components
- Used to share common behavior across actors, that can attach components as sub objects
- For example, different enemies --> different combat styles --> use different components
- It's like a strategy pattern?
- scene components can use: SetupAttachment(another component) or AttachToComponent(another component)
- primitive components: box component, capsule component, static mesh, skeletal mesh
- sphere, capsule and box all inherits from UShapeComponent and are used for collisions
- movement component: only used for Character and provides methods for walkink, falling, swimming and flying


### Controllers

- They can possess Pawns (and Characters, since they inherit from APawn) to control their actions
- They are an interface between the player inputs and the character
- In character class, setup input binds inputs to character methods
- Pawn.h has a method to return the local player controller:
    - ```APlayerController* APawn::GetLocalViewingPlayerController() const ```
    - the method uses GetWorld() and iterates over all the controller, finally returning the local player if found
    - has also: virtual void SetupPlayerInputComponent(UInputComponent* PlayerInputComponent): Allows a Pawn to set up custom input bindings. Called upon possession by a PlayerController, using the InputComponent created by CreatePlayerInputComponent().
    - So, level starts -> Game Instance -> Player Controller: on pawn possessed calls *pawn->SetupPlayerInputComponent(CreatePlayerInputComponent())

### Game Mode & Game State

1. Game Launched
   |
   V
2. GameInstance is created (only once, persists across level loads)
   |
   V
3. Engine loads the specified GameMode (defined by the map)
   |
   V
4. GameMode is instantiated (only on server in multiplayer)
   |
   V
5. GameMode does:
   - Creates DefaultPawnClass
   - Spawns PlayerController
   - Possesses pawn with PlayerController
   - Spawns HUD
   - Spawns GameState
   |
   V
6. PlayerController is created
   |
   V
7. PlayerController calls SpawnDefaultPawnFor()
   - Spawns Character (or Pawn)
   - Possesses it
   |
   V
8. Character (or Pawn) is initialized
   - BeginPlay() is called
   - Components are initialized
   |
   V
9. PlayerController sets up input bindings (e.g. in SetupInputComponent)
   |
   V
10. Level begins!
    - Tick starts running
    - Player input is processed
    - PlayerController translates input into movement/actions on pawn


### Pawn
- an actor that can be possessed by a player controller
- character inherits from pawn




