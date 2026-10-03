# Bloodline TDM — Multiplayer Session System

## Overview

Bloodline TDM is a competitive multiplayer system developed for Bloodline RP that operates alongside the main roleplay environment while keeping competitive gameplay isolated from normal RP state.

The system supports private team matches and public free-for-all sessions across multiple locations.

A major engineering objective was ensuring that temporary TDM state — including weapons, ammunition, damage behaviour, teams and routing — did not interfere with normal roleplay systems.

---

## Core Features

- Private team rooms
- Public free-for-all sessions
- Multiple playable environments
- Routing-bucket isolation
- Configurable team sizes
- Host-selected round targets
- Team scoring
- Custom combat rules
- Temporary TDM weapons
- Finite TDM ammunition
- Spawn protection
- Redzone handling
- Fall recovery
- Safe spawn handling
- Temporary team outfits
- Custom NUI interface
- Disconnect handling
- Crash/restart recovery

---

## Session Architecture

    Normal RP Environment
             |
             v
         TDM Entry
             |
             v
       Select Game Mode
             |
       +-----+-----+
       |           |
       v           v
    Team Room   Public FFA
       |           |
       +-----+-----+
             |
             v
       Routing Bucket
             |
             v
      Temporary TDM State
             |
       +-----+-------------------+
       |                         |
       v                         v
    Combat State            Session State
       |                         |
       +-- Weapon                +-- Teams
       +-- Ammunition            +-- Score
       +-- Damage Rules          +-- Round Target
       +-- Armour                +-- Spawn State
       +-- Hit Tracking          +-- Zone State
             |
             v
          TDM Exit
             |
             v
     Restore Normal RP State

---

## Multiplayer Isolation

Routing buckets are used to separate active TDM sessions from the normal roleplay environment and from other competitive sessions.

Private team rooms can therefore operate independently while public FFA environments use dedicated multiplayer instances.

The TDM system was designed so that normal RP systems such as the following are not intentionally modified by a competitive session:

- Player economy
- Jobs
- Garage state
- EMS gameplay
- Normal RP inventory
- Permanent weapon state

This separation was particularly important because TDM and normal roleplay run on the same server.

---

## Custom Combat Model

Standard GTA/QBCore damage behaviour did not provide the exact gameplay behaviour required for Bloodline TDM.

A TDM-specific combat model was therefore introduced.

The system distinguishes between:

- Close-range headshots
- Longer-range headshots
- Body hits

The TDM hit-tracking system determines competitive eliminations while the normal GTA/QBCore death flow is prevented from ending the player prematurely during an active match.

This allowed competitive combat behaviour to remain separate from normal RP injury and EMS mechanics.

---

## Weapon & Ammunition Lifecycle

TDM weapons exist only as temporary competitive-session state.

During development, testing identified that weapons could be issued while the player was still transitioning from the TDM menu into the match.

The lifecycle was revised so that weapon activation occurs only after the relevant session routing, teleport/spawn and world loading stages.

Later revisions also removed infinite ammunition.

Players instead receive a finite TDM ammunition allocation for a spawn or round, allowing the normal ammunition display to decrease correctly.

On TDM exit, competitive weapon state is removed so that it does not become part of the player's normal RP inventory state.

---

## Team Match Management

Private team rooms support configurable team sizes and host-selected match targets.

A later development revision corrected match completion logic so that the selected target represents the number of rounds a team must win.

For example:

    FIRST TO 30

means that the first team reaching 30 round wins completes the match.

The system also provides a compact live team score while retaining access to a more detailed scoreboard.

---

## Player Departure & Room State

Multiplayer room lifecycle management required additional debugging.

An earlier behaviour could allow one player's departure to affect other participants in the room.

The room lifecycle was revised so that:

- Individual players can leave independently.
- Remaining players stay in the session.
- Waiting-room ownership can transfer to another participant.
- A player disconnect does not automatically remove everyone.
- Explicit room deletion remains a separate host action.

This was an important multiplayer state-management correction because player state and room state needed to be treated separately.

---

## Spawn & World Safety

Competitive maps introduced several world-state problems that required additional handling.

Examples included:

- Players spawning at unsafe terrain heights.
- Players appearing before collision data was fully available.
- Terrace players falling from elevated areas.
- Spawn locations placing players near unsuitable map geometry.
- Ambient GTA vehicles and NPCs appearing inside competitive areas.

The system evolved to include:

