# TDM State Isolation — Sanitised Technical Evidence

## Purpose

This document provides a simplified technical representation of the multiplayer state-isolation architecture used in the Bloodline RP Team Deathmatch system.

It is **not production source code**.

The pseudocode and diagrams below are sanitised representations of the engineering logic documented from the completed system. They demonstrate multiplayer state management without exposing proprietary Lua implementation, server configuration or security-sensitive logic.

---

## Engineering Problem

Bloodline RP operates as a persistent roleplay environment.

The TDM system introduces a very different type of gameplay:

- Competitive teams
- Public free-for-all sessions
- Match-specific weapons
- Match-specific ammunition
- Scores
- Respawning
- Spawn protection
- Custom combat behaviour
- Temporary team state
- Match-specific maps

These systems needed to operate without incorrectly modifying normal roleplay state.

The central requirement was therefore:

> Competitive TDM state must remain isolated from normal Bloodline RP gameplay state.

---

## High-Level State Model

A player's lifecycle can be simplified as:

    Normal RP State
          |
          v
     Join TDM
          |
          v
    Validate Session
          |
          v
    Assign TDM Context
          |
          v
    Enter Isolated Session
          |
          v
    Temporary Match State
          |
          v
    Leave / Match End
          |
          v
    Clean Temporary State
          |
          v
    Return to Normal RP

The TDM lifecycle should therefore behave as a temporary gameplay context rather than permanently replacing the player's normal roleplay state.

---

## Session Isolation

FiveM routing buckets provide a mechanism for separating groups of players into different networked environments.

Conceptually:

    Main RP World
    Bucket 0
        |
        +----------------------+
        |                      |
        v                      v
    TDM Room A             TDM Room B
    Bucket A               Bucket B
        |                      |
    Team Players           Team Players

Players assigned to one competitive session should interact with the players and entities belonging to that session rather than unrelated players elsewhere on the server.

---

## Simplified Session Assignment

Illustrative pseudocode:

    function joinTDM(player, room):

        if room does not exist:
            reject()
            return

        if player already in TDM:
            reject()
            return

        saveTemporarySessionContext(player)

        assignPlayerToRoom(player, room)

        setRoutingBucket(
            player,
            room.bucket
        )

        preparePlayerForMatch(player)

        teleportToSafeSpawn(player, room)

        giveTemporaryMatchEquipment(player)

        markPlayerAsActive(player)

This represents the lifecycle rather than the production implementation.

---

## Temporary vs Persistent State

A major architectural requirement was distinguishing match-specific state from normal RP state.

### Temporary TDM State

Examples include:

- Team assignment
- Match room
- Score
- Match weapon
- Match ammunition
- Spawn protection
- Redzone state
- Competitive damage configuration
- Temporary map position

### Normal RP State

Examples include:

- Character identity
- Job
- Economy
- Owned vehicles
- Garage state
- Persistent inventory
- Normal medical/EMS context

The intended relationship is:

    TDM State
       !=
    Persistent RP State

Temporary competitive state should exist only for the competitive lifecycle.

---

## Match Equipment Isolation

TDM uses temporary competitive equipment.

The engineering requirement was that match equipment should not become normal persistent RP inventory.

Conceptually:

    Enter TDM
       |
       v
    Create Match Context
       |
       v
    Provide Temporary Weapon
       |
       v
    Competitive Gameplay
       |
       v
    Leave TDM
       |
       v
    Remove Match Equipment
       |
       v
    Return to RP

Simplified pseudocode:

    function provideMatchEquipment(player):

        if not player.inTDM:
            return

        weapon = createTemporaryMatchWeapon()

        assignTemporaryWeapon(
            player,
            weapon
        )

        assignMatchAmmo(
            player,
            configuredAmmo
        )

The key concept is that the equipment belongs to the **match lifecycle**, not the player's permanent roleplay inventory.

---

## Weapon Timing Challenge

During development, simply giving the player a weapon immediately after requesting entry was not always reliable.

Several state transitions could still be occurring:

    Join Request
         |
         v
    Routing Bucket Change
         |
         v
       Teleport
         |
         v
    Player Spawn
         |
         v
    Collision / World Ready
         |
         v
    Match Equipment

Providing equipment before the player had completed the required transition could create inconsistent behaviour.

The workflow was therefore revised so equipment provisioning occurred at the appropriate stage of the TDM entry lifecycle.

---

## Elimination State vs RP Death State

A competitive elimination should not automatically be treated as a normal roleplay medical emergency.

Conceptually:

    Player Health Reaches
    TDM Elimination State
              |
              v
       Is Player in TDM?
          /          \
        Yes           No
         |             |
         v             v
    TDM Respawn     Normal RP
      Workflow      Death/EMS
      Workflow

This separation prevents competitive gameplay from unintentionally invoking normal RP medical behaviour.

---

## Simplified Elimination Handling

Illustrative pseudocode:

    function handlePlayerDown(player):

        if player.inTDM:

            registerElimination(player)

            updateMatchScore()

            clearTemporaryCombatState(player)

            respawnAtTDMLocation(player)

            restoreMatchEquipment(player)

            return

        processNormalRPDownState(player)

This demonstrates the architectural separation.

The production implementation remains private.

---

## Score State

Competitive score is temporary match state.

A simplified model is:

    Match
      |
      +--> Team A Score
      |
      +--> Team B Score
      |
      +--> Target Score
      |
      +--> Player Statistics

