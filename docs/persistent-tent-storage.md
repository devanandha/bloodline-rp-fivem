# Bloodline Persistent Tent Storage

## Overview

Bloodline Persistent Tent Storage is a placeable world-storage system developed for Bloodline RP.

The system allows a player to purchase and deploy a tent that becomes a persistent private storage location in the game world.

Unlike a temporary prop or simple inventory container, the tent system coordinates several forms of state:

- Physical world placement
- Player ownership
- Persistent location
- Private stash storage
- PIN-based access
- Inventory state
- Placement validation
- Removal and deletion rules
- Administrative permissions
- Database-backed persistence

A simplified lifecycle is:

    Player Obtains Tent
            |
            v
      Placement Mode
            |
            v
    Validate Location
            |
       +----+----+
       |         |
       v         v
    Invalid     Valid
       |         |
       v         v
    Reject    Place Tent
                 |
                 v
         Persist Ownership
                 |
                 v
           Private Storage
                 |
           +-----+-----+
           |           |
           v           v
       PIN Access    Removal
           |           |
           v           v
        Stash       Validation

The main engineering challenge was keeping the physical tent, inventory item, persistent database state and private storage state synchronised.

---

## Core Features

The system includes:

- Purchasable tent item
- Player-controlled placement
- Placement preview
- Persistent world placement
- One-tent-per-player rules
- Ground-position handling
- Placement-distance validation
- Minimum-distance checks
- Restricted placement environments
- Private stash storage
- PIN-protected access
- Failed PIN-attempt handling
- Storage capacity configuration
- Tent removal
- Deletion safeguards
- Inventory transaction protection
- Administrative and management permissions
- Persistent ownership and location state

---

## System Architecture

    Player Inventory
          |
          v
       Tent Item
          |
          v
    Placement Preview
          |
          v
    Location Validation
          |
          v
      Place Object
          |
     +----+----+
     |         |
     v         v
 Ownership   Position
     |         |
     +----+----+
          |
          v
     Persistent Tent
          |
     +----+----+
     |         |
     v         v
 PIN Access   Removal
     |
     v
 Private Stash
     |
     v
 Inventory State

---

## Persistent World Object

The tent is designed as a persistent world object rather than a prop that exists only for the current session.

A placed tent therefore needs to maintain information such as:

- Owner
- Position
- Orientation
- Access configuration
- Associated storage

This state must remain meaningful beyond the immediate placement interaction.

Conceptually:

    Placement
       |
       v
    World Coordinates
       |
       v
    Ownership
       |
       v
    Persistent Record
       |
       v
    World Reconstruction

This provides the foundation for restoring the player's tent as part of the persistent server environment.

---

## Placement Workflow

Tent placement uses a controlled lifecycle.

    Use Tent Item
         |
         v
    Start Preview
         |
         v
    Select Position
         |
         v
    Validate Position
         |
      +--+--+
      |     |
      v     v
    Reject Accept
            |
            v
       Create Tent
            |
            v
       Save State
            |
            v
      Remove Item

A particularly important design decision is that the inventory item should only be removed after placement has completed successfully.

This avoids losing the player's item because of a failed or invalid placement attempt.

---

## Placement Validation

A persistent world object should not be placeable everywhere.

The system therefore includes placement validation covering conditions such as:

- Maximum placement distance
- Minimum distance requirements
- Ground position
- Restricted interiors
- Road placement
- Vehicle-related placement restrictions
- Existing tent ownership

These checks protect both gameplay and persistent world state.

---

## One Tent Per Player

The system was designed around a one-tent-per-player rule.

Before creating another persistent tent, the player's existing tent state can be checked.

Conceptually:

    Player Requests Placement
              |
              v
       Existing Tent?
          /       \
        Yes        No
         |          |
         v          v
       Reject     Continue
                    |
                    v
                 Place

This prevents uncontrolled duplication of persistent personal storage.

---

## Placement Preview

Before final placement, the system provides a preview stage.

The preview allows the player to determine where the tent will be positioned before committing the persistent object.

This introduces temporary state that must remain separate from the final tent.

    Temporary Preview
           |
           v
    Position Adjustment
           |
           v
      Confirmation
           |
           v
    Persistent Object

If the player cancels or placement fails, the temporary preview should disappear without creating permanent state.

---

## Ground & Height Handling

World-object placement introduced challenges around ground position and vertical offsets.

A visually correct preview does not automatically guarantee that the final persistent object will appear at the correct height.

During development, placement behaviour required revisions around:

