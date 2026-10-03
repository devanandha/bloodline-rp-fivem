# Bloodline Dual Character System

## Overview

The Bloodline Dual Character System is a persistent character-management system developed for Bloodline RP.

The system allows a player to maintain multiple roleplay identities while keeping each character's data, appearance and gameplay state separated.

Rather than treating character selection as a simple menu, the system manages a lifecycle involving:

- Character ownership
- Character-slot permissions
- Persistent character data
- Appearance state
- Character selection
- Spawn selection
- Character loading
- Administrative controls
- Recovery from invalid appearance state

A simplified lifecycle is:

    Player Connects
           |
           v
    Character System
           |
      +----+----+
      |         |
      v         v
    Slot 1    Slot 2
      |         |
      |    Permission Check
      |         |
      +----+----+
           |
           v
    Character Selection
           |
           v
    Appearance Loading
           |
           v
      Spawn Selection
           |
           v
       Enter World

---

## Core Features

The Dual Character System includes:

- Two character slots
- Primary character access
- Controlled second-character access
- Administrator-controlled Slot 2 unlocking
- Character ownership validation
- Persistent character information
- Appearance integration
- Character preview
- Cinematic selection presentation
- Spawn selection
- Character loading
- Default appearance handling
- Recovery functionality
- Database-backed slot state

The objective was to allow multiple persistent identities without allowing one character's state to incorrectly affect another.

---

## Character Slot Architecture

The system is based around two character slots.

    Player Account
          |
     +----+----+
     |         |
     v         v
   Slot 1    Slot 2
     |         |
     |    Unlock Required
     |         |
     +----+----+
          |
          v
    Character Identity
          |
          +-- Character Data
          +-- Appearance
          +-- Spawn State
          +-- Gameplay State

Slot 1 represents the normal character path.

Slot 2 provides an additional roleplay identity but can be controlled through an administrative unlock process.

---

## Controlled Second Character

The second character slot was designed as a controlled feature rather than automatically giving every account unrestricted additional character creation.

Before Slot 2 can be used, the system can validate whether the player has permission to access it.

Conceptually:

    Player Selects Slot 2
             |
             v
      Check Slot Access
             |
        +----+----+
        |         |
        v         v
      Locked    Unlocked
        |         |
        v         v
      Deny      Continue
                  |
                  v
          Character Workflow

This provides server administration with control over additional character access.

---

## Administrative Unlocking

Administrative functionality allows authorised staff to manage second-character access.

This creates a persistent relationship between:

- Player identity
- Character-slot entitlement
- Administrative permission
- Database state

The unlock therefore needs to persist beyond the current session.

Development evidence includes a dedicated database structure for Bloodline dual-character slot information.

---

## Ownership Validation

A character-selection system must ensure that players can access only identities associated with them.

The system therefore includes ownership and identity validation as part of the character lifecycle.

The intended relationship is:

    Player
      |
      v
    Validate Identity
      |
      v
    Validate Character
      |
      v
    Validate Slot
      |
      v
    Load Character

This helps prevent character selection from becoming only a client-side interface decision.

---

## Character Data Separation

Each roleplay identity must remain logically separate.

Character-specific state can include information such as:

- Identity
- Character metadata
- Appearance
- Spawn information
- Associated gameplay state

The core requirement is:

    Character A State
          !=
    Character B State

Switching identities should therefore load the selected character's state rather than carrying inappropriate state from the previously active identity.

---

## Character Selection Interface

The character-selection process includes a visual presentation layer rather than functioning only as a text menu.

The interface was developed to provide a more immersive transition between connecting to the server and entering the roleplay world.

The selection experience includes concepts such as:

- Character slots
- Character information
- Character preview
- Selection controls
- Cinematic presentation
- Transition into the selected identity

This required coordination between UI state, character data and the game world.

---

## Cinematic Character Preview

Character selection includes a preview stage designed to visually present the available identity before gameplay begins.

The lifecycle can be represented as:

    Character Data
          |
          v
    Preview Environment
          |
          v
    Character Model
          |
          v
    Appearance Applied
          |
          v
    Cinematic Presentation
          |
          v
    Player Selection

The preview state is temporary and must remain separate from the player's final active gameplay state.

---

## Appearance Integration

Appearance was one of the more important integration areas within the Dual Character System.

Selecting an identity requires more than loading database text fields.

The correct player model and appearance also need to be associated with the selected character.

The relationship can be represented as:

    Selected Character
           |
           v
    Character Identifier
           |
           v
    Appearance Record
           |
           v
      Player Model
           |
           v
    Character Clothing
           |
           v
      Active Character

Incorrect or missing appearance data can therefore affect the entire character-loading process.

---

## Default Appearance Challenge

### Problem

During development, testing exposed situations where a character could load with an incorrect or default appearance.

The problem involved the relationship between character creation, stored appearance data and the appearance-loading workflow.

A character could exist successfully while the expected visual state was not being restored correctly.

### Investigation

The issue required examining the lifecycle across:

- Character creation
- Character identifiers
- Appearance records
- Default skin state
- Character loading
- Appearance-resource integration
- Spawn behaviour

This demonstrated that a successful character database record does not automatically guarantee a valid appearance state.

### Resolution

The character workflow was revised to handle default appearance state more reliably and provide a recovery path where appearance data was missing or invalid.

### Result

Character identity and visual state could be brought back into synchronisation rather than leaving an affected character permanently dependent on the incorrect appearance.

---

## Appearance Recovery

Recovery handling was an important part of making the system resilient.

The ideal lifecycle is:

    Character Selected
           |
           v
    Appearance Available?
         /     \
       Yes      No
        |        |
        v        v
      Load    Recovery /
              Default Flow
                 |
                 v
          Valid Appearance
                 |
                 v
             Continue

