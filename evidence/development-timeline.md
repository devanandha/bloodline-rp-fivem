# Bloodline RP — Development Timeline

## Overview

Bloodline RP was developed over approximately three months as a complete FiveM roleplay server built around the QBCore framework.

The project evolved from the initial server environment into an interconnected multiplayer platform containing custom gameplay systems, persistent data, business workflows, vehicle systems, emergency services, character management and competitive multiplayer functionality.

Development was iterative rather than linear.

Individual systems were repeatedly implemented, tested in-game, debugged, revised and then tested again against other resources.

The overall development pattern was:

    Requirement
        |
        v
    Research & System Design
        |
        v
    Initial Implementation
        |
        v
    In-Game Testing
        |
        v
    Bug / Edge Case Discovery
        |
        v
    Debugging
        |
        v
    Revision
        |
        v
    Integration Testing
        |
        v
    Working System

This document summarises the major development phases and engineering progression of the project.

---

# Phase 1 — Server Foundation

The first phase established the technical environment required to build and test Bloodline RP.

The development environment included:

- FiveM
- QBCore
- Lua
- MySQL
- phpMyAdmin
- XAMPP
- Apache
- MySQL database services
- txAdmin
- Third-party FiveM dependencies

The project was organised into resource groups covering areas such as:

- QBCore resources
- Jobs
- Banking
- Clothing
- Maps
- Graphics
- Phone systems
- Radio
- Vehicles
- Standalone dependencies
- Custom Bloodline resources

This provided the base environment on which the custom Bloodline systems could be developed.

---

# Phase 2 — Database & Persistence Foundation

As the project expanded, persistent systems required dedicated database structures.

Bloodline-specific persistent data was introduced for multiple areas including:

- Fuel economy
- Fuel businesses
- Fuel company operations
- EMS
- Vehicle-related systems
- Character slots
- Electronics/business systems
- Player sales
- Staff records
- Transaction records
- Persistent world systems

The database architecture eventually contained dedicated Bloodline tables such as:

    bloodline_dualcharacter_slots

    bloodline_ems_accounts
    bloodline_ems_service_history
    bloodline_ems_staff
    bloodline_ems_transactions

    bloodline_fuelcompany
    bloodline_fuelcompany_ledger
    bloodline_fuelcompany_staff

    bloodline_fuel_business_logs
    bloodline_fuel_player_sales
    bloodline_fuel_refill_jobs
    bloodline_fuel_stations

alongside other server persistence structures.

The introduction of persistent state changed the project from a collection of temporary gameplay interactions into systems capable of retaining operational state across sessions.

---

# Phase 3 — Fuel System Development

One of the largest development areas was the Bloodline fuel ecosystem.

Initial work focused on vehicle refuelling behaviour.

The system expanded to include:

- Multiple fuel grades
- Vehicle tank capacity
- Vehicle fuel consumption
- Pump interaction
- Fuel nozzle behaviour
- Hose behaviour
- Fuel-cap targeting
- Jerrycan support
- Inventory integration

Development then moved beyond basic vehicle refuelling into a broader petroleum economy.

---

# Phase 4 — Fuel Interaction Debugging

Fuel interaction required repeated in-game testing.

One challenge involved identifying appropriate fuel interaction positions across different vehicle models.

A fixed interaction approach was not reliable because vehicles can have different fuel-cap positions.

The interaction model was therefore revised toward dynamic vehicle-associated fuel-cap targeting.

Testing also required comparing different targeting approaches and correcting interaction behaviour around the vehicle.

---

# Phase 5 — Jerrycan Persistence

The jerrycan system became one of the most difficult individual development areas.

Approximately a week of development and debugging was spent investigating fuel-container behaviour and related edge cases.

The intended model was:

    100 litres = 100 durability

The remaining fuel therefore needed to decrease as the container was used.

Testing exposed problems involving:

- Capacity
- Durability
- Inventory metadata
- Item removal
- Item re-addition
- Inventory slots
- Continuous usage
- State persistence

A particularly important issue was preventing inventory movement from resetting a partially used jerrycan.

