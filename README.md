## **Engine version:** 5.7.4

## How to Run the Game
1. Download the packaged game ('.zip' file) from the GitHub Releases section.
2. Unzip the archive into a new folder on your computer.
3. Open the extracted folder and double-click 'CollectAndAvoid.exe' to launch the game.

## Controls:
**WASD/Arrow Keys:** Player Movement

**Mouse:** Camera Control/Look Around

**Left Shift:** Sprint

**Spacebar:** Jump

**Escape:** Pause Game

## Blueprint Overview & Responsibilities

**BP_Hero:** Player character containing movement logic, camera controls, sprint mechanic and input listeners for the Pause menu.

**BP_Coin:** Collectible item with a rotating component. Detects overlap with the player, plays a collection sound, spawns Niagara burst VFX, and triggers the score event dispatcher.

**BP_GameMode:** Manages central game states ('Playing', 'Won', 'Lost') and tracks score progression using 'CurrentScore' and 'TargetScore' variables.

**BP_Enemy & BP_EnemyAIController:** Enemy AI logic using Behavior Tree & Blackboard. Features AI Perception for player line-of-sight detection and dynamically toggles chase music.

**WBP_HUD:** On-screen User Widget displaying current/target scores and control hints without per-frame bindings.

**WBP_Pause/WBP_Win/WBP_Lose:** Modal UI screens for pausing, winning and losing states with interactive buttons and UI input mode handling.



## 1. Camera Adjustments:



**Target Arm Length:** 600

**Socket Offset:** Y = 50, Z = 100



## 2\. Character Movement Adjustments:



**Max Walk Speed:** 200

This sets the character's default walking speed. It's intentionally set low so that when the custom 'Sprint' action is activated, the speed dynamically increases to 800.

**Jump Z Velocity:** 700

This determines the initial upward speed of the jump, controlling how high the character can jump.

**Air Control:** 0.8

This controls how much the player can steer or change the character's direction while in the air.

**Jump Max Count:** 2



## 3\. Enhanced Input System \& Custom Actions:



Added 'Mouse XY 2D-Axis' to 'IA\_Look' to enable camera control. Adjusted the 'Negate' modifiers to ensure standard mouse direction (non-inverted).
Added the custom action 'IA\_Sprint' to the Left Shift key.

**IA\_Sprint:** Created a new Input Action specifically for the sprint mechanic.



## 4\. Collectibles \& Score System:

Created a collectible coin combining a Static Mesh and a Sphere Collision component.

Added a 'RotationMovement' component to handle the coin's rotation.

Used 'OnComponentBeginOverlap' to detect interactions.

**Why the cast check is needed?** - A 'Cast to BP\_Hero' is needed to verify that the overlapping actor is actually the player character. Without this cast, any entity (for example, Enemy) entering the sphere would trigger the collection logic.



## 5\. Game Mode \& Score Logic:

Three variables were created in 'BP\_GameMode' to track the gameplay logic:

\- 'CurrentScore' (Integer);

\- 'TargetScore' (Integer): Set to a default of 8 and exposed to be editable;

\- 'GameScore\_1' (E\_GameState): Tracks the flow of the game.



**Enumeration ('E\_GameState'):** Created with three states - 'Playing', 'Won' and 'Lost'.

**Event Dispatcher Binding:** To separate the coin from the UI and logic, 'BP\_Coin' calls an 'OnCoinCollected' Event Dispatcher. In 'BP\_GameMode's' 'BeginPlay', a 'Get All Actors Of Class' loop binds each coin's dispatcher to a custom 'AddScore' event.

**Win Condition:** When a coin is collected, the 'AddScore' event increase a current score and uses a 'Branch' to check if 'CurrentScore>=TargetScore'. If true, the 'GameState\_1' switches to 'Won'.

## 6\. Enemy: Patrol & Chase:

Created BP_Enemy and BP_EnemyAIConroller. Implemented Behavior Tree with Blackboard. Used AI Perception for sigh. Enemy loses target when player is out of the enemy's sight. Enemy Speed is exposed. Sight Radius is also exposed as an editable variable in BP_Enemy, but due to Blueprint limitations of AI Perception component, the actual functioning sight radius is configured directly inside the AI Perception Config in the AI Controller.

## 7\. UI Optimization

The HUD score updates using Events instead of UMG Property Bindings.

**Why:** Bindings force the game to check and update the score on every single frame. This wastes processing power, because the score only changes when a coin is picked up.

**What I did:** The score updates only when the player actually collects a coin. This keeps the project optimized.

## 8\. Assets & Licenses

All third-party assets used in this project are free:

**Background Music:** "Game Music Loop 7" by XtremeFreddy from Pixabay (Pixabay License - Free for commercial use).

**Chase Music:** "Warning!!" by VABsounds (Freesound) from Pixabay (Pixabay License - Free for commercial use).

**Coin SFX:** "coin recieved" by RibhavAgrawal from Pixabay (Pixabay License - Free for commercial use).

## 9\. UI & Menus:

**HUD:** Displays the current score, target score and a brief control hint. Created on 'BeginPlay' and added to the viewport.

**Pause Menu:** Toggled using the 'Escape' key. It pauses the game time, switches the input mode to 'UI Only' and shows the mouse cursor. A 'Continue' button continues the game.

**Win/Lose Screen:** When the game ends, a respective widget is created. It pauses the game and provides a 'Restart' button that reopens the current level.

## 10\. Visual Feedback & Effects:

To enhance game feel and ensure the player always knows what is happening, feedback was to two key actions:

**Coin Collection:** A Niagara system burst spawns at the coin's location when collected.

**Player Defeat:** A 'LegacyCameraShake' is triggered when the enemy catches the player. A short 0.5s delay allows the camera to shake before the game pauses and the Game Over screen appears.

## 11\. What I learned and what was hard:

I learned how to create a third-person game, work with Blueprints and make UI widgets. The most difficult part was figuring out how Blueprint logic works and finding the correct nodes and components. Since this is my very first time using Unreal Engine, it was pretty hard to navigate the editor without clear, step-by-step tutorials.
