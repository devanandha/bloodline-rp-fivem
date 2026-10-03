# Bloodline RP — Engineering Challenges & Problem Solving

## Overview

Bloodline RP was developed through repeated implementation, in-game testing, debugging and revision.

Many of the most significant engineering tasks were not the initial creation of a feature, but resolving problems that appeared when persistent state, multiplayer sessions, inventory systems and separate FiveM resources interacted with each other.

This document records selected engineering challenges encountered during development using the following structure:

    Problem
       |
       v
    Investigation
       |
       v
    Engineering Change
       |
       v
    Retesting
       |
       v
    Result

The examples focus on technical reasoning and system behaviour rather than exposing proprietary production source code.

---

# Challenge 1 — Jerrycan Fuel Persistence

## Problem

The Bloodline fuel system included reusable jerrycans with finite fuel capacity.

The intended model was:

    100 litres = 100 durability

As fuel was consumed, the remaining capacity needed to decrease and remain associated with that specific inventory item.

Testing exposed an important edge case.

Removing and re-adding a partially used jerrycan could cause its fuel state to behave incorrectly.

If the remaining state was recreated as a full container, normal inventory operations could effectively provide unlimited fuel.

## Investigation

The issue required examining the relationship between:

- Inventory slots
- Item metadata
- Item removal
- Item re-addition
- Jerrycan durability
- Remaining fuel
- Client state
- Server state

The problem demonstrated that temporary client-side fuel state was not sufficient for a persistent inventory object.

## Engineering Change

Jerrycan state handling was revised so that remaining capacity could persist through item metadata and server-side handling.

The container's remaining fuel therefore became associated with the inventory item rather than only the current interaction.

## Result

Partially used jerrycans could retain their remaining state across inventory operations.

This prevented a state-reset exploit and created a more reliable finite-container model.

## Engineering Areas

- Inventory metadata
- Persistent state
- Server authority
- Client/server synchronisation
- Edge-case testing
- Exploit prevention

---

# Challenge 2 — Dynamic Vehicle Fuel-Cap Interaction

## Problem

Vehicle refuelling initially faced a physical interaction problem.

Different GTA vehicle models do not all place their fuel-cap area in the same position.

A fixed interaction point could therefore:

- Appear on the wrong side
- Feel unnatural
- Be difficult to access
- Fail to represent the selected vehicle correctly

## Investigation

Testing compared vehicle interaction behaviour across different models.

The investigation considered:

- Vehicle position
- Vehicle orientation
- Interaction zones
- Entity targeting
- Driver-side positioning
- Player distance from the vehicle

## Engineering Change

Fuel interaction was revised toward vehicle-associated dynamic targeting rather than depending only on a static world-space zone.

## Result

The refuelling interaction became more closely associated with the actual vehicle and more consistent across different vehicle models.

## Engineering Areas

- Vehicle entities
- World coordinates
- Targeting systems
- Spatial interaction
- Gameplay testing

---

# Challenge 3 — Vehicle Purchase & Key Synchronisation

## Problem

During vehicle-shop development, a vehicle could be purchased successfully while the player's immediate key access did not correctly reflect the new ownership state.

This created an inconsistent result:

    Vehicle Purchase = Successful

but:

    Vehicle Access = Incorrect

## Investigation

The complete purchase lifecycle was traced across:

- Payment
- Vehicle creation
- Ownership record
- Vehicle plate
- Key item/state
- Vehicle-key resource
- Immediate access

The issue was therefore treated as a cross-resource integration problem rather than only a dealership problem.

## Engineering Change

The purchase workflow was revised so that ownership creation and the appropriate vehicle-key state occurred as part of the same lifecycle.

## Result

A completed purchase could transition directly into usable vehicle ownership.

## Engineering Areas

- Persistent ownership
- Cross-resource integration
- Vehicle identity
- Inventory/key state
- Transaction lifecycle

---

