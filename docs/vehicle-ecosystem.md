# Bloodline Vehicle Ecosystem

## Overview

The Bloodline Vehicle Ecosystem is a collection of interconnected vehicle-management systems developed and integrated for Bloodline RP.

Rather than treating vehicle purchasing, ownership, keys, garages, engine control and fuel as independent mechanics, the system was designed around a shared vehicle lifecycle.

A typical vehicle can move through the following lifecycle:

    Vehicle Dealership
           |
           v
       Purchase
           |
           v
    Ownership Record
           |
      +----+----+
      |         |
      v         v
     Keys     Garage
      |         |
      +----+----+
           |
           v
    Vehicle Operation
           |
      +----+----+
      |         |
      v         v
    Engine     Fuel
      |         |
      +----+----+
           |
           v
       Player Use

This required coordination between QBCore vehicle data, database persistence, inventory state, vehicle entities, player ownership and several custom Bloodline resources.

---

## Core Systems

The vehicle ecosystem includes:

- Vehicle dealership
- Vehicle purchasing
- Persistent vehicle ownership
- Test drives
- Showroom vehicle displays
- Vehicle keys
- Vehicle locking
- Engine access
- Garage storage
- Public garages
- Job-specific garages
- Vehicle retrieval
- Vehicle return handling
- Fuel integration
- Vehicle state persistence

The objective was to provide a consistent lifecycle from vehicle purchase through everyday use.

---

## System Architecture

    Vehicle Dealership
            |
            v
       Vehicle Selection
            |
       +----+----+
       |         |
       v         v
    Test Drive  Purchase
                    |
                    v
             Payment Validation
                    |
                    v
              Ownership Record
                    |
              +-----+-----+
              |           |
              v           v
         Vehicle Keys   Garage State
              |           |
              +-----+-----+
                    |
                    v
              Vehicle Access
                    |
             +------+------+
             |             |
             v             v
         Engine State   Fuel State
             |             |
             +------+------+
                    |
                    v
               Player Use

---

## Vehicle Dealership

A custom vehicle-shop workflow was developed for Bloodline RP.

The dealership handles the transition between browsing a vehicle and creating persistent player ownership.

Key functionality includes:

- Vehicle browsing
- Showroom displays
- Vehicle purchasing
- Bank-based payment
- Test drives
- Persistent ownership creation
- Key assignment
- Administrative controls
- Dealership management
- User-interface interactions

The purchase process required coordination between the user interface, payment state, vehicle database records and key system.

---

## Persistent Vehicle Ownership

Purchasing a vehicle is not only a client-side spawn event.

A successful purchase must create a persistent ownership relationship between the player and vehicle.

The lifecycle can be represented as:

    Player Selects Vehicle
              |
              v
       Purchase Request
              |
              v
       Payment Validation
              |
              v
       Vehicle Record Created
              |
              v
        Ownership Assigned
              |
              v
           Key Access
              |
              v
        Persistent Vehicle

Vehicle ownership data integrates with the wider QBCore vehicle environment and persistent player vehicle records.

This allows other systems, particularly garages and vehicle keys, to recognise the same vehicle as player-owned.

---

## Vehicle Key System

Vehicle access is controlled through a key system associated with vehicle ownership.

The system supports concepts such as:

- Vehicle key items
- Ownership-based access
- Locking and unlocking
- Engine access restrictions
- Test-drive access
- Job-specific access
- Plate normalisation
- Temporary access scenarios

Key state must remain consistent with the vehicle's ownership state.

This became particularly important during vehicle purchases and test drives.

---

## Purchase-Key Integration Challenge

### Problem

During development, a purchased vehicle could be successfully created while the player's key access did not immediately match the new ownership state.

This created a cross-system problem:

    Vehicle Purchase = Successful

but:

    Vehicle Access = Incorrect

### Investigation

The purchase flow was reviewed across:

- Vehicle creation
- Ownership persistence
- Vehicle plate information
- Key-item handling
- Key-resource integration
- Immediate vehicle access

### Resolution

The purchase lifecycle was revised so that the correct vehicle-key state was created alongside the ownership process.

### Result

A successful vehicle purchase could transition directly into usable vehicle ownership rather than requiring a separate workaround to gain access.

---

## Test Drive Lifecycle

Test drives required a different lifecycle from permanently owned vehicles.

A test-drive vehicle should be usable temporarily without creating normal ownership.

