# Architectural Decisions

This document explains the reasoning behind some of the key architectural and design pattern choices made during the development of Suits of Siege. 
The primary goal was to create a codebase that was clean, decoupled, and easy to extend with new features in the future.

---

### Why use a component-based, manager-driven architecture?

The project was intentionally structured around the principle of Single Responsibility, where each major part of the game is handled by its own dedicated manager. This approach made the project far more modular, 
allowing me to focus on one system at a time — for example, I could modify the `WaveManager`'s spawning logic without any risk of breaking how the `TowerManager` handles tower firing.

The `GameManager` acts as the central coordinator for these otherwise independent systems. By implementing it as a Singleton, it provides a grounded, central point for managing the overall game state and allowing 
the different managers to communicate when necessary.

This design also makes the project highly extensible. To add a major new feature, like a spell system, I could create a new `SpellManager` as a self-contained component. Because each system operates as a "closed loop", 
this new manager could then be integrated via the `GameManager` with minimal interference to the rest of the codebase.

---

### Why use the Command Pattern for player actions?

The primary motivation for using the Command Pattern was to prevent the `TowerManager` from becoming overly complex and to keep the UI decoupled from the game's core logic.

I wanted the `TowerManager`'s main responsibility to be simple: to manage the collection of towers and their current states. By using commands, the process is cleaner. The UI's job is just to create a command object 
(like `UpgradeTowerCommand`) that represents the player's intent. That command then handles the execution of the specific logic itself. Afterward, the `TowerManager` is simply left to manage the tower in its new, 
updated state.

This was a conscious effort to decouple the systems. By preventing the UI from being too tightly coupled with the game's rules, I could change the logic for upgrading or placing towers without needing to modify the UI 
code. This reduced the risk of a single change breaking multiple systems and allowed each class to thrive independently.

This design also makes future expansion much more straightforward. For planned features like selling a tower or adding a currency system, the Command and Event systems would be crucial. A "Sell Tower" action would be 
its own command, which could then publish an event that the `TowerManager`, UI, and a new `CurrencyManager` could all listen and react to, without needing direct, complex connections to each other.

---

### Why use the Strategy & Decorator patterns for tower abilities?

A primary goal for this project was to build a system that was highly extensible. The core design philosophy was that if I wanted to expand the game later, I could easily create a new class based on a 'base' version 
of a tower, enemy, or ability without needing to rewrite a lot of existing logic.

Using a rigid inheritance structure would have made this difficult. Creating a specific class for every combination of suit and special card (e.g., `KingOfSpadesTower`, `QueenOfClubsTower`) would lead to a large and 
unmanageable number of classes.

The chosen patterns solve this problem directly:
* **Strategy Pattern:** By giving each tower an `ITowerAttackStrategy`, new tower abilities can be created as a new, self-contained strategy class. The `TowerFactory` can then easily assign this new strategy to a tower 
type.
* **Decorator Pattern:** This allows special abilities from evolved cards (like a King or Queen) to be layered on top of any existing suit strategy. This provides maximum flexibility for combining effects and is 
crucial for creating strategic depth.

This combination means I can easily add new suits or new special cards in the future, and they will all be able to work together without conflict. This same philosophy of extensibility was applied to other systems, 
such as the enemies, allowing for new types of bosses with unique effects to be easily added later on.