# Challenge 4 — Temporary Test-Drive Access

## Problem

Test-drive vehicles are intentionally not permanently owned by the player.

Normal vehicle-access rules could therefore interpret a legitimate test-drive vehicle as a vehicle for which the player had no valid key.

This could trigger:

- Missing-key behaviour
- Engine restrictions
- Hotwire-related behaviour

## Investigation

The difference between permanent and temporary vehicle state was examined.

The required distinction was:

    Purchased Vehicle
          =
    Persistent Ownership

while:

    Test-Drive Vehicle
          =
    Temporary Authorisation

## Engineering Change

The vehicle-access lifecycle was revised so that active test-drive vehicles could be recognised as temporarily authorised.

## Result

Players could operate legitimate test-drive vehicles without weakening normal ownership rules for other vehicles.

## Engineering Areas

- Temporary state
- Persistent state
- Vehicle permissions
- Lifecycle design
- Cross-resource integration

---

# Challenge 5 — TDM vs Normal RP Death Behaviour

## Problem

Bloodline TDM runs inside the same server as normal roleplay.

Standard GTA/QBCore death behaviour could interfere with TDM-specific combat before the competitive system had completed its own hit and elimination logic.

Normal RP death could also incorrectly involve EMS behaviour.

## Investigation

The combat lifecycle was examined across:

- Player health
- GTA damage
- QBCore death state
- TDM hit tracking
- Headshots
- Body hits
- EMS behaviour
- Respawn behaviour

The key architectural requirement became clear:

    Normal RP Medical State

must remain separate from:

    Competitive TDM State

## Engineering Change

TDM-specific health, damage and elimination handling was introduced.

Temporary competitive state was kept within the TDM lifecycle rather than intentionally modifying normal RP medical state.

## Result

Competitive eliminations could be controlled by TDM while normal RP injury and EMS workflows remained separate.

## Engineering Areas

- State isolation
- Combat systems
- Player lifecycle
- Cross-resource conflict resolution
- Temporary session state

---

# Challenge 6 — TDM Weapon Timing

## Problem

Testing identified that a TDM weapon could become available before the player had fully entered the competitive session.

The player could still be transitioning through:

- Menu state
- Routing
- Teleportation
- Spawn preparation
- Collision loading

## Investigation

The session-entry sequence was reviewed.

The original lifecycle needed stronger ordering between world transition and competitive equipment.

## Engineering Change

Weapon activation was moved later in the session-entry lifecycle.

Conceptually:

    Join Match
        |
        v
    Assign Routing Bucket
        |
        v
    Teleport
        |
        v
    Prepare Spawn
        |
        v
    Load World / Collision
        |
        v
    Activate TDM Equipment

## Result

Competitive equipment became associated with the active match rather than the transition/menu environment.

## Engineering Areas

- Event ordering
- Multiplayer state
- Routing buckets
- Equipment lifecycle
- Race-condition-style behaviour

---

# Challenge 7 — TDM Player Departure vs Room Lifecycle

## Problem

An earlier multiplayer behaviour could allow one participant leaving or disconnecting to affect other players in the same TDM room.

Player lifecycle and room lifecycle were too closely coupled.

## Investigation

The system needed to distinguish between:

    Remove One Participant

and:

    Delete Multiplayer Room

These are fundamentally different operations.

The investigation also considered what should happen if the current room owner leaves.

## Engineering Change

Individual player departure was separated from explicit room deletion.

Waiting-room ownership could transfer to another participant where appropriate.

Disconnect handling was also revised so one player's departure did not automatically remove everyone.

## Result

Remaining participants could continue using the room independently.

## Engineering Areas

- Multiplayer session management
- Ownership transfer
- Disconnect handling
- Shared state
- Player lifecycle

---

# Challenge 8 — Unsafe TDM Spawns

## Problem

Competitive environments introduced world-positioning issues.

Players could encounter:

