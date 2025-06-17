# Codebase Overview

The project's source code is organised into several key namespaces, each with a distinct responsibility. This separation of concerns ensures the codebase is clean, scalable, and easy to maintain. The following is a high-level guide to the structure and purpose of the main components.

---

## Core Systems & Initialisation

This is the heart of the application, managing the game loop, state, and all other high-level managers.

* **`SuitsOfSiegeGame.cs`**: The main class for the game, inheriting from MonoGame's `Game` class. It is the application's entry point and is responsible for initialising the game window and kicking off the core `Update` and `Draw` cycles.
* **`GameManager.cs`**: A central Singleton that acts as the coordinator for the entire game. It manages the current game state (e.g., Main Menu, Playing, Paused), and holds references to all other major managers.
* **`AssetManager.cs`**: A centralised system built upon MonoGame's `ContentManager` that handles the loading, caching, and retrieval of all game assets, such as textures and fonts, to maintain performance.
* **`EventSystem.cs`**: A global Singleton that implements the Observer (Pub/Sub) pattern. It allows different parts of the game to communicate with each other (e.g., the UI publishing a "button clicked" event) without being tightly coupled.

---

## Tower System

This is the largest part of the project, handling everything from a tower's creation and placement to its combat behaviour and upgrades. It is broken into several namespaces for organisation.

### TowerManagement/

This folder contains the core logic for managing and controlling towers.
* **`BaseTower.cs`**: The abstract base class for all towers in the game. It defines the common properties like level, damage, range, and fire rate, as well as the core `Upgrade()` and `EvolveToSpecialCard()` methods.
* **`TowerManager.cs`**: A high-level manager that maintains the list of all active towers. It is responsible for creating towers via its factory and executing `ICommand` objects on them (e.g., `UpgradeTowerCommand`).
* **`TowerFactory.cs`**: Implements the Factory pattern to handle the complex creation of different tower types, from a `BasicTower` to specific `SuitTowers`.
* **`TowerPlacementController.cs`**: Manages the player's interaction with the map for placing towers. It checks if a location is valid by reading map data and ensures towers do not overlap.

### TowerVariants/

This folder contains the concrete implementations for each different type of tower that can exist in the game.
* **`BasicTower.cs`**: The default tower before it has been morphed into a suit.
* **`ClubsTower.cs`, `DiamondsTower.cs`, etc.**: These classes represent the suit-specific towers. Each one sets its own unique `AttackStrategy` (e.g., `SpadesTower` uses `SpadesAttackStrategy` to apply a slow effect).

### Strategies/

This folder contains all the implementations for the **Strategy** and **Decorator** patterns that define a tower's unique combat effects.
* **`ITowerAttackStrategy.cs`**: The simple interface that defines the `Attack()` method required for all attack strategies.
* **`SpecialCardStrategy.cs`**: The abstract class that serves as the base for the Decorator pattern. It wraps another `ITowerAttackStrategy`, allowing special abilities to be layered on top of base suit abilities.

---

## UI System

This system handles everything the player sees and interacts with, from the main menu to the in-game heads-up display (HUD).

### UI/

* **`UIElement.cs`, `UIPanel.cs`, `UIButton.cs`**: A simple, composite-based UI framework. `UIElement` is the abstract base class, `UIPanel` can contain other elements, and `UIButton` handles player clicks.
* **`HUDManager.cs`**: Manages the main in-game interface, including the top and bottom HUD panels and the `TowerMenu` that appears when a tower is selected.
* **`TowerMenu.cs`**: A specialised `UIPanel` that displays the selected tower's stats and provides buttons for upgrading, morphing, and evolving it.
* **`ButtonFactory.cs`**: A dedicated factory for creating all the different types of UI buttons used in the game, from menu buttons to tower action buttons.

---

## Other Key Systems

The remaining folders contain logic for other core gameplay systems, ensuring that each part of the project has a well-defined home.

### Enemy/

This folder contains all logic related to enemy units and wave management.
* **`BaseEnemy.cs`**: The base class for all enemy units, handling their movement along a set of waypoints, their health, and any status effects applied to them.
* **`WaveManager.cs`**: A crucial manager that controls the entire flow of enemy waves, using `WaveConfig.cs` to determine the timing, count, and type of enemies to spawn for each wave.
* **`EnemyFactory.cs`**: Implements the Factory pattern to create enemy instances, assigning them the correct stats and textures based on the wave number.

### ProjectileManagement/

This small but important system manages all the projectiles fired by towers.
* **`BaseProjectile.cs`**: The class for a single projectile, which travels towards a target enemy and applies damage upon collision.
* **`ProjectileManager.cs`**: Manages the list of all active projectiles, updating their position each frame and removing them upon impact or expiration.

### Commands/

This folder contains all implementations of the **Command** pattern.
* **`ICommand.cs`**: The interface for all command objects, ensuring they have an `Execute()` method.
* **`PlaceTowerCommand.cs`, `UpgradeTowerCommand.cs`, etc.**: Each class encapsulates a single, specific player action. This decouples the UI from the game logic; the UI simply creates and invokes a command without needing to know the details of how the action is performed.