The implementation evolved toward server-authoritative state and persistent item metadata.

This prevented a normal inventory operation from effectively recreating a full container.

---

# Phase 6 — Petroleum Economy Expansion

The vehicle fuel mechanic was expanded into a connected economic system.

The architecture evolved into:

    Fuel Company
         |
         v
    Central Depot
         |
         v
    Wholesale Supply
         |
         v
    Fuel Businesses
         |
         v
    Fuel Stations
         |
         v
    Player Refuelling
         |
         v
    Vehicle Fuel

The system incorporated concepts including:

- Central depot stock
- Company funds
- Wholesale pricing
- Fuel-station inventory
- Staff
- Managers
- Deliveries
- Business logs
- Player sales
- Refill jobs

This transformed fuel from a vehicle mechanic into a persistent supply-chain system.

---

# Phase 7 — Vehicle Ecosystem Development

Vehicle systems were developed and integrated around a common ownership lifecycle.

The major workflow became:

    Vehicle Shop
         |
         v
      Purchase
         |
         v
      Ownership
         |
       +-+-+
       |   |
       v   v
     Keys Garage
       |   |
       +-+-+
         |
         v
      Engine
         |
         v
       Fuel

Work included:

- Vehicle dealership
- Vehicle purchasing
- Bank payment
- Persistent ownership
- Test drives
- Showroom displays
- Vehicle keys
- Garages
- Engine behaviour
- Fuel integration

---

# Phase 8 — Vehicle Integration Debugging

Vehicle development exposed several cross-resource problems.

One involved successfully purchasing a vehicle without immediately obtaining the expected key access.

The purchase workflow was revised so ownership and key state were coordinated.

Another issue involved test-drive vehicles triggering normal key or anti-theft behaviour.

Temporary test-drive authorisation was therefore distinguished from normal permanent ownership.

Further revisions included:

- Persistent showroom displays
- UI prompt handling
- NUI interaction fixes
- Vehicle-image optimisation
- Test-drive state handling

These issues demonstrated that vehicle ownership could not be implemented reliably by modifying only one resource.

---

# Phase 9 — EMS & Emergency Services

The EMS system expanded the server into structured emergency-service gameplay.

Development included:

- CPR
- Revival
- Treatment
- Duty management
- EMS staff
- Staff ranks
- Permissions
- Service payments
- EMS accounts
- Transaction history
- Service history
- Night-duty bonuses
- Uniforms
- EMS vehicles
- EMS garage
- Lift access
- Helipad access
- Administrative functionality

Persistent database structures were introduced for EMS organisational and financial state.

This system required coordination between player state, staff permissions, finance, vehicles and persistent records.

---

# Phase 10 — Dual Character System

A custom dual-character workflow was developed to allow multiple persistent roleplay identities.

The system introduced:

- Two character slots
- Primary character access
- Controlled second-character access
- Administrator-controlled Slot 2 unlocking
- Character ownership validation
- Persistent slot state
- Character selection
- Character previews
- Appearance loading
- Spawn selection

A dedicated persistent slot structure was introduced through:

    bloodline_dualcharacter_slots

The system required careful separation between account-level state and character-level state.

---

# Phase 11 — Character Appearance Debugging

Character appearance became an important debugging area.

Testing exposed situations where a valid character could exist while the expected appearance was not restored correctly.

Investigation covered:

- Character creation
- Character identifiers
- Appearance records
- Default skin state
- Character loading
- Spawn flow

The workflow was revised to improve default appearance handling and provide recovery behaviour for invalid or missing appearance state.

This was an example of debugging across multiple resources rather than treating the visible appearance problem as an isolated UI issue.

---

# Phase 12 — Persistent Tent Storage

A persistent placeable storage system was developed.

The system included:

- Tent item
- Placement preview
- Placement validation
- Ground handling
- Persistent world position
- One-tent-per-player rules
- Private stash
- PIN access
- Failed-attempt handling
- Removal safeguards
- Administrative permissions

The storage configuration supported:

    50 slots
    500 kg configured capacity