- Ground detection
- Object height
- Vertical offsets
- Preview position
- Final placement position

This was particularly important because incorrect Z-axis handling could leave tents floating above or partially inside the terrain.

---

## Placement Height Challenge

### Problem

Earlier placement behaviour could produce differences between the previewed tent position and the final world position.

Ground level and model offsets could cause the tent to appear too high or too low.

### Investigation

Testing compared:

- Player position
- Ground position
- Preview coordinates
- Object model offsets
- Final saved coordinates
- Recreated object position

### Resolution

Placement and preview handling were revised so that ground and height calculations produced more consistent final positioning.

### Result

The persistent tent more closely matched the location shown during placement.

---

## Private Storage

Each tent provides private inventory storage.

The storage system was configured around a substantial personal stash, including:

    50 storage slots
    500 kg configured capacity

The stash is associated with the tent rather than functioning as an unrelated generic storage interface.

The intended relationship is:

    Tent
      |
      v
    Ownership
      |
      v
    Access Validation
      |
      v
    Private Stash

This turns the physical world object into an access point for persistent player storage.

---

## PIN-Based Access

Tent storage is protected by a PIN-based access workflow.

The player must provide the correct PIN before being allowed to access the associated storage.

Conceptually:

    Player Interacts
           |
           v
      PIN Requested
           |
           v
      Validate PIN
        /       \
     Invalid    Valid
        |         |
        v         v
     Reject    Open Stash

This adds an access-control layer between world interaction and inventory access.

---

## Failed PIN Attempts

PIN validation includes handling for repeated incorrect attempts.

Rather than relying only on client-side UI behaviour, failed attempts are handled through server-side logic.

The configured workflow includes a maximum of three incorrect attempts.

    PIN Attempt
        |
        v
      Correct?
      /      \
    Yes       No
     |         |
     v         v
   Access   Attempt +1
                |
                v
           Limit Reached?
             /       \
           No         Yes
            |          |
            v          v
          Retry    Disconnect /
                   Enforcement

This demonstrates server-side validation for security-relevant gameplay state.

---

## Inventory Transaction Integrity

Tent placement interacts directly with player inventory.

A key requirement was ensuring that the player did not lose a tent item when placement failed.

The safe lifecycle is:

    Tent Exists in Inventory
              |
              v
       Placement Requested
              |
              v
       Placement Validated
              |
              v
       Object Successfully
            Created
              |
              v
        Persistent State
            Confirmed
              |
              v
       Remove Tent Item

The inventory mutation therefore occurs after successful placement rather than before it.

This is an example of transaction-style thinking applied to gameplay state.

---

## Tent Removal

Removing a tent requires a different workflow from simply deleting a world prop.

The system needs to consider:

- Ownership
- Access permission
- Stored items
- PIN validation
- Persistent records
- World object removal
- Inventory state

A controlled removal workflow prevents persistent storage from being accidentally destroyed.

---

## Storage Deletion Safeguard

One important safeguard is checking storage state before permanent tent deletion.

A tent containing stored items should not be treated exactly like an empty tent.

Conceptually:

    Remove Tent
        |
        v
    Validate PIN
        |
        v
    Check Stash
      /      \
   Items     Empty
     |         |
     v         v
   Block    Continue
               |
               v
          Delete Tent

This reduces the risk of accidentally deleting stored player property.

---

## Storage Integration

The tent system integrates with the server inventory environment.

Development revisions included handling around stash opening and compatibility with the inventory resource.

Fallback behaviour was also considered where necessary to make storage interaction more reliable.

This demonstrates cross-resource integration between:

- Tent state
- QBCore
- Inventory
- NUI
- Persistent storage

---

## NUI Interaction Challenge

### Problem

Storage interaction could leave UI focus in an incorrect state during certain workflows.

This could affect the player's ability to return cleanly to normal gameplay.

### Investigation

The issue required checking the relationship between:

- Tent interaction
- Storage interface
- NUI focus
- Inventory opening
- Interface closing

### Resolution

The storage interaction lifecycle was revised so that interface focus was released appropriately.

### Result

Players could transition more reliably between tent storage and normal gameplay.

---

## Persistent State

The tent system combines several types of persistent information.

### Ownership State

Who owns the tent.

### World State

Where the tent is positioned.

### Access State

How the private storage is protected.

### Storage State

What is contained inside the associated stash.

These states need to remain logically associated.

    Owner
      |
      +--> Tent Position
      |
      +--> Access
      |
      +--> Storage

---

## Temporary vs Persistent State