- Unsafe terrain heights
- Incomplete collision loading
- Elevated spawn locations
- Falls
- Unsuitable map geometry

## Investigation

Testing examined:

- Spawn coordinates
- Ground position
- Collision availability
- Player height
- Fall behaviour
- Valid gameplay zones

## Engineering Change

The system evolved to include mechanisms such as:

- Safer spawn positions
- Ground checks
- Collision-loading allowance
- Spawn protection
- Fall detection
- Fall recovery
- Last-safe-position handling

## Result

Entry and respawn behaviour became more reliable across different competitive environments.

## Engineering Areas

- World coordinates
- Collision state
- Spawn management
- Recovery logic
- Multiplayer testing

---

# Challenge 9 — TDM Redzone State

## Problem

Competitive maps required bounded gameplay areas.

Zone handling also needed to avoid triggering while a player was still entering the session.

## Investigation

The system needed to distinguish:

    Player Transitioning Into TDM

from:

    Player Actively Outside TDM Boundary

Without this distinction, zone enforcement could react to temporary transition coordinates.

## Engineering Change

Zone handling was associated with the active competitive-session state.

Players remaining outside a configured valid area beyond the permitted grace period could be returned to the gameplay area.

## Result

Boundary enforcement became part of active TDM gameplay without incorrectly controlling players during session transition.

## Engineering Areas

- Zone state
- Session validation
- Coordinates
- Multiplayer state
- Edge-case handling

---

# Challenge 10 — TDM Crash / Restart Recovery

## Problem

The ideal TDM exit sequence assumes that the resource, client and session continue operating normally.

A restart or interrupted client state could leave a player associated with invalid competitive-session state.

## Investigation

Recovery needed to consider:

- Routing state
- Session state
- Player position
- Interrupted match state
- Reconnection

## Engineering Change

Recovery handling was introduced so affected players could be returned toward the normal routing environment and TDM entry context after interrupted competitive state.

## Result

The system gained a recovery path for abnormal termination rather than relying only on the ideal match-exit sequence.

## Engineering Areas

- Failure recovery
- Routing buckets
- Persistent recovery state
- Player lifecycle
- Defensive engineering

---

# Challenge 11 — Character Appearance Recovery

## Problem

During Dual Character development, a valid character could exist while its expected visual appearance did not load correctly.

This could result in an unintended default appearance.

## Investigation

The problem required tracing state across:

- Character creation
- Character identifiers
- Character ownership
- Appearance records
- Default skin state
- Character loading
- Spawn flow
- Appearance-resource integration

The investigation showed that:

    Valid Character Record

does not necessarily mean:

    Valid Appearance Record

## Engineering Change

Default appearance handling and recovery behaviour were incorporated into the character lifecycle.

The workflow was revised to better handle missing or invalid appearance state.

## Result

Affected characters had a controlled recovery path instead of remaining dependent on an incorrect visual state.

## Engineering Areas

- Persistent identity
- Cross-resource state
- Failure recovery
- Character lifecycle
- Appearance integration

---

# Challenge 12 — Tent Inventory Transaction Integrity

## Problem

A placeable tent begins as an inventory item and becomes a persistent world object.

Removing the inventory item before confirming successful placement could create an undesirable state:

    Item Removed
         |
         v
    Placement Fails
         |
         v
    Player Loses Item

## Investigation

The operation was treated as a transaction involving:

- Inventory
- Placement validation
- World-object creation
- Persistent state

## Engineering Change

The lifecycle was reordered:

    Validate Placement
          |
          v
    Create World Object
          |
          v
    Confirm Persistent State
          |
          v
    Remove Inventory Item

## Result

Inventory mutation more accurately reflected the actual outcome of the placement operation.

## Engineering Areas

- Transaction integrity
- Inventory state
- Persistent world objects
- Event ordering
- Failure handling

---

# Challenge 13 — Tent Height & Ground Position

## Problem