- Spawn protection
- Ground-position checking
- Collision-loading allowance
- Fall detection
- Fall recovery
- Last-safe-position handling
- Redzone return behaviour
- Ambient world cleanup during TDM

---

## Redzone Handling

Some competitive environments use bounded gameplay areas.

Instead of relying solely on map markers, later revisions introduced an in-world boundary representation.

When a player remains outside a configured gameplay zone beyond the permitted grace period, the system returns them to the valid play area.

Zone handling only becomes active after the player has successfully entered the TDM session, preventing transition states from incorrectly triggering boundary logic.

---

## Crash & Restart Recovery

Competitive session state can become problematic if a resource or client session is interrupted unexpectedly.

Recovery handling was introduced so that players reconnecting after an interrupted TDM state can be returned to the normal routing environment and TDM entry location rather than remaining trapped in an invalid multiplayer instance.

This provides a recovery path for abnormal session termination.

---

## Temporary Team Appearance

Private team matches can apply temporary team-specific clothing to make opposing players visually distinguishable.

The clothing state is associated with the competitive match rather than the player's permanent RP identity.

Normal character appearance is restored when leaving the competitive environment.

---

## User Interface Iteration

The TDM interface went through multiple revisions during development.

Changes included:

- Standardising team and FFA map cards
- Improving map selection
- Adding selected-map information
- Reducing unnecessary empty space
- Refining menu dimensions
- Improving lobby controls
- Adding real in-game map images
- Renaming public maps for clearer identification
- Improving live score presentation

This demonstrates that development continued beyond initial functionality into usability and presentation refinement.

---

## Selected Engineering Challenges

### Challenge 1 — Normal RP Death Interfering With TDM

**Problem**

Normal GTA/QBCore death behaviour could occur before the custom competitive hit rules had completed.

**Resolution**

TDM-specific health and damage handling was introduced so competitive elimination could be controlled independently.

**Result**

TDM combat behaviour became isolated from normal RP death and EMS flows.

---

### Challenge 2 — Weapon Appearing Before Match Entry

**Problem**

A competitive weapon could become available while the player was still at the TDM menu or transitioning into a session.

**Resolution**

Weapon activation was moved later in the session lifecycle, after routing and spawn preparation.

**Result**

Competitive equipment remained associated with an active match rather than the menu environment.

---

### Challenge 3 — One Player Leaving Affected the Room

**Problem**

Player departure and room lifecycle were too closely coupled.

**Resolution**

Individual participant removal was separated from explicit room deletion, with ownership transfer for waiting rooms.

**Result**

Remaining participants could continue without being removed because another player disconnected or left.

---

### Challenge 4 — Unsafe Map Spawns

**Problem**

Some map coordinates could place players at unsuitable heights or before world collision was ready.

**Resolution**

Ground checks, collision handling, safer spawn positions and fall recovery were introduced.

**Result**

Match entry became more reliable across different environments.

---

### Challenge 5 — Recovery From Interrupted Sessions

**Problem**

A resource restart or interrupted client state could leave a player associated with an invalid competitive session.

**Resolution**

Recovery state was introduced to return affected players to the normal routing environment and TDM entry point.

**Result**

The system gained a controlled recovery path instead of relying only on the ideal match-exit sequence.

---

## Development Pattern

The TDM system demonstrates the iterative development approach used throughout Bloodline RP:

    Requirement
        |
        v
    Initial Implementation
        |
        v
    Multiplayer Testing
        |
        v
    Edge Case / Bug Discovery
        |
        v
    Debugging
        |
        v
    System Revision
        |
        v
    Retesting
        |
        v
    Stable Behaviour

Several TDM features evolved through repeated revisions rather than being treated as complete after their first implementation.

---

## Technical Areas Demonstrated

The system provides practical evidence of work involving:

- Lua-based FiveM resource development
- QBCore integration
- Multiplayer routing buckets
- Session-state management
- Temporary versus persistent state
- Cross-resource isolation
- Player lifecycle management
- Custom combat logic
- World-coordinate handling
- Spawn safety
- Failure recovery
- NUI integration
- UI/UX iteration
- Gameplay testing and debugging

---

## Production Source

The complete production implementation is maintained privately.

This public case study documents the architecture, behaviour, engineering decisions and debugging process without distributing proprietary server source code or security-sensitive configuration.

---

## Status

**Bloodline TDM was implemented as a working component of the completed Bloodline RP multiplayer server.**