Tent placement also demonstrates the distinction between temporary and persistent state.

### Temporary State

- Placement preview
- Placement controls
- Temporary object positioning
- UI focus

### Persistent State

- Tent ownership
- Final coordinates
- Storage association
- Access configuration

Temporary placement state should disappear after the interaction.

Persistent tent state must remain available across player sessions and server lifecycle events.

---

## Administrative Controls

The system includes management and administrative permission concepts.

These provide controlled access to functions that should not be available to ordinary players.

This separates:

    Player Interaction

from:

    Management / Administrative Action

Administrative functionality can therefore operate independently from normal ownership-based access.

---

## Cross-System Integration

Persistent Tent Storage interacts with multiple server systems.

Examples include:

- QBCore
- Player inventory
- Persistent database state
- World-object management
- Stash storage
- NUI
- Notifications
- Permissions

A normal placement can involve:

    Inventory
       |
       v
    Placement
       |
       v
    World Object
       |
       v
    Database
       |
       v
    Private Storage

A normal access workflow can involve:

    World Object
       |
       v
    PIN Validation
       |
       v
    Ownership / Access
       |
       v
    Inventory Stash

This makes the system a useful example of cross-resource state management.

---

## Selected Engineering Challenges

### Challenge 1 — Inventory Item Loss

**Problem**

Removing the tent item before confirming successful placement could cause the player to lose the item when placement failed.

**Resolution**

Inventory removal was placed after successful placement validation and object creation.

**Result**

The inventory transaction better reflects the actual outcome of the placement operation.

---

### Challenge 2 — Persistent Object Height

**Problem**

Preview coordinates and final world-object height could differ because of terrain and model offsets.

**Resolution**

Ground-position and height handling were revised through repeated placement testing.

**Result**

Preview and final tent placement became more consistent.

---

### Challenge 3 — Secure Storage Access

**Problem**

A world-accessible object requires a mechanism to protect private storage.

**Resolution**

PIN validation and server-side failed-attempt tracking were incorporated into the access workflow.

**Result**

Physical access to the tent did not automatically provide unrestricted access to its storage.

---

### Challenge 4 — Preventing Storage Loss

**Problem**

Deleting a tent while its stash still contains items could destroy or orphan player property.

**Resolution**

Tent removal incorporates storage-state validation before permanent deletion.

**Result**

The deletion lifecycle provides additional protection for persistent inventory.

---

### Challenge 5 — UI Focus During Storage

**Problem**

Cross-resource inventory interaction could leave NUI focus active incorrectly.

**Resolution**

The storage opening and closing lifecycle was revised to manage interface focus more reliably.

**Result**

Players could return to normal gameplay after storage interaction without remaining trapped in an interface state.

---

## Development Approach

The Persistent Tent Storage system followed an iterative development cycle:

    Requirement
        |
        v
    Initial Implementation
        |
        v
    In-Game Placement Test
        |
        v
    State / Position Issue
        |
        v
    Investigation
        |
        v
    Revision
        |
        v
    Persistence Test
        |
        v
    Storage Test
        |
        v
    Working Behaviour

Multiple revisions were required around placement preview, object height, storage integration and UI behaviour.

This demonstrates development beyond initial feature implementation into edge-case handling and reliability.

---

## Technical Areas Demonstrated

The Persistent Tent Storage system provides evidence of practical work involving:

- Lua-based FiveM development
- QBCore integration
- Persistent world objects
- Coordinate management
- Ground-position handling
- Object placement
- Temporary versus persistent state
- Inventory integration
- Transaction integrity
- Private stash storage
- PIN-based access control
- Server-side validation
- Failed-attempt handling
- Permission management
- NUI integration
- UI focus management
- Database-backed persistence
- Edge-case testing
- Cross-resource debugging

---

## Security & Privacy

The public repository does not contain:

- Player PINs
- Player identifiers
- Private stash contents
- Production database records
- Administrative credentials
- Server credentials
- Security configuration

The system is documented at an architectural and engineering level without exposing private player data or production security information.

---

## Production Source

The complete production implementation is maintained privately.

This public case study documents the architecture, behaviour, persistence model, access-control approach and engineering challenges without distributing proprietary production source code or security-sensitive configuration.

QBCore, inventory resources and other third-party infrastructure are not presented as original Bloodline RP development.

---

## Status

**Bloodline Persistent Tent Storage was implemented as part of the completed Bloodline RP server, providing persistent placeable world storage with ownership, access control, inventory integration and protected removal workflows.**