A tent could appear correctly during preview but be positioned too high or too low after final placement.

Terrain and model offsets affected the final Z-axis position.

## Investigation

Testing compared:

- Player coordinates
- Ground coordinates
- Preview coordinates
- Model offset
- Saved coordinates
- Reconstructed tent position

## Engineering Change

Ground and height handling were revised through repeated placement testing.

## Result

Preview and persistent placement became more consistent.

## Engineering Areas

- 3D coordinates
- Ground detection
- Persistent objects
- World-state testing

---

# Challenge 14 — Tent Storage Protection

## Problem

A persistent world object containing private inventory requires stronger controls than a normal prop.

Two important risks were:

1. Unauthorised stash access.
2. Deleting a tent while it still contained stored items.

## Investigation

Access and deletion were treated as separate protected operations.

The system considered:

- PIN validation
- Incorrect PIN attempts
- Server-side attempt tracking
- Stash contents
- Ownership
- Deletion state

## Engineering Change

PIN-based access was implemented with server-side handling for failed attempts.

Permanent tent removal also considered whether the associated storage was empty before deletion.

## Result

The physical tent did not automatically provide unrestricted storage access, and deletion included safeguards against accidental stored-item loss.

## Engineering Areas

- Access control
- Server validation
- Persistent storage
- Data protection
- Failure prevention

---

# Challenge 15 — Tent Storage UI Focus

## Problem

Opening storage through the tent system could leave NUI/interface focus in an incorrect state.

The storage itself could work while the player interaction lifecycle remained incomplete.

## Investigation

The interaction was traced across:

- Tent interaction
- Storage request
- Inventory UI
- NUI focus
- Interface closure

## Engineering Change

The opening and closing lifecycle was revised to release interface focus appropriately.

## Result

Players could return more reliably from tent storage to normal gameplay.

## Engineering Areas

- NUI
- UI lifecycle
- Inventory integration
- Cross-resource debugging

---

# Challenge 16 — NPC Marketplace Transaction Integrity

## Problem

The Interactive NPC Marketplace combines basket selection, player money and inventory delivery.

These states must remain coordinated.

An undesirable transaction could look like:

    Payment Successful
           |
           v
    Item Delivery Fails

or:

    Item Delivered
           |
           v
    Payment Not Completed

## Investigation

The transaction was separated into stages:

- Basket selection
- Purchase request
- Validation
- Payment
- Item delivery
- Completion

Temporary UI state was distinguished from authoritative gameplay state.

## Engineering Change

The purchase flow was structured around server validation and an ordered transaction lifecycle.

## Result

Basket selection did not itself represent a completed transaction, and persistent state changes occurred through the controlled purchase workflow.

## Engineering Areas

- Transaction processing
- Client/server validation
- Inventory integration
- Player economy
- Temporary vs persistent state

---

# Challenge 17 — NPC World Interaction

## Problem

An NPC can technically spawn correctly while still providing a poor interaction experience.

Issues can include:

- Incorrect heading
- Poor positioning
- Interaction misalignment
- Animation misalignment
- Player positioning

## Investigation

NPC placement and transaction behaviour were tested directly in the game world.

Coordinates and orientation were revised based on actual player interaction rather than configuration alone.

## Engineering Change

World placement and interaction behaviour were iterated through repeated in-game testing.

## Result

The NPC marketplace became better aligned with the intended environment and interaction flow.

## Engineering Areas

- NPC entities
- World coordinates
- Interaction targeting
- Animation state
- Gameplay testing

---

# Challenge 18 — Cross-System Vehicle Lifecycle

## Problem

Vehicle functionality was distributed across multiple resources.

A vehicle could interact with:

- Dealership
- Payment
- Ownership
- Keys
- Garage
- Engine
- Fuel
- Job access

A successful operation in one resource did not automatically guarantee a valid state in the others.

## Investigation

