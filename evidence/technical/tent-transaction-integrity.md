# Persistent Tent Transaction Integrity — Sanitised Technical Evidence

## Purpose

This document provides a simplified technical representation of the transaction and state-management principles used in the Bloodline RP Persistent Tent Storage system.

It is **not production source code**.

The pseudocode and diagrams below are sanitised representations of the documented engineering logic. They demonstrate how inventory state, world-object placement, persistence and private storage were coordinated without publishing proprietary Lua implementation, internal events or security-sensitive configuration.

---

## Engineering Problem

A Bloodline tent begins as an inventory item and becomes a persistent world object with private storage.

This creates a multi-stage operation:

    Inventory Item
          |
          v
    Placement Preview
          |
          v
    Placement Validation
          |
          v
    World Object
          |
          v
    Persistent Tent State
          |
          v
    Private Storage

The important engineering problem was determining when the original inventory item should be consumed.

If the item were removed too early, a failed placement could cause the player to lose the tent without receiving the persistent world object.

---

## Unsafe Placement Sequence

A problematic workflow would be:

    Player Uses Tent Item
             |
             v
       Remove Item
             |
             v
      Start Placement
             |
             v
     Placement Fails
             |
             v
        No Tent
             +
       Item Already Gone

This creates an inconsistent state.

The player's inventory says the operation completed, while the world state says it did not.

---

## Transaction-Safe Placement Principle

The placement workflow was therefore structured around successful completion.

Conceptually:

    Tent Exists in Inventory
              |
              v
       Placement Requested
              |
              v
        Preview Position
              |
              v
       Validate Placement
              |
          +---+---+
          |       |
          v       v
       Invalid   Valid
          |       |
          v       v
        Reject  Create Tent
                  |
                  v
            Save Persistent
                State
                  |
                  v
           Confirm Success
                  |
                  v
          Remove Tent Item

The inventory mutation occurs after successful placement rather than before it.

---

## Simplified Placement Logic

The production implementation remains private.

Illustrative pseudocode:

    function placeTent(player, requestedPosition):

        tentItem =
            findTentItem(player)

        if tentItem does not exist:
            reject()
            return

        if playerAlreadyHasTent(player):
            reject()
            return

        position =
            validatePlacement(
                player,
                requestedPosition
            )

        if position is invalid:
            reject()
            return

        tent =
            createTentObject(position)

        if tent creation failed:
            reject()
            return

        persistentRecord =
            saveTentState(
                player,
                position
            )

        if persistentRecord failed:
            cleanupTentObject(tent)
            reject()
            return

        removeTentItem(
            player,
            tentItem
        )

        confirmPlacement(player)

This pseudocode demonstrates the transaction sequence rather than reproducing the production implementation.

---

## Placement Validation

Before committing persistent state, the placement request needs to satisfy the configured placement rules.

Documented validation areas include:

- Placement distance
- Minimum-distance requirements
- Ground position
- Restricted interiors
- Road placement
- Vehicle-related restrictions
- Existing tent ownership

Conceptually:

    Requested Position
           |
           v
    Validate Distance
           |
           v
      Validate Ground
           |
           v
    Validate Environment
           |
           v
    Validate Ownership
           |
           v
       Accept / Reject

An invalid request should not consume the player's inventory item.

---

## Temporary vs Persistent Placement State

The placement system contains both temporary and persistent state.

### Temporary

- Preview object
- Preview coordinates
- Placement controls
- UI state
- Position adjustments

### Persistent

- Tent ownership
- Final coordinates
- Orientation
- Access configuration
- Associated storage

The intended lifecycle is:

    Temporary Preview
           |
           v
       Validation
           |
       +---+---+
       |       |
       v       v
    Cancel   Confirm
       |       |
       v       v
    Cleanup  Persistent State

Cancelling the preview should not create a permanent tent.

---

## Ground & Height Consistency

World-object placement introduced another state problem.

The preview could appear correct while the final object appeared too high or too low because of terrain, ground detection or model offsets.

The relevant state included:

    Player Position
          |
          v
    Preview Coordinates
          |
          v
    Ground Position
          |
          v
    Model Offset
          |
          v
    Final Coordinates
          |
          v
    Persistent Position

Testing therefore needed to compare the temporary preview position with the final recreated world object.

---

## One-Tent-Per-Player State

Before creating another persistent tent, the system checks the player's existing tent state.

Conceptually:

    Placement Request
           |
           v
    Existing Persistent Tent?
         /             \
       Yes              No
        |                |
        v                v
      Reject          Continue

This prevents uncontrolled duplication of persistent personal storage.

---

## Private Storage Access

Once placed, the tent becomes an access point for private storage.

The documented storage configuration includes:

    50 slots
    500 kg configured capacity

Access is protected by a PIN workflow.

Conceptually:

    Player Interacts
           |
           v
       Request PIN
           |
           v
      Validate PIN
        /       \
    Invalid     Valid
       |          |
       v          v
     Reject    Open Stash

The physical presence of the tent does not automatically provide unrestricted access to the associated storage.

---

## Failed PIN Attempts

Incorrect PIN attempts are handled through server-side logic.