The intended flow is:

    Dealership
        |
        v
    Start Test Drive
        |
        v
    Temporary Vehicle
        |
        v
    Temporary Access
        |
        v
    Test Period
        |
        v
    End Test Drive
        |
        v
    Remove Temporary State

This distinction was necessary because permanent ownership rules should not apply to temporary dealership vehicles.

---

## Test Drive Access Challenge

### Problem

Normal key and anti-theft behaviour could interfere with dealership test-drive vehicles.

A player participating in a legitimate test drive could be treated as if they did not have valid vehicle access.

This could trigger behaviour associated with:

- Missing keys
- Hotwire logic
- Engine restrictions

### Resolution

The vehicle-access lifecycle was revised so that active test-drive vehicles could be recognised as temporary authorised vehicles.

### Result

Test drives became isolated from normal ownership and anti-theft rules while still allowing normal rules to apply outside the test-drive context.

---

## Persistent Showroom Vehicles

Showroom vehicles represent another form of vehicle state.

Unlike normal player vehicles, they are presentation entities associated with the dealership environment.

Development revisions introduced persistent showroom-display handling so dealership vehicles could remain associated with configured display positions rather than behaving like ordinary temporary world vehicles.

This required separating:

- Display vehicles
- Test-drive vehicles
- Purchased vehicles
- Normal player-owned vehicles

Each category has a different lifecycle and persistence requirement.

---

## Vehicle Garage System

The garage layer manages storage and retrieval of owned vehicles.

The system was designed so that garage interaction is connected to persistent vehicle ownership.

Functionality includes:

- Player-owned vehicle storage
- Vehicle retrieval
- Multiple public garage locations
- Job-specific garages
- Police vehicle access
- EMS vehicle access
- Vehicle return behaviour
- Storage-state handling
- Retrieval-related fees where applicable

The garage system therefore acts as a persistence layer between active world vehicles and stored ownership records.

---

## Ownership Validation

An important garage requirement is preventing arbitrary vehicles from being stored as player-owned vehicles.

The garage workflow therefore needs to distinguish between:

    Player-Owned Vehicle

and:

    Unowned / Temporary / External Vehicle

Ownership validation helps maintain consistency between the physical vehicle in the game world and the persistent ownership record.

---

## Job-Specific Vehicles

The vehicle ecosystem also supports specialised access patterns for job-related vehicles.

Examples include:

- Police garages
- EMS garages

These vehicles operate under different access rules from normal privately owned vehicles.

This required the ecosystem to support multiple vehicle-access models rather than assuming that every usable vehicle must be personally purchased by the player.

---

## Engine Control

Vehicle engine behaviour is integrated with vehicle-access and vehicle-state logic.

The engine system includes handling for:

- Engine state
- Vehicle access
- Fuel availability
- Vehicle health conditions

This allows the engine to respond to other systems rather than functioning as an isolated toggle.

For example:

    Player Attempts Engine Start
                |
                v
         Access Validation
                |
                v
           Fuel State
                |
                v
        Vehicle Condition
                |
                v
           Engine State

---

## Fuel Integration

The Vehicle Ecosystem connects directly with the Bloodline Fuel & Petroleum Economy.

Vehicle operation therefore participates in a larger lifecycle:

    Vehicle Purchase
          |
          v
       Ownership
          |
          v
        Garage
          |
          v
      Vehicle Use
          |
          v
    Fuel Consumption
          |
          v
       Refuelling
          |
          v
    Continued Operation

This cross-system relationship means fuel state and vehicle state must remain compatible.

The fuel ecosystem is documented separately in:

`fuel-petroleum-economy.md`

---

## User Interface Development

The vehicle-shop interface went through multiple development revisions.

Testing identified issues that were not purely visual but affected interaction state.

One example involved UI prompt behaviour becoming stuck or remaining active incorrectly.

Resolving this required considering the relationship between:

- NUI state
- Player interaction
- Vehicle-shop state
- Focus handling
- Resource callbacks

UI development therefore formed part of the system engineering rather than being treated only as presentation work.

---

## UI Performance

Vehicle dealership interfaces can contain numerous vehicle images and other visual assets.

During development, image handling was also refined, including the use of optimised WebP assets.

This helped reduce unnecessary asset overhead while maintaining the visual dealership experience.

---

## Cross-System Integration

