Unreal Engine Project: First Person Shooter Mechanics
This project implements a series of gameplay mechanics for a first-person shooter (FPS) game in Unreal Engine 5. These include player health, ammo and health pickups, shooting, target destruction, and level progression. Each section below describes the functionality of the various blueprints used in this project.

Table of Contents
Player Health System
HUD Setup
Target Blueprint
Health Pickup Blueprint
Ammo Pickup Blueprint
Gun Firing Mechanic
Level Progression
Player Health System
This blueprint manages the player's health, including damage processing, UI updates, and level restart upon death.

Event AnyDamage: Triggered whenever the player takes damage.
Damage Processing: Adjusts CurrentHealth based on incoming damage and clamps it to ensure it stays within a valid range (between 0 and MaxHealth).
Update Health UI: Calls a function to refresh the health display on the HUD.
Branch Node: Checks if CurrentHealth has dropped to zero. If so, it initiates a level restart by:
Executing Console Command: Restarts the level upon death.
Print String (Debugging): Logs "You Died Restarting Level" for development purposes.
HUD Setup
This blueprint initializes and displays the player's HUD at the start of the game.

Event BeginPlay: Called when the game begins.
Create HUD Widget: Creates the health widget and stores a reference to it.
Add to Viewport: Displays the widget in the player’s viewport.
Initialize Health: Sets the initial values of CurrentHealth and MaxHealth and updates the HUD.
Target Blueprint
This blueprint defines the behavior of a destructible target.

Event Hit: Triggered when the target is struck (e.g., by a projectile).
Health Reduction: Reduces CurrentHealth by a specified amount (e.g., 50).
Branch Check: Destroys the target if CurrentHealth falls to zero.
Print String (Debugging): Logs "Destroyed!" to indicate target destruction.
Game Mode Interaction: Updates the target count in the game mode to track remaining targets, helping to manage progression or win conditions.
Health Pickup Blueprint
This blueprint provides a health pickup that restores the player's health upon collection.

On Component Begin Overlap: Detects when the player collides with the health pickup.
Cast to BP_FirstPersonCharacter: Ensures only the player can collect the health pickup.
Set Health: Restores the player’s health to a predefined amount (e.g., 100).
Update Health UI: Refreshes the HUD to reflect the new health value.
Destroy Actor: Removes the pickup from the game after use, preventing reuse.
Ammo Pickup Blueprint
This blueprint defines an ammo pickup, allowing the player to collect additional ammunition.

Event ActorBeginOverlap: Detects when the player collides with the ammo pickup.
Cast to BP_FirstPersonCharacter: Confirms that the overlapping actor is the player.
Add Ammo: Increases the player’s ammo by a specified amount (e.g., 10).
Set Ammo: Updates the ammo count in the player’s variables.
Destroy Actor: Removes the pickup after collection to prevent further use.
Print String (Debugging): Logs "You Found 10 Bullets" to confirm ammo pickup during testing.
Gun Firing Mechanic
This blueprint implements the shooting functionality, allowing the player to fire projectiles.

Input Action (IA_Shoot): Triggered when the player presses the shoot button.
Ammo Check: Ensures the player has enough ammo. If not, displays "No Ammo" and halts firing.
Projectile Spawn:
Socket Location and Camera Rotation: Retrieves the weapon’s socket position and aligns it with the player’s view.
Make Transform: Creates a transform for spawning the projectile.
Spawn Actor (Projectile): Spawns the projectile, simulating a gunshot.
Sound Effect: Plays a firing sound at the spawn location.
Ammo Decrement: Reduces ammo by 1 after each shot.
Firing Animation: Plays an animation montage to provide visual feedback for the shooting action.
Level Progression
This blueprint controls level progression by checking if all targets have been destroyed before loading the next level.

Check All Targets Destroyed: Custom event that verifies whether all targets in the level have been eliminated.
Target Counter Check: Compares the Target Counter to zero.
Branch: If the counter is zero, it initiates level progression.
Open Level: Loads the next level (e.g., "Level2") upon successful target clearance.
Additional Notes
Debugging: Various "Print String" nodes have been used throughout the blueprints for testing and debugging purposes. These can be disabled or removed in the final build.
Customization: The values for health restoration, ammo addition, and target count can be modified to suit specific gameplay needs.