The documented configuration includes a maximum of three incorrect attempts before enforcement behaviour.

A simplified representation is:

    PIN Submitted
         |
         v
      Correct?
      /      \
    Yes       No
     |         |
     v         v
   Access   Increment
            Attempts
               |
               v
          Limit Reached?
            /       \
          No         Yes
           |          |
           v          v
         Retry     Enforcement

The exact production enforcement implementation remains private.

---

## Protected Tent Removal

Deleting a persistent tent requires more than removing the visible world object.

The system needs to consider:

- Ownership
- PIN/access validation
- Associated storage
- Stored items
- Persistent database state
- World-object state

A simplified removal lifecycle is:

    Remove Tent Request
            |
            v
      Validate Access
            |
            v
       Validate PIN
            |
            v
       Inspect Stash
          /       \
       Items      Empty
         |          |
         v          v
       Block     Continue
                    |
                    v
              Delete Record
                    |
                    v
              Remove Object

This reduces the risk of destroying or orphaning stored player property.

---

## Simplified Removal Logic

Illustrative pseudocode:

    function removeTent(player, tent):

        if not validOwnerOrPermission(
            player,
            tent
        ):
            reject()
            return

        if not validAccessConfirmation(
            player,
            tent
        ):
            reject()
            return

        if tentStorageContainsItems(tent):
            reject(
                "Storage must be empty"
            )
            return

        persistenceRemoved =
            deletePersistentTentRecord(tent)

        if not persistenceRemoved:
            reject()
            return

        removeWorldObject(tent)

        confirmRemoval(player)

Again, this represents the engineering lifecycle rather than the private implementation.

---

## Preventing Partial State

The placement and deletion workflows both involve multiple systems.

### Placement Failure Example

Undesirable:

    Item Removed
         |
         v
    Tent Creation Fails

Preferred:

    Validate
       |
       v
    Create
       |
       v
    Persist
       |
       v
    Consume Item

### Removal Failure Example

Undesirable:

    World Object Deleted
            |
            v
    Storage Still Contains Items

Preferred:

    Validate Access
          |
          v
      Check Storage
          |
          v
    Delete Persistent State
          |
          v
     Remove World Object

This is transaction-style reasoning applied to gameplay state.

---

## NUI / Inventory Integration

Tent storage also interacts with the inventory interface.

A storage operation can involve:

    Tent Interaction
          |
          v
     PIN Interface
          |
          v
    Storage Request
          |
          v
    Inventory Interface
          |
          v
     Close Storage
          |
          v
    Release UI Focus
          |
          v
    Normal Gameplay

Testing identified situations where storage could function while interface focus remained incorrect.

The interaction lifecycle therefore needed to manage both the storage operation and the player's interface state.

---

## Failure-State Thinking

Useful failure questions included:

- What happens if placement validation fails?
- What happens if world-object creation fails?
- What happens if persistent state cannot be confirmed?
- Has the inventory item already been removed?
- What happens if the player already owns a tent?
- What happens if the PIN is incorrect?
- What happens after repeated failed PIN attempts?
- What happens if storage still contains items?
- What happens if UI focus is not released?

These questions helped identify states where one resource could complete while another remained incomplete.

---

## State Model

The system can be summarised as four related forms of state:

    Player
      |
      +--> Inventory State
      |
      +--> Tent Ownership
      |
      +--> World Position
      |
      +--> Private Storage

These states need to remain logically synchronised.

A visible tent without valid persistent ownership is incorrect.

Persistent ownership without a usable world object is also incorrect.

Removing the inventory item without successful placement is incorrect.

Deleting the tent while storage remains populated can also create an invalid state.

---

## Debugging Approach

Development required testing the complete lifecycle rather than only checking whether a tent model appeared.

Investigation included:

- Inventory consumption timing
- Placement preview
- Placement validation
- Ground position
- Vertical offsets
- World-object creation
- Persistent ownership
- One-tent-per-player state
- PIN validation
- Failed attempts
- Stash opening
- Stored-item protection
- NUI focus
- Tent removal
- Persistent cleanup

This turned the feature from a placeable prop into a persistent state-management problem.

---

## Engineering Outcome

The resulting workflow coordinates inventory, placement, persistent state and storage through controlled lifecycle transitions.

The work demonstrates practical experience involving:

- Persistent world objects
- Inventory integration
- Transaction sequencing
- Failure-state handling
- Temporary versus persistent state
- Coordinate management
- Ground-position debugging
- Access control
- Server-side validation
- Private storage integration
- UI state management
- Database-backed persistence
- Cross-resource debugging

---

## Supporting Documentation

See:

- [Persistent Tent Storage](../../docs/persistent-tent-storage.md)
- [Engineering Challenges](../engineering-challenges.md)
- [Development Timeline](../development-timeline.md)
- [Engineering Evidence Index](../evidence-index.md)

---

## Evidence Boundary

This document intentionally does not publish:

- Production Lua source
- Player PINs
- Internal event names
- Inventory implementation details
- Private stash contents
- Database credentials
- Server credentials
- Player identifiers
- Security configuration

The pseudocode documents the engineering architecture and state transitions without reproducing the private production implementation.