When the configured match condition is reached:

    Score Updated
         |
         v
    Target Reached?
       /       \
     No         Yes
      |          |
      v          v
    Continue   End Match
                  |
                  v
              Cleanup

Match scoring should therefore remain associated with the competitive session rather than persistent RP state.

---

## Player Departure

One of the multiplayer lifecycle challenges involved distinguishing:

    Player Leaves Room

from:

    Room Is Deleted

These are not equivalent operations.

A player leaving should not automatically remove every other participant from the competitive session.

Simplified lifecycle:

    Player Requests Leave
             |
             v
      Remove Player
      From Room State
             |
             v
      Clean Player TDM
           State
             |
             v
      Return Player to
         Normal RP
             |
             v
    Room Still Active?
        /         \
      Yes          No
       |            |
       v            v
    Continue     Cleanup Room

This separates the lifecycle of an individual participant from the lifecycle of the multiplayer room.

---

## Room Ownership Transfer

Where room ownership or management state is required, the departure of the current owner should not necessarily invalidate the complete room.

Conceptually:

    Owner Leaves
         |
         v
    Other Players Remain?
       /            \
     Yes             No
      |               |
      v               v
    Transfer        Cleanup
    Ownership        Room

This reduces unnecessary disruption to other participants.

---

## Safe Spawn Handling

Competitive maps introduced additional edge cases around player spawning.

Potential problems included:

- Unsafe coordinates
- Falling
- Elevated spawn points
- Spawn overlap
- Immediate damage
- Redzone boundaries

The spawn lifecycle therefore required more than teleporting a player to a coordinate.

Conceptually:

    Select Spawn
         |
         v
    Validate Position
         |
         v
    Prepare Player
         |
         v
      Teleport
         |
         v
    Stabilise Position
         |
         v
    Spawn Protection
         |
         v
    Begin Combat

This helped make entry and respawn behaviour more reliable.

---

## Redzone Handling

Some competitive environments required boundary control.

A simplified model is:

    Player Position
          |
          v
    Inside Allowed Zone?
       /           \
     Yes            No
      |              |
      v              v
    Continue      Warning /
                 Recovery
                     |
                     v
                Safe Position

The competitive boundary logic should only operate while the player is inside the relevant TDM context.

It should not remain active after returning to normal RP gameplay.

---

## Interrupted Session Recovery

Multiplayer systems also need to consider abnormal lifecycle events.

Examples include:

- Player disconnect
- Resource restart
- Interrupted match
- Incomplete cleanup

A player should not remain indefinitely trapped in invalid TDM state after an interruption.

Conceptually:

    Player Session Starts
            |
            v
    Previous TDM Context?
        /           \
      No             Yes
       |              |
       v              v
    Normal        Validate /
     Start         Recover
                       |
                       v
                 Safe RP State

Recovery logic provides a path back to a valid gameplay context.

---

## Cleanup Lifecycle

Leaving TDM requires more than changing the player's position.

A simplified cleanup process is:

    Leave TDM
       |
       v
    Remove Match Equipment
       |
       v
    Reset Combat Modifiers
       |
       v
    Clear Team State
       |
       v
    Clear Score / Session State
       |
       v
    Leave Routing Bucket
       |
       v
    Restore Normal Context
       |
       v
    Return to RP

This illustrates why cleanup is an important part of multiplayer state management.

---

## State-Machine View

The player's competitive lifecycle can be represented as:

    RP
     |
     v
    JOINING
     |
     v
    PREPARING
     |
     v
    ACTIVE
     |
     +----------------+
     |                |
     v                v
    ELIMINATED       LEAVING
     |                |
     v                v
    RESPAWNING      CLEANUP
     |                |
     +--> ACTIVE      v
                     RP

Thinking about the system as a state machine helped separate valid transitions from unintended state changes.

---

## Debugging Approach

The TDM system required debugging across multiple multiplayer and gameplay boundaries.

Investigation included:

- Routing bucket assignment
- Room membership
- Team membership
- Player spawning
- Weapon timing
- Ammunition state
- Damage behaviour
- Elimination handling
- EMS separation
- Player departure
- Room deletion
- Ownership transfer
- Redzone behaviour
- Fall recovery
- Disconnect handling
- Resource restart recovery
- Cleanup behaviour

Many problems could not be solved by examining only the visible UI or combat event.

The complete player lifecycle needed to be traced.

---

## Engineering Outcome

The resulting architecture treated TDM as an isolated temporary multiplayer context operating alongside the persistent roleplay environment.

This demonstrates practical work involving:

- Multiplayer session architecture
- Routing-bucket isolation
- State-machine reasoning
- Temporary versus persistent state
- Client/server coordination
- Player lifecycle management
- Competitive scoring
- Equipment lifecycle management
- Spawn management
- Failure recovery
- Cross-resource integration
- Edge-case debugging

---

## Supporting Documentation

See:

- [TDM & Multiplayer Session System](../../docs/tdm-multiplayer-system.md)
- [Engineering Challenges](../engineering-challenges.md)
- [Development Timeline](../development-timeline.md)
- [Engineering Evidence Index](../evidence-index.md)

---

## Evidence Boundary

This document intentionally does not publish:

- Production Lua source
- Server events
- Internal event names
- Security logic
- Resource configuration
- Server credentials
- Player identifiers
- Anti-cheat configuration

The pseudocode describes the engineering architecture and state lifecycle rather than reproducing the private production implementation.
