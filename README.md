---

# VR Project: README

## **Overview**
This project is a VR shooting game where players must destroy targets in a VR environment. Once all targets are destroyed, the game transitions to a "You Win" level. The key features include reloading mechanics, dynamically updating UI, destructible targets, and a win condition.

---

## **Features Implemented**

### **1. Reloading Mechanic**
- **Functionality**:
  - The pistol has an ammo count displayed in the HUD.
  - The player cannot shoot if the ammo count is `0`.
  - A reload button resets the ammo count after a delay.
  - A notification sound plays when reloading is complete.

- **Key Steps**:
  - A `Widget Component` was added to the Pistol Blueprint and linked to a HUD Widget Blueprint (`CR_HUD`).
  - `AmmoCount` updates dynamically in the widget.
  - Shooting decreases `AmmoCount`. Reloading resets it after a delay.

---

### **2. Targets with Dynamic Destruction**
- **Functionality**:
  - At least 5 targets are placed in the level.
  - Each target keeps track of how many times it has been hit:
    - **First hit**: The target changes its color/material.
    - **Second hit**: The target is destroyed.
  - Destroying a target triggers an event that updates a counter in the Level Blueprint.

- **Key Steps**:
  - A `Target_BP` Blueprint was created with:
    - A `Static Mesh Component` to represent the target.
    - A `HitCount` variable to track hits.
    - A material change for the first hit.
    - Logic to destroy the target on the second hit.
  - An `Event Dispatcher` was added to `Target_BP` to notify the Level Blueprint when a target is destroyed.

---

### **3. Win Condition**
- **Functionality**:
  - The game tracks the number of destroyed targets.
  - When all targets are destroyed, the game transitions to a "You Win" level.

- **Key Steps**:
  - The Level Blueprint:
    - Uses `Get All Actors of Class` to count all `Target_BP` instances at the start of the game.
    - Binds an event to the `On Target Destroyed` dispatcher in `Target_BP`.
    - Increments a counter (`Targets Destroyed Counter`) each time a target is destroyed.
    - Compares the counter to the total number of targets.
    - Loads the "You Win" level when all targets are destroyed.

---

### **4. You Win Level**
- **Functionality**:
  - A new level (`YouWin`) displays a "You Win!" message.

- **Key Steps**:
  - A new level was created and named `YouWin`.
  - A Widget Blueprint (`YouWinWidget`) was created to display "You Win!" text.
  - The `YouWinWidget` was added to the viewport on level load using the **Level Blueprint** of the `YouWin` level.

---

## **Blueprint Logic**

### **Level Blueprint (Main Level)**
1. **Event Begin Play**:
   - Uses `Get All Actors of Class` to retrieve all `Target_BP` instances.
   - Binds the `On Target Destroyed` event for each target to a custom event (`OnTargetDestroyedHandler`).
   - Stores the total number of targets in a `TotalTargets` variable.

2. **On Target Destroyed Handler**:
   - Increments the `Targets Destroyed Counter`.
   - Checks if `Targets Destroyed Counter` equals `TotalTargets`.
   - If true, loads the "You Win" level using the `Open Level` node.

### **Target_BP**
1. Tracks hits via a `HitCount` variable:
   - **First hit**: Changes the material to `HitMaterial`.
   - **Second hit**: Calls the `On Target Destroyed` dispatcher and destroys the actor.

2. The `On Target Destroyed` dispatcher notifies the Level Blueprint when the target is destroyed.

---

## **Testing**

### **Reloading Mechanic**:
1. Shoot until `AmmoCount` reaches `0` and confirm shooting is disabled.
2. Press the reload button and verify:
   - A delay occurs before ammo resets.
   - The UI updates to display the new `AmmoCount`.

### **Target Destruction**:
1. Shoot targets and confirm:
   - Material changes after the first hit.
   - Targets are destroyed on the second hit.
   - The destroyed counter in the Level Blueprint increments.

### **Win Condition**:
1. Destroy all targets and confirm:
   - The game transitions to the `YouWin` level.

### **You Win Level**:
1. Load the `YouWin` level and verify:
   - The "You Win!" message is displayed.

---

## **Known Issues and Troubleshooting**
- **Targets Not Destroying**:
  - Verify the `HitCount` logic in `Target_BP`.
  - Ensure the `Destroy Actor` node is triggered after the dispatcher.

- **Win Level Not Loading**:
  - Confirm the `Targets Destroyed Counter` equals `TotalTargets`.
  - Ensure the `On Target Destroyed` dispatcher is called in `Target_BP`.

- **Widget Not Updating**:
  - Verify the `Widget Component` in the Pistol Blueprint is linked to the `CR_HUD` widget.

---

## **Future Enhancements**
- Add animations or particle effects for targets being destroyed.
- Include a restart button in the `YouWinWidget` to reset the game.
- Add audio feedback for reloading, shooting, and destroying targets.

---