Development required coordinating inventory state with world-object state.

---

# Phase 13 — Tent Placement & Storage Debugging

Testing exposed several placement and storage issues.

These included:

- Preview height
- Final object height
- Ground positioning
- Inventory item removal timing
- Storage opening
- NUI focus

The workflow was revised so the tent item was removed only after successful placement.

This reduced the risk of losing an item because a placement attempt failed.

Deletion handling also considered whether the associated stash still contained items before permanent removal.

PIN validation included server-side failed-attempt handling.

---

# Phase 14 — Interactive NPC Marketplace

An NPC-based marketplace workflow was developed as another interactive system.

The system included:

- NPC interaction
- Product selection
- Product pricing
- Basket state
- Cash transactions
- Inventory delivery
- Handover interactions
- Management functionality
- Administrative controls
- NPC placement
- Cross-resource integration

The marketplace required coordination between temporary interface state and authoritative money/inventory state.

---

# Phase 15 — NPC & Transaction Iteration

Marketplace development continued through repeated in-game revisions.

Testing covered:

- NPC coordinates
- NPC heading
- Interaction position
- Marketplace access
- Basket behaviour
- Payment flow
- Inventory delivery
- Handover scenes
- World placement
- Other resource interactions

This demonstrated the difference between a feature being technically functional and being reliable within the complete game environment.

---

# Phase 16 — TDM Multiplayer System

A major later development area was the Bloodline TDM multiplayer system.

The objective was to create competitive gameplay inside the same server while isolating it from normal roleplay state.

The system developed support for:

- Private team rooms
- Public free-for-all
- Multiple maps
- Routing buckets
- Team management
- Round targets
- Team scoring
- Custom combat rules
- Temporary weapons
- Finite ammunition
- Spawn protection
- Redzones
- Fall recovery
- Team clothing
- Custom NUI
- Disconnect handling
- Restart recovery

---

# Phase 17 — TDM State Isolation

One of the major engineering requirements was ensuring that competitive gameplay did not interfere with normal roleplay.

The architecture separated:

    Normal RP State

from:

    Temporary TDM State

Competitive state included:

- Temporary weapons
- Temporary ammunition
- Teams
- Score
- TDM health
- TDM damage behaviour
- Routing buckets
- Spawn state
- Zone state

Normal RP systems such as jobs, economy, garage state and EMS were intentionally kept outside the competitive session lifecycle.

---

# Phase 18 — TDM Combat Debugging

Standard GTA/QBCore death behaviour did not provide the required competitive behaviour.

Testing exposed conflicts between normal death handling and TDM-specific hit logic.

The TDM combat model was therefore revised to manage competitive elimination separately.

Development also included:

- Headshot/body-hit handling
- Health buffering
- Damage modifiers
- Weapon-state management
- Respawn behaviour
- Finite ammunition

This prevented normal EMS/death workflows from controlling competitive eliminations.

---

# Phase 19 — TDM Multiplayer Edge Cases

Multiplayer testing exposed additional lifecycle problems.

Examples included:

- Weapons appearing before match entry
- Unsafe map spawns
- Collision loading
- Players falling from elevated locations
- Redzone handling
- Ambient NPCs and vehicles
- One player leaving affecting other participants
- Room ownership
- Resource restart recovery

The system was revised so that individual player departure and room lifecycle were treated separately.

Waiting-room ownership could transfer rather than automatically removing all participants.

Recovery state was also introduced for interrupted sessions.

---

# Phase 20 — User Interface & Experience Refinement

Several systems underwent UI and usability revisions after their underlying functionality was working.

Examples included:

- TDM map selection
- TDM lobby controls
- Live score presentation
- Real map imagery
- Vehicle-shop UI
- Vehicle image optimisation
- Tent placement preview
- Storage interface behaviour
- Character selection
- Marketplace interaction

This stage represented refinement rather than only initial feature creation.

---

# Phase 21 — Cross-System Integration

As Bloodline RP expanded, individual systems increasingly needed to operate together.

