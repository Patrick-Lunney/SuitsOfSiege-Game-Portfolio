# Suits of Siege: Design & Technical Architecture

This document provides a detailed breakdown of the design, mechanics, and technical architecture of the Suits of Siege project.

---

## Game Overview
Suits of Siege is a Tower Defence game that combines strategic tower defence mechanics with a unique playing card theme, 
challenging players to defend their base through careful tower placement and resource management. 
Built using the MonoGame Framework for fundamental operations while implementing custom systems for gameplay mechanics and state management, 
players defend their base against waves of increasingly challenging enemies through strategic tower placement and upgrades.

## Theme & Mechanics
In this world, playing cards exist as living entities. A revolution has sparked: the lowerranked cards, feeling undervalued, have revolted against their higher-ranked
counterparts. The Aces aligned with the high cards, while the Jokers joined the
rebellion.
This conflict directly influences the core mechanics:
- Towers begin at level 6, representing the lowest of the "high" cards defending their realm
- The progression to level 10 and eventual evolution represents the full range of defensive forces
- Enemy waves consist of the lower-ranked rebellious cards
- Joker bosses appear as powerful leaders of the rebellion every five waves

### Getting Started
Players defend their base by placing towers at strategic points along enemy paths.
Through successful defence, players can upgrade existing towers, transform them into specialized suits, and evolve them into powerful special cards (with resource management planned for future implementation). 
Success requires careful observation of enemy patterns and preparation for boss waves.

## Technical Foundation

### MonoGame Framework Usage
MonoGame Framework provides the game's foundational architecture, handling the essential game loop (Update/Draw cycle), graphics device operations, mouse input handling, asset loading through its content pipeline, 
and sprite rendering via SpriteBatch.

## Custom Core Systems
The game implements three primary custom systems that build upon MonoGame's base functionality:

### State Management
A custom state system controls gameplay flow and user interactions, managing transitions between game states (menu, gameplay, pause) and integrating tower selection, placement, and wave progression systems.

### Resource Management
Built upon MonoGame's ContentManager, this system provides centralized access to game assets (textures, fonts) and handles resource lifecycle management to maintain performance.

### Rendering Implementation
Expands MonoGame's SpriteBatch capabilities with custom layer management for proper drawing order of game elements (

## Core Gameplay Systems

### Game Architecture
The game is built around a central GameManager that coordinates all major systems through an event-driven architecture. This design allows for clean separation of concerns while maintaining efficient communication 
between components. The core loop handles input processing, game state updates, and rendering through distinct phases, ensuring smooth performance and responsive gameplay.
The implementation deliberately utilizes several design patterns to maintain clean and efficient code structure:

**Primary Design Patterns:**
- Singleton Pattern: Implements global access points for critical systems (`GameManager` and `EventSystem`)
- Observer Pattern: Powers the event system, allowing components like `TowerMenu` or `RenderingManager` to communicate through subscription to game events
- Command Pattern: Encapsulates tower operations (placement, morphing, etc) into command objects
- Factory Pattern: Manages creation of complex objects through specialized factories (`TowerFactory`, `ButtonFactory`, `EnemyFactory`)
- Strategy Pattern: Defines interchangeable tower attack behaviours through different attack strategies (Clubs, Spades, etc)
- Decorator Pattern: Implements special card effects through attack strategy decorators (Queen, King, etc)

**Additional patterns emerged through the architecture and framework usage:**
- State Pattern: Manifests through the game state management system
- Template Method Pattern: Inherited from MonoGame’s Game class structure
- Composite Pattern: Naturally emerged in the UI component hierarchy

### Tower Defence Mechanics
Players protect their base (100HP) using a tower system that incorporates the playing card theme into every aspect of gameplay. The tower progression system allows players to develop their defences through multiple stages:
- Towers advance levels, starting at level 6 and reaching a maximum level of 10.
- Each tower can ‘Morph’ (transform) into one of four suits (Hearts, Diamonds, Clubs, Spades) one time.
- At maximum level, towers can ‘Evolve’ into special cards (Jack, Queen, King, Ace).

This system uses the Decorator and Strategy patterns to combine multiple abilities effectively. For example, a ‘Spades’ tower applies slow effects to enemies, while evolution to a ‘Queen’ card adds critical hit capability, 
creating opportunities for strategic depth through tower combinations.

### Combat System
The combat system creates dynamic battles through the interaction between specialized tower abilities and diverse enemy types. Each suit imbues towers with unique combat capabilities:
- Tower Abilities:
  - Spades: Enemy Slow Effects
  - Clubs: Bonus Damage
  - Diamonds: Increased Attack Speed
  - Hearts: Allied Tower Buffing (Not Yet Implemented)

Enemy variety provides escalating challenges through five distinct tiers. Health values range from 100 HP for level 1 enemies to 500 HP for level 5 enemies, while special Joker bosses appear every 5 waves starting at 
1,500 HP. Implementation of this system allows for future additions of suit-specific Joker bosses, each with added special effects, increasing strategic depth.

### User Interface
The interface design focuses on providing clear information and intuitive controls. The top and bottom HUD panels house essential gameplay controls and resource information, while the main battlefield view dominates 
the screen for optimal strategic oversight. When a tower is selected, a menu activates, providing upgrade options and statistics of the tower. All player actions receive feedback, ensuring players understand the results 
of their decisions.