This provides a controlled path for abnormal state rather than assuming every character will always contain perfect appearance data.

---

## Spawn Selection

After character selection and loading, the player can transition into the gameplay environment through the spawn workflow.

Conceptually:

    Character Selected
           |
           v
      Data Loaded
           |
           v
    Appearance Applied
           |
           v
     Spawn Selection
           |
           v
      World Loading
           |
           v
      Active Gameplay

Spawn selection therefore occurs as part of the wider character lifecycle rather than being completely independent of character state.

---

## Persistent Slot State

Second-character access must survive reconnects and server restarts.

Slot entitlement is therefore represented as persistent data rather than only an in-memory flag.

Development and deployment evidence includes the Bloodline database structure:

    bloodline_dualcharacter_slots

This supports persistent character-slot access state.

Production records are not included in this public repository.

---

## State Management

The Dual Character System manages several forms of state simultaneously.

### Account-Level State

- Player identity
- Slot entitlement

### Character-Level State

- Character identity
- Character data
- Appearance
- Spawn-related information

### Temporary Selection State

- Preview character
- Camera state
- Selection interface
- Temporary positioning

### Administrative State

- Slot access
- Permission to unlock additional characters

Separating these states is important because they have different persistence requirements.

---

## Temporary vs Persistent State

Character selection illustrates the difference between temporary and persistent state.

For example:

    Persistent
    ----------
    Character identity
    Slot entitlement
    Appearance data

    Temporary
    ---------
    Preview camera
    Selection interface
    Preview position
    Transition state

Temporary selection state should disappear after the player enters the world, while persistent character state must remain available across sessions.

---

## Cross-System Integration

The Dual Character System interacts with several parts of Bloodline RP.

Examples include:

- QBCore character data
- Database persistence
- Appearance system
- Spawn system
- Administrative permissions
- User interface
- Player identity
- Other character-dependent resources

A simplified integration model is:

    Player Account
          |
          v
    Dual Character System
          |
     +----+----+
     |         |
     v         v
    Slot     Character Data
               |
          +----+----+
          |         |
          v         v
      Appearance   Spawn
          |         |
          +----+----+
               |
               v
         Active Player
               |
               v
       Bloodline RP Systems

Once the selected identity becomes active, other resources can operate against the correct character context.

---

## Selected Engineering Challenges

### Challenge 1 — Controlling Second-Character Access

**Problem**

Providing a second character required a persistent way to determine which players were authorised to use the additional slot.

**Resolution**

A dedicated slot-access model with administrative unlocking and persistent database state was introduced.

**Result**

Additional character access could be managed independently from the normal primary-character workflow.

---

### Challenge 2 — Character and Appearance Synchronisation

**Problem**

A valid character record did not always guarantee that the correct appearance would load.

**Resolution**

The loading workflow was examined across character identifiers, appearance records and default state handling.

**Result**

Character identity and appearance could be loaded through a more controlled lifecycle.

---

### Challenge 3 — Invalid or Missing Appearance State

**Problem**

Missing or incorrect appearance information could leave a character loading with an unintended default visual state.

**Resolution**

Default-state handling and recovery behaviour were incorporated into the workflow.

**Result**

Affected characters had a recovery path instead of relying only on the ideal appearance-loading sequence.

---

### Challenge 4 — Preview State vs Gameplay State

**Problem**

Character preview requires temporary models, positioning, cameras and interface state that should not become permanent gameplay state.

**Resolution**

The selection lifecycle was treated separately from the final active-character lifecycle.

**Result**

Cinematic presentation could be used without making preview state part of the player's persistent identity.

---

### Challenge 5 — Maintaining Character Separation

**Problem**

Multiple identities associated with one player must remain separate.

**Resolution**

Character-specific data is loaded according to the selected identity rather than treating the account itself as the complete gameplay identity.

**Result**

Players can maintain distinct roleplay characters while using the same server account.

---

## Development Approach

The Dual Character System was developed through iterative implementation, testing and debugging.

A typical development cycle was:

    Requirement
        |
        v
    Character Workflow Design
        |
        v
    Implementation
        |
        v
    In-Game Testing
        |
        v
    State / Appearance Issue
        |
        v
    Investigation
        |
        v
    Workflow Revision
        |
        v
    Retesting
        |
        v
    Working Character Lifecycle

Appearance-related debugging was particularly useful because it required tracing state across multiple systems rather than modifying only the visible UI.

---

## Technical Areas Demonstrated

The Dual Character System provides evidence of practical work involving:

- Lua-based FiveM development
- QBCore integration
- Persistent identity management
- MySQL-backed state
- Character ownership validation
- Role and permission handling
- Multi-character architecture
- Character appearance integration
- Player model state
- Spawn-system integration
- Temporary versus persistent state
- UI integration
- Cinematic presentation
- Failure recovery
- Cross-resource debugging
- Player lifecycle management
- Administrative functionality

---

## Security & Privacy

The public repository does not include:

- Player identifiers
- Character records
- Private account information
- Production database contents
- Administrative credentials
- Server credentials
- Security configuration

The architecture and engineering process are documented without exposing private player data.

---

## Production Source

The complete production implementation is maintained privately.

This public case study documents the architecture, character lifecycle, persistence model, integration and debugging work without distributing proprietary production source code or security-sensitive configuration.

QBCore and other third-party infrastructure are not presented as original Bloodline RP development.

---

## Status

**The Bloodline Dual Character System was implemented as part of the completed Bloodline RP server, providing persistent multiple-character support with controlled slot access, character selection, appearance management, spawn integration and recovery handling.**