The vehicle ecosystem depends on several systems remaining synchronised.

For example:

    Vehicle Purchase
          |
          +--> Player Payment
          |
          +--> Ownership Record
          |
          +--> Vehicle Plate
          |
          +--> Vehicle Keys
          |
          +--> Garage Recognition

Another lifecycle is:

    Vehicle Entry
          |
          v
      Key Validation
          |
          v
      Engine Access
          |
          v
       Fuel State
          |
          v
    Vehicle Operation

Failures at one integration point can therefore appear as problems in another system.

This made cross-resource debugging an important part of development.

---

## Selected Engineering Challenges

### Challenge 1 — Purchase Completed but Key Access Was Incorrect

**Problem**

Vehicle ownership and key state were not always immediately synchronised after purchase.

**Resolution**

The purchase flow was revised to connect the newly created ownership state with the correct key-access process.

**Result**

Purchased vehicles could transition directly into usable player-owned vehicles.

---

### Challenge 2 — Test Drives Triggering Normal Access Rules

**Problem**

Temporary dealership vehicles could be treated like ordinary vehicles without valid ownership.

**Resolution**

Test-drive state was incorporated into vehicle-access validation.

**Result**

Temporary authorised access could operate without weakening normal vehicle ownership rules.

---

### Challenge 3 — Showroom Vehicle Persistence

**Problem**

Dealership display vehicles require a different lifecycle from normal spawned vehicles.

**Resolution**

Showroom display state was separated from test-drive and player-owned vehicle state.

**Result**

Dealership displays could remain associated with their configured showroom environment.

---

### Challenge 4 — UI Interaction State

**Problem**

Vehicle-shop UI interaction could leave prompts or interface focus in an incorrect state.

**Resolution**

The interaction lifecycle and NUI focus behaviour were revised and retested.

**Result**

Players could enter and leave dealership interactions more reliably.

---

### Challenge 5 — Multiple Vehicle States

**Problem**

The server contains several fundamentally different types of vehicle state:

- Showroom
- Test drive
- Player owned
- Stored
- Job vehicle
- Active world vehicle

Treating all vehicles identically would create ownership and access conflicts.

**Resolution**

Vehicle lifecycle logic was separated according to the purpose and state of each vehicle.

**Result**

The ecosystem could support dealership, ownership, garage and job workflows while maintaining different access rules.

---

## Persistent Data

The vehicle ecosystem relies on persistent database-backed ownership and state.

Persistent records allow vehicle-related systems to recognise information such as:

- Vehicle ownership
- Vehicle identity
- Plate information
- Storage state
- Associated player

The public repository documents this architecture without publishing production player records or private database contents.

---

## Development Approach

The Vehicle Ecosystem was developed through iterative implementation and in-game testing.

A typical development cycle was:

    Requirement
        |
        v
    Implementation
        |
        v
    In-Game Test
        |
        v
    Integration Problem
        |
        v
    Debugging
        |
        v
    Resource Revision
        |
        v
    Retesting
        |
        v
    Working Lifecycle

This was particularly important because many vehicle problems occurred at the boundaries between separate systems rather than entirely within one resource.

---

## Technical Areas Demonstrated

The Vehicle Ecosystem provides evidence of practical work involving:

- Lua-based FiveM development
- QBCore integration
- Vehicle entity management
- Persistent ownership
- MySQL-backed vehicle data
- Inventory integration
- Vehicle key management
- State validation
- Temporary versus persistent state
- NUI integration
- UI state management
- Asset optimisation
- Garage logic
- Engine-state management
- Fuel-system integration
- Cross-resource debugging
- Gameplay testing
- Edge-case handling

---

## Third-Party Components

Bloodline RP uses QBCore and other third-party FiveM resources as part of its underlying platform.

Third-party components are not presented as original Bloodline RP development.

The engineering work documented here focuses on the custom Bloodline vehicle systems, modifications, integrations, state management and debugging performed to create the complete vehicle lifecycle.

---

## Production Source

The complete production implementation is maintained privately.

This public case study documents the architecture, functionality, development process and engineering challenges without distributing proprietary production source code, private configuration or security-sensitive implementation details.

---

## Status

**The Bloodline Vehicle Ecosystem was implemented as part of the completed Bloodline RP server, connecting dealership purchasing, persistent ownership, vehicle access, garages, engine behaviour and fuel into a shared vehicle lifecycle.**
