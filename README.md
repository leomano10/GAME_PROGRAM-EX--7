# GAME_PROGRAM-EX--7

### NAME    : MANORAJAPRIYAN L. E.

### REG. NO.: 212225040227

---

# EXP: 7

## AI Random Roam with Chase – Unreal Engine

## Aim

To create an AI character in Unreal Engine that roams randomly within a **NavMesh** area and chases the player when they come within a certain range, using **Behavior Trees, Blackboard, and AI Perception**.

---

## Procedure

### 1. Setup Navigation

* Add a `NavMeshBoundsVolume` to the level.
* Scale it to cover the entire area where the AI needs to roam.
* Press **P** to visualize the navigation area.
* The navigable area should appear in **green**.

---

### 2. Create AI Character

* Create a Blueprint Character named:

`BP_AIEnemy`

* Assign a suitable **Skeletal Mesh** to the character.
* Set the appropriate **AI Controller Class**.

Create an AI Controller Blueprint named:

`BP_AIController`

* Assign `BP_AIController` to the AI character.

---

### 3. Enable AI Perception

In `BP_AIController`:

* Add an **AI Perception** component.
* Add a **Sight** sense.
* Configure the following properties:

  * Detection Range
  * Lose Sight Range
  * Peripheral Vision Angle

Use the `OnPerceptionUpdated` event to detect the player and update the Blackboard values:

* `CanSeePlayer`
* `PlayerActor`

---

### 4. Set Up Blackboard

Create a Blackboard named:

`BB_AI`

Add the following keys:

| Key              | Type    | Purpose                                  |
| ---------------- | ------- | ---------------------------------------- |
| `TargetLocation` | Vector  | Stores the random destination            |
| `PlayerActor`    | Object  | Stores the detected player               |
| `CanSeePlayer`   | Boolean | Determines whether the player is visible |

---

### 5. Create Behavior Tree

Create a Behavior Tree named:

`BT_AI`

Use the following structure:

```text id="7zq2k1"
Root
└── Selector
    ├── Sequence (Chase Player)
    │   ├── Blackboard Check: CanSeePlayer == true
    │   └── Move To: PlayerActor
    │
    └── Sequence (Random Roam)
        ├── Task: Find Random Location → TargetLocation
        └── Move To: TargetLocation
```

The **Selector** gives priority to the Chase Player sequence when the player is detected. Otherwise, the AI continues with the Random Roam sequence.

---

### 6. Custom Task: Find Random Location

Create a new **BTTask_BlueprintBase** task.

The task should generate a random reachable location within the NavMesh area.

The Unreal Engine Navigation System function can be used:

```cpp id="2g4x8s"
UNavigationSystemV1::GetRandomReachablePointInRadius()
```

The generated location is stored in the Blackboard key:

`TargetLocation`

The Behavior Tree then uses the **Move To** node to move the AI toward the generated location.

---

### 7. Test the AI

* Add a player character to the level.
* Place the AI enemy in the map.
* Assign the appropriate AI Controller.
* Assign the Behavior Tree.
* Ensure the `NavMeshBoundsVolume` covers the required area.
* Press **Play** to test the AI.

### Expected Behavior

```text id="0jv8xp"
AI Starts
     ↓
Random Roaming
     ↓
Player Enters Sight Range
     ↓
AI Perception Detects Player
     ↓
AI Chases Player
     ↓
Player Leaves Sight Range
     ↓
AI Resumes Random Roaming
```

---

## Output

### AI Random Roaming / Navigation

<img width="880" height="449" alt="AI Random Roaming" src="https://github.com/user-attachments/assets/74c416e1-692e-4777-aa0a-f89872e05fc9" />

<br>

### AI Behavior Tree / Chase System

<img width="876" height="748" alt="AI Behavior Tree" src="https://github.com/user-attachments/assets/a1abf0ae-586d-42c7-9d23-d7b48f22a148" />

---

## Result

The AI character was successfully implemented to **roam randomly within a defined NavMesh area**.

When the player enters the AI's sight range, the AI detects the player using **AI Perception**, stops roaming, and begins to chase the player.

When the player moves out of the AI's sight range, the AI stops chasing and resumes its **random roaming behavior**.

Thus, the **AI Random Roam with Chase system using NavMesh, Behavior Tree, Blackboard, and AI Perception** was successfully implemented in Unreal Engine.