Examples include:

    Vehicle Shop
        |
        v
    Vehicle Keys
        |
        v
      Garage
        |
        v
      Engine
        |
        v
       Fuel

and:

    Fuel Company
        |
        v
    Fuel Business
        |
        v
    Fuel Station
        |
        v
    Player Vehicle

and:

    EMS Staff
        |
        v
    EMS Garage
        |
        v
    Vehicle Access

and:

    Character Selection
        |
        v
    Appearance
        |
        v
    Spawn
        |
        v
    Active RP Systems

Cross-resource testing became increasingly important because a change that solved one system could affect another.

---

# Phase 22 — Deployment & Live Server Testing

The project progressed from local development into a working FiveM server environment.

Deployment evidence reviewed during project documentation included txAdmin showing Bloodline resources loading in the server environment.

Examples included:

- Bloodline fuel resources
- Bloodline vehicle systems
- Bloodline shop systems
- Bloodline customs systems
- Database connectivity through oxmysql
- QBCore resources
- Third-party infrastructure dependencies

The development environment also included local Apache and MySQL services used during server development and database administration.

---

# Phase 23 — Community Use

Bloodline RP progressed beyond an isolated development exercise.

The project was associated with an online roleplay community containing more than 150 Discord members and approximately 50 whitelist applications.

This provided a real community context for the server rather than the project existing only as a local technical demonstration.

Community activity also created practical requirements around:

- Reliability
- Player onboarding
- Persistent data
- Gameplay usability
- Administrative functionality
- Multiplayer behaviour

---

# Engineering Progression

Across approximately three months, the project progressed through several levels of complexity.

    Server Setup
        |
        v
    Individual Features
        |
        v
    Persistent Systems
        |
        v
    Business Logic
        |
        v
    Cross-System Integration
        |
        v
    Multiplayer State
        |
        v
    Edge-Case Handling
        |
        v
    Deployment
        |
        v
    Community Use

The later stages of the project increasingly focused on state management, persistence, recovery and integration rather than simply adding new features.

---

# Development Method

Development involved a combination of:

- Requirement definition
- Technical research
- Reviewing relevant documentation and examples
- Implementation
- Lua development
- Database work
- Configuration
- Integration
- In-game testing
- Debugging
- Iterative revision

When an implementation did not behave as intended, the problem was investigated through testing, external technical references where useful, comparison with existing FiveM/QBCore patterns, code revision and further testing.

The engineering contribution therefore extended beyond initial implementation into integration, debugging and operational refinement.

---

# Technical Scope Demonstrated

Across the project, Bloodline RP demonstrates practical experience involving:

- FiveM server development
- Lua
- QBCore
- MySQL
- phpMyAdmin
- Client/server architecture
- Persistent state
- Inventory integration
- Vehicle state
- Multiplayer routing buckets
- Session management
- Database-backed business logic
- Role-based permissions
- Player lifecycle management
- NPC interaction
- NUI
- World coordinates
- Object placement
- Transaction integrity
- Failure recovery
- Cross-resource integration
- In-game testing
- Iterative debugging

---

# Evidence Boundaries

This repository documents the engineering process without publicly distributing the complete production implementation.

The following remain private:

- Production source code
- Credentials
- Database passwords
- API keys
- FiveM/Cfx keys
- Webhooks
- Security configuration
- Player identifiers
- Private player records
- Production database contents

Third-party resources, frameworks and infrastructure are not presented as original Bloodline RP development.

Where Bloodline RP integrates or modifies external components, the public documentation focuses on the integration and engineering work performed for the project.

---

# Current Status

**Bloodline RP reached a completed working-server stage after approximately three months of iterative development, testing, debugging and integration.**

The resulting project included interconnected systems covering:

- Competitive multiplayer
- Petroleum economy
- Vehicle lifecycle
- Emergency services
- Multiple characters
- Persistent world storage
- NPC commerce
- Business systems
- Player-facing interfaces
- Database-backed persistence

The development timeline demonstrates the progression from server foundation to interconnected persistent multiplayer systems rather than presenting the final implementation without its engineering history.
