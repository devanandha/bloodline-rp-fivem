# Bloodline RP — Engineering Evidence Index

## Purpose

This index connects the main engineering claims documented in the Bloodline RP portfolio with the supporting technical case studies and evidence contained in this repository.

The purpose is to make the project easier to review without publishing proprietary production source code, credentials, private player information or security-sensitive configuration.

The evidence should be considered collectively. Individual screenshots, database tables or resource names demonstrate technical context and deployment, but are not presented independently as proof of authorship.

----

# Evidence Map

| Engineering Area | Engineering Contribution | Primary Case Study | Supporting Evidence |
|---|---|---|---|
| Multiplayer Architecture | Isolated competitive sessions, routing, temporary match state and player/room lifecycle management | [TDM & Multiplayer Session System](../docs/tdm-multiplayer-system.md) | [Engineering Challenges](engineering-challenges.md), [Development Timeline](development-timeline.md) |
| Fuel State & Persistence | Persistent jerrycan state, vehicle-associated fuel interaction and server-authoritative economic state | [Fuel & Petroleum Economy](../docs/fuel-petroleum-economy.md) | [Engineering Challenges](engineering-challenges.md), [Development Timeline](development-timeline.md) |
| Petroleum Economy | Connected depot, wholesale supply, fuel businesses, station stock and player refuelling | [Fuel & Petroleum Economy](../docs/fuel-petroleum-economy.md) | [Development Timeline](development-timeline.md), [Deployment Evidence](deployment-evidence.md) |
| Vehicle Lifecycle | Purchasing, ownership, key access, temporary test-drive state, garages, engine and fuel integration | [Vehicle Ecosystem](../docs/vehicle-ecosystem.md) | [Engineering Challenges](engineering-challenges.md), [Development Timeline](development-timeline.md) |
| Emergency Services | Medical workflow, EMS organisation, permissions, finance, service history and vehicle integration | [EMS & Emergency Services](../docs/ems-emergency-services.md) | [Development Timeline](development-timeline.md), [Deployment Evidence](deployment-evidence.md) |
| Character Management | Multiple identities, controlled Slot 2 access, appearance integration, spawn workflow and recovery handling | [Dual Character System](../docs/dual-character-system.md) | [Engineering Challenges](engineering-challenges.md), [Deployment Evidence](deployment-evidence.md) |
| Persistent World Storage | Persistent tent placement, private storage, access control and protected deletion | [Persistent Tent Storage](../docs/persistent-tent-storage.md) | [Engineering Challenges](engineering-challenges.md), [Development Timeline](development-timeline.md) |
| Transaction Integrity | Controlled sequencing of inventory, persistent objects, payment and delivery | [Persistent Tent Storage](../docs/persistent-tent-storage.md), [NPC Marketplace](../docs/interactive-npc-marketplace.md) | [Engineering Challenges](engineering-challenges.md) |
| NPC Commerce | World interaction, temporary basket state, server transaction validation and inventory delivery | [Interactive NPC Marketplace](../docs/interactive-npc-marketplace.md) | [Engineering Challenges](engineering-challenges.md), [Development Timeline](development-timeline.md) |
| Persistent Data | MySQL-backed state supporting multiple Bloodline systems | Multiple case studies | [Deployment Evidence](deployment-evidence.md) |
| Cross-System Integration | Coordination between independently operating gameplay resources and persistent state | Multiple case studies | [Engineering Challenges](engineering-challenges.md), [Development Timeline](development-timeline.md) |

---

# 1. Multiplayer State Isolation

## Engineering Claim

Bloodline TDM was designed as an isolated competitive multiplayer environment operating alongside the normal roleplay server.

The implementation required management of:

- Routing buckets
- Teams
- Match state
- Scores
- Temporary weapons and ammunition
- Spawn state
- Player departure
- Room lifecycle
- Competitive damage behaviour
- Recovery from interrupted sessions

A central engineering requirement was preventing temporary competitive state from incorrectly affecting normal roleplay systems.

## Evidence

- [TDM & Multiplayer Session System](../docs/tdm-multiplayer-system.md)
- [Engineering Challenges](engineering-challenges.md)
- [Development Timeline](development-timeline.md)
- [Deployment Evidence](deployment-evidence.md)

## Future Visual Evidence

Gameplay evidence can demonstrate:

- Team-room workflow
- Public FFA
- TDM interface
- Match score
- Competitive maps
- Temporary equipment
- Normal return from the competitive environment

---