Vehicle behaviour was therefore examined as a complete lifecycle:

    Purchase
       |
       v
    Ownership
       |
       v
      Keys
       |
       v
     Garage
       |
       v
     Engine
       |
       v
      Fuel

## Engineering Change

Integration points were tested and revised according to the complete vehicle lifecycle rather than treating each resource independently.

## Result

Vehicle state became more consistent across purchasing, access, storage and operation.

## Engineering Areas

- Cross-resource architecture
- Persistent ownership
- State synchronisation
- Integration testing
- Lifecycle design

---

# Challenge 19 — Temporary vs Persistent State

A recurring engineering problem across Bloodline RP was deciding which state should survive beyond the immediate interaction.

Examples include:

| System | Temporary State | Persistent State |
|---|---|---|
| TDM | Match weapon, team, score, routing | Normal RP character state |
| Vehicle Shop | Test-drive access | Purchased ownership |
| Tent Storage | Placement preview | Tent position and ownership |
| NPC Marketplace | Basket | Completed inventory transaction |
| Dual Character | Preview camera | Character and appearance |
| Fuel | Active nozzle interaction | Jerrycan metadata / station stock |

Incorrectly mixing these two categories could produce bugs, exploits or invalid state.

Explicitly separating temporary and persistent state became an important architectural pattern across the project.

---

# Challenge 20 — Cross-Resource Debugging

As the server expanded, many bugs appeared at integration boundaries rather than entirely inside one resource.

Examples included:

    Vehicle Shop
        |
        v
    Vehicle Keys

    Fuel
      |
      v
    Inventory Metadata

    TDM
     |
     v
    Normal RP Death / EMS

    Dual Character
          |
          v
      Appearance

    Tent
      |
      v
    Inventory / NUI

Solving these problems required tracing the complete event and state lifecycle across multiple resources.

This became increasingly important as Bloodline RP evolved from individual features into an interconnected server.

---

# Engineering Method

The recurring debugging process across the project was:

    Observe Unexpected Behaviour
               |
               v
       Reproduce the Problem
               |
               v
       Identify State Involved
               |
               v
      Trace Resource Boundaries
               |
               v
       Review Relevant Logic
               |
               v
        Implement Revision
               |
               v
          In-Game Retest
               |
               v
        Test Edge Cases
               |
               v
      Validate Integration

Technical research, documentation, examples and existing FiveM/QBCore patterns were used where useful during investigation.

The final implementation was shaped through repeated testing and revision rather than assuming an initial implementation was complete.

---

# Engineering Themes Demonstrated

The challenges documented above demonstrate work involving:

- Debugging
- Root-cause investigation
- Client/server architecture
- State synchronisation
- Persistent state
- Temporary state
- MySQL-backed systems
- Inventory metadata
- Multiplayer session management
- Routing buckets
- Failure recovery
- Transaction integrity
- Access control
- Vehicle lifecycle management
- World-coordinate handling
- NUI state
- Cross-resource integration
- Edge-case testing
- Exploit prevention
- Iterative development

---

# Evidence Boundary

This document intentionally describes problems and engineering solutions without publishing the complete production implementation.

Production source remains private to protect:

- Proprietary implementation details
- Server security
- Credentials
- Configuration
- Private player information
- Operational data

Third-party resources and QBCore infrastructure are not presented as original Bloodline RP development.

The evidence focuses on the development, integration, modification, debugging and testing work performed for Bloodline RP.

---

# Conclusion

Bloodline RP required more than implementing individual gameplay features.

The most significant engineering work increasingly involved maintaining correct behaviour across persistent data, multiplayer state, inventory systems, vehicle resources, user interfaces and failure conditions.

The project progressed from feature implementation toward system-level engineering involving:

    State
      +
    Persistence
      +
    Integration
      +
    Recovery
      +
    Testing

The challenges documented here provide examples of how technical problems were investigated, revised and validated during the approximately three-month Bloodline RP development process.
