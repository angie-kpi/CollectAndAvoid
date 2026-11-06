\*\*Engine version:\*\* 5.7.4

\*\*Quality Preset:\*\* Scalable



1. Camera Adjustments:



\*\*Target Arm Length:\*\* 600
\*\*Socket Offset:\*\* Y = 50, Z = 100



2\. Character Movement Adjustments:



\*\*Max Walk Speed:\*\* 200

\*\*Jump Z Velocity:\*\* 700

\*\*Air Control:\*\* 0.8

\*\*Jump Max Count:\*\* 2



3\. Enhanced Input System \& Custom Actions:



Added 'Mouse XY 2D-Axis' to 'IA\_Look' to enable camera control. Adjusted the 'Negate' modifiers to ensure standard mouse direction (non-inverted).
Added the custom action 'IA\_Sprint' to the Left Shift key.

\*\*IA\_Sprint:\*\* Created a new Input Action specifically for the sprint mechanic.



4\. Collectibles \& Score System:

Created a collectible coin combining a Static Mesh and a Sphere Collision component.

Added a 'RotationMovement' component to handle the coin's rotation.

Used 'OnComponentBeginOverlap' to detect interactions.

\*\*Why the cast check is needed?\*\* - A 'Cast to BP\_Hero' is needed to verify that the overlapping actor is actually the player character. Without this cast, any entity (for example, Enemy) entering the sphere would trigger the collection logic.



5\. Game Mode \& Score Logic:

Three variables were created in 'BP\_GameMode' to track the gameplay logic:
- 'CurrentScore' (Integer);

\- 'TargetScore' (Integer): Set to a default of 8 and exposed to be editable;

\- 'GameScore\_1' (E\_GameSet): Tracks the flow of the game.



\*\*Enumeration ('E\_GameSet'):\*\* Created with three states - 'Playing', 'Won' and 'Lost'.

\*\*Event Dispatcher Binding:\*\* To separate the coin from the UI and logic, 'BP\_Coin' calls an 'OnCoinCollected' Event Dispatcher. In 'BP\_GameMode's' 'BeginPlay', a 'Get All Actors Of Class' loop binds each coin's dispatcher to a custom 'AddScore' event.

\*\*Win Condition:\*\* When a coin is collected, the 'AddScore' event increase a current score and uses a 'Branch' to check if 'CurrentScore>=TargetScore'. If true, the 'GameState\_1' switches to 'Won'.