# 2. Persistent Fuel State

## Engineering Claim

The fuel implementation required persistent state to remain synchronised across inventory, vehicle and server systems.

A significant debugging case involved reusable jerrycans.

Remaining fuel needed to follow the inventory item rather than existing only as temporary interaction state.

This required investigation of:

- Inventory metadata
- Remaining capacity
- Item removal and re-addition
- Client/server synchronisation
- Continuous refuelling
- Server-side state handling

## Evidence

- [Fuel & Petroleum Economy](../docs/fuel-petroleum-economy.md)
- [Engineering Challenges](engineering-challenges.md)
- [Development Timeline](development-timeline.md)

## Engineering Significance

This provides evidence of debugging a persistent-state problem rather than only implementing a user-facing fuel interface.

---

# 3. Petroleum Supply Chain

## Engineering Claim

Vehicle fuel was connected to a wider petroleum economy involving:

    Central Depot
         |
         v
    Wholesale Supply
         |
         v
    Fuel Business
         |
         v
    Station Stock
         |
         v
    Player Refuelling
         |
         v
    Vehicle Fuel

The architecture included persistent business, stock, financial and transaction state.

## Evidence

- [Fuel & Petroleum Economy](../docs/fuel-petroleum-economy.md)
- [Development Timeline](development-timeline.md)
- [Deployment Evidence](deployment-evidence.md)

Database evidence includes Bloodline-specific structures associated with fuel-company, station, worker, ledger, sales, job and depot state.

---

# 4. Vehicle Lifecycle Integration

## Engineering Claim

Vehicle functionality was treated as a lifecycle spanning multiple resources rather than as independent scripts.

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

Development required debugging integration boundaries between these systems.

Examples included:

- Purchase/key synchronisation
- Temporary test-drive access
- Ownership validation
- Persistent showroom state
- Garage integration
- Engine behaviour
- Fuel integration

## Evidence

- [Vehicle Ecosystem](../docs/vehicle-ecosystem.md)
- [Engineering Challenges](engineering-challenges.md)
- [Development Timeline](development-timeline.md)

---

# 5. EMS Organisational Persistence

## Engineering Claim

Bloodline EMS combines medical gameplay with organisational and persistent operational state.

The documented system includes:

- CPR and recovery workflows
- EMS staff
- Rank/permission concepts
- Duty state
- Financial accounts
- Service history
- Transactions
- Emergency vehicles
- Garage integration
- Operational infrastructure

## Evidence

- [EMS & Emergency Services](../docs/ems-emergency-services.md)
- [Development Timeline](development-timeline.md)
- [Deployment Evidence](deployment-evidence.md)

Deployment evidence includes database structures such as:

    bloodline_ems_accounts
    bloodline_ems_service_history
    bloodline_ems_staff
    bloodline_ems_transactions

These structures support the documented persistent organisational architecture.

---

# 6. Persistent Character Management

## Engineering Claim

The Dual Character System manages multiple persistent roleplay identities while separating account-level access from character-specific state.

Engineering areas include:

- Two character slots
- Controlled Slot 2 access
- Administrative unlocking
- Character ownership validation
- Character data separation
- Appearance integration
- Spawn selection
- Temporary preview state
- Recovery from missing or invalid appearance state

## Evidence

- [Dual Character System](../docs/dual-character-system.md)
- [Engineering Challenges](engineering-challenges.md)
- [Development Timeline](development-timeline.md)
- [Deployment Evidence](deployment-evidence.md)

Persistent slot evidence includes:

    bloodline_dualcharacter_slots

This supports the documented persistent second-character access model.

---

# 7. Persistent World Storage

## Engineering Claim

The Tent Storage system converts an inventory item into a persistent world object connected to ownership, coordinates, access control and private inventory storage.

A particularly important design requirement was transaction integrity.

The tent item should not be removed before successful placement has been confirmed.

    Inventory Item
          |
          v
    Placement Request
          |
          v
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

Additional engineering work included:

- Placement preview
- Ground/height handling
- One-tent-per-player rules
- PIN access
- Failed-attempt handling
- Private stash integration
- Deletion safeguards
- NUI focus management

## Evidence

- [Persistent Tent Storage](../docs/persistent-tent-storage.md)
- [Engineering Challenges](engineering-challenges.md)
- [Development Timeline](development-timeline.md)

---

# 8. NPC Transaction Integrity

## Engineering Claim

The NPC Marketplace separates temporary user-interface state from authoritative gameplay transactions.

    Product Selection
          |
          v
       Basket
          |
          v
    Purchase Request
          |
          v
    Server Validation
          |
          v
       Payment
          |
          v
    Inventory Delivery
          |
          v
    Handover Sequence

Adding an item to the basket does not itself represent a completed transaction.

Money and inventory state are handled through the controlled purchase workflow.

## Evidence

- [Interactive NPC Marketplace](../docs/interactive-npc-marketplace.md)
- [Engineering Challenges](engineering-challenges.md)
- [Development Timeline](development-timeline.md)

This demonstrates transaction-style reasoning across UI state, player economy and inventory delivery.

---

# 9. Temporary vs Persistent State

A recurring architectural problem across Bloodline RP was deciding which state should survive after an interaction.

Examples include:

| System | Temporary State | Persistent / Longer-Lived State |
|---|---|---|
| TDM | Match weapons, teams, score, routing | Normal RP character state |
| Fuel | Active nozzle/refuelling interaction | Jerrycan metadata, station/depot stock |
| Vehicle Shop | Test-drive access | Purchased vehicle ownership |
| Dual Character | Preview camera/interface | Character and appearance state |
| Tent Storage | Placement preview | Tent ownership/location/storage |
| NPC Marketplace | Basket state | Completed inventory transaction |

Explicit separation between temporary and persistent state became a recurring engineering pattern throughout the project.

---

# 10. Cross-Resource Debugging

Many significant development problems appeared at the boundaries between resources.

Examples include:

- Vehicle purchase completing while key access was incorrect
- Test-drive vehicles triggering normal anti-theft behaviour
- Inventory operations affecting jerrycan state
- Character records existing while appearance state was invalid
- TDM elimination conflicting with normal RP death behaviour
- Tent placement interacting incorrectly with inventory state
- Storage interaction leaving interface focus active
- Marketplace UI state requiring authoritative server transaction handling

These problems required tracing complete state and event lifecycles rather than examining only the visible feature where the problem appeared.

Supporting evidence:

- [Engineering Challenges](engineering-challenges.md)
- [Development Timeline](development-timeline.md)

---

# 11. Deployment Evidence

Bloodline RP reached a working FiveM/QBCore server environment containing Bloodline-specific resources and MySQL-backed systems.

Deployment evidence documents:

- FiveM runtime environment
- txAdmin
- QBCore
- Bloodline-specific resources
- oxmysql
- MySQL
- XAMPP development services
- Database structures
- Persistent system architecture
- Third-party infrastructure

See:

[Deployment Evidence](deployment-evidence.md)

Runtime and database evidence demonstrates deployment and technical scope.

It is not presented independently as proof that every resource visible in the environment was originally authored for Bloodline RP.

---

# 12. Visual Evidence

The repository structure provides dedicated locations for visual evidence:

    demonstrations/
        runtime/
        database/
        tdm/
        fuel/
        vehicles/
        ems/
        characters/
        storage/

Visual evidence will complement the written engineering documentation by demonstrating systems operating within the deployed server environment.

Before publication, screenshots should be checked for:

- IP addresses
- Credentials
- API keys
- FiveM/Cfx licence keys
- Webhooks
- Player identifiers
- Discord identifiers
- Email addresses
- Private database records
- Security-sensitive configuration

---

# Evidence Interpretation

This repository should be evaluated as a combined technical record consisting of:

    Architecture
        +
    Case Studies
        +
    Engineering Challenges
        +
    Development Timeline
        +
    Deployment Evidence
        +
    Demonstrations

No single screenshot, resource name or database table is intended to establish the complete engineering contribution by itself.

The strongest evidence comes from the relationship between documented engineering decisions, debugging history, system architecture, deployed resources and demonstrations of working behaviour.

---

# Third-Party Components

Bloodline RP was developed on top of QBCore and a wider FiveM ecosystem containing third-party frameworks, libraries, resources, maps and assets.

The presence of a third-party component within the server does not represent a claim of original authorship.

This portfolio focuses on Bloodline-specific development, modifications, integrations, debugging, testing and system engineering.

Ownership and licensing of external components remain with their respective authors.

---

# Public Evidence / Private Implementation

The production implementation remains private.

The public repository intentionally focuses on:

- Architecture
- Engineering decisions
- System behaviour
- Integration
- Debugging
- Persistence
- Testing
- Deployment evidence
- Demonstrations

while excluding proprietary production source code and security-sensitive information.

This provides a reviewable engineering record while protecting the production implementation.
