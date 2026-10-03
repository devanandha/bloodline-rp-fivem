# Bloodline RP — FiveM Roleplay Server Engineering Project

> A fully integrated FiveM/QBCore roleplay server developed over approximately three months, combining persistent multiplayer systems, vehicle infrastructure, an interconnected petroleum economy, emergency services, character management, persistent storage, business systems and isolated competitive multiplayer.

## Technical Architecture

![Bloodline RP Technical Architecture](Bloodline%20RP%20Technical%20Architecture.png)

The diagram summarises the project's layered architecture from the FiveM player client through Bloodline RP gameplay resources and shared framework integrations to server-side persistence. Detailed architecture and evidence are documented throughout this repository.

---

## Project Overview

**Bloodline RP** is a functioning FiveM roleplay server developed around the QBCore framework.

The project grew from a server environment into an interconnected multiplayer platform containing custom-developed systems, modified integrations, persistent MySQL-backed state and third-party FiveM infrastructure.

Development involved much more than configuring individual resources. Systems had to operate together across vehicle ownership, inventory, characters, businesses, multiplayer sessions, persistence, permissions and failure recovery.

### Project Context

- **Development period:** approximately 3 months
- **Platform:** FiveM
- **Framework:** QBCore
- **Primary development:** Lua
- **Database:** MySQL
- **Database integration:** oxmysql
- **Administration/runtime:** txAdmin
- **Database administration:** phpMyAdmin
- **Community:** 219 Discord members
- **Whitelist interest:** approximately 200 applications

---

## Key Outcomes

- Developed and integrated a functioning FiveM/QBCore roleplay environment over approximately three months.
- Engineered and debugged interconnected multiplayer systems spanning vehicles, fuel, EMS, character management, persistent storage, commerce and isolated TDM gameplay.
- Implemented MySQL-backed persistence and cross-resource workflows using Lua, QBCore and oxmysql.
- Worked across temporary and persistent state, transaction integrity, multiplayer session isolation, permissions, recovery and in-game edge cases.
- Progressed the project beyond local feature development into a community-facing server environment with **219 Discord members** and **approximately 200 whitelist applications**.
- Preserved the production implementation privately while documenting architecture, technical case studies, debugging evidence and sanitised deployment/database evidence in this portfolio.

For reviewers, the fastest route through the project is the **Engineering Evidence Index**, followed by the individual technical case studies and visual demonstrations.

---

## Portfolio Navigation

| Review Area | Evidence |
|---|---|
| System overview | [Technical Architecture](docs/architecture.md) |
| End-to-end player experience | [Player Journey & Integrated Gameplay Loop](evidence/player-journey.md) |
| Reviewer evidence map | [Engineering Evidence Index](evidence/evidence-index.md) |
| Development progression | [Development Timeline](evidence/development-timeline.md) |
| Engineering/debugging case studies | [Engineering Challenges](evidence/engineering-challenges.md) |
| Runtime/deployment context | [Deployment Evidence](evidence/deployment-evidence.md) |
| Database design | [Database Architecture](evidence/database/database-architecture.md) |
| Sanitised persistence evidence | [Schema Inventory](evidence/database/schema-inventory.md) |
| Community/operational context | [Community Adoption](evidence/community-adoption.md) |
| Visual evidence | [Demonstrations](demonstrations/) |

---

# What I Worked On

My work on Bloodline RP covered the design, development, configuration, integration, testing and debugging of the server and its interconnected systems.

Key engineering areas included:

- Lua-based FiveM development
- QBCore integration
- Client/server event architecture
- MySQL-backed persistence
- Multiplayer session state
- Routing buckets
- Vehicle ownership and access
- Inventory metadata
- Business and transaction logic
- Character lifecycle management
- EMS workflows
- NUI integration
- NPC interactions
- Persistent world objects
- Permissions and administration
- Failure recovery
- Cross-resource debugging
- In-game testing and iterative refinement

The server also uses third-party frameworks, resources, maps, assets and infrastructure. These are distinguished from Bloodline-specific engineering throughout this repository.

---

# Featured Engineering Systems

## 1. TDM & Multiplayer Session System

An isolated competitive multiplayer environment operating inside the wider roleplay server.

Features include:

- Private team rooms
- Public free-for-all sessions
- Routing-bucket isolation
- Configurable team matches
- Team scoring
- Temporary TDM weapons
- Finite ammunition
- Custom combat behaviour
- Spawn protection
- Redzone handling
- Fall recovery
- Temporary team outfits
- Custom NUI
- Disconnect handling
- Room ownership transfer
- Restart/crash recovery

A major architectural requirement was preventing temporary TDM state from interfering with normal roleplay inventory, economy, character and EMS state.

**Technical case study:**  
[TDM & Multiplayer Session System](docs/tdm-multiplayer-system.md)

---

## 2. Fuel & Petroleum Economy

What began as vehicle refuelling developed into an interconnected petroleum economy.

Architecture:

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
    Vehicles

Features include:

- Multiple fuel grades
- Vehicle fuel consumption
- Vehicle tank capacities
- Dynamic fuel-cap interaction
- Nozzle/hose behaviour
- Persistent jerrycan capacity
- Fuel businesses
- Company management
- Central depot stock
- Wholesale pricing
- Staff permissions
- Delivery workflows
- Sales and operational records

One of the most significant debugging tasks involved maintaining jerrycan fuel state through inventory metadata and preventing state resets through normal inventory operations.

**Technical case study:**  
[Fuel & Petroleum Economy](docs/fuel-petroleum-economy.md)

---

## 3. Vehicle Ecosystem

The vehicle architecture connects purchasing, ownership, access, storage, engine behaviour and fuel.

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

Features include:

- Vehicle purchasing
- Bank payments
- Persistent ownership
- Vehicle keys
- Test drives
- Temporary test-drive authorisation
- Persistent showroom displays
- Public garages
- Job-specific vehicles
- Engine control
- Fuel integration
- NUI workflows

**Technical case study:**  
[Vehicle Ecosystem](docs/vehicle-ecosystem.md)

---

## 4. EMS & Emergency Services

A structured emergency-services system connecting medical gameplay with staff, financial and vehicle systems.

Features include:

- CPR
- Revival
- Treatment
- Duty management
- Staff ranks
- Permissions
- Service payments
- EMS financial accounts
- Transaction history
- Service history
- Night-duty bonuses
- Uniforms
- EMS vehicles
- Garage integration
- Lift/helipad access
- Administrative functionality

The EMS architecture is intentionally separated from competitive TDM elimination state.

**Technical case study:**  
[EMS & Emergency Services](docs/ems-emergency-services.md)

---

## 5. Dual Character System

A persistent character-management workflow supporting multiple roleplay identities.

Features include:

- Two character slots
- Controlled second-slot access
- Administrative unlocking
- Ownership validation
- Persistent slot state
- Character selection
- Cinematic previews
- Appearance integration
- Spawn selection
- Appearance recovery

The system required separation between account-level access state and character-specific persistent state.

**Technical case study:**  
[Dual Character System](docs/dual-character-system.md)

---

## 6. Persistent Tent Storage

A persistent placeable storage system combining inventory state, world objects and access control.

Features include:

- Placeable tents
- Placement preview
- Ground/height validation
- Persistent coordinates
- One-tent-per-player control
- Private storage
- 50 configured slots
- 500 kg configured storage capacity
- PIN access
- Failed-attempt handling
- Removal safeguards
- Administrative controls

A key engineering requirement was ensuring that inventory items were not consumed until successful world placement had been confirmed.

**Technical case study:**  
[Persistent Tent Storage](docs/persistent-tent-storage.md)

---

## 7. Interactive NPC Marketplace

An interactive NPC commerce workflow connecting world interaction, basket state, payments and inventory delivery.

Features include:

- NPC interaction
- Product selection
- Basket state
- Cash transactions
- Server-side validation
- Inventory delivery
- Handover interactions
- Management controls
- Administrative controls
- World placement and interaction refinement

**Technical case study:**  
[Interactive NPC Marketplace](docs/interactive-npc-marketplace.md)

---

# Technical Architecture

At a high level:

    Players
       |
       v
    FiveM Client
       |
       v
    Bloodline Resources
       |
       +----------------------+
       |                      |
       v                      v
    QBCore              FiveM / External
    Framework             Dependencies
       |
       v
    Server-Side Logic
       |
       v
     oxmysql
       |
       v
      MySQL

The project contains both temporary gameplay state and persistent database-backed state.

Examples of persistent state include:

- Character-slot access
- Vehicle ownership
- EMS staff and transactions
- Fuel-company state
- Fuel-business state
- Station stock
- Business records
- Persistent storage

[View the complete architecture documentation](docs/architecture.md)

---

# Engineering Evidence

The repository is designed to document not only **what the server contains**, but also **how it was developed and debugged**.
## Engineering Evidence Index

A reviewer-focused map connecting the project's main engineering claims to the relevant case studies, development history, technical challenges and deployment evidence.

[Engineering Evidence Index](evidence/evidence-index.md)

## Development Timeline

Documents the progression from server foundation through persistence, vehicles, fuel, EMS, characters, multiplayer systems, integration and deployment.

[Development Timeline](evidence/development-timeline.md)

## Engineering Challenges

Detailed problem-solving examples including:

- Jerrycan metadata persistence
- Dynamic vehicle fuel-cap interaction
- Vehicle ownership/key synchronisation
- Test-drive state
- TDM/RP state isolation
- TDM weapon timing
- Multiplayer room lifecycle
- Spawn and fall recovery
- Character appearance recovery
- Tent transaction integrity
- Storage access control
- Marketplace transaction integrity
- Cross-resource debugging

[Engineering Challenges & Problem Solving](evidence/engineering-challenges.md)

## Deployment Evidence

Documents the FiveM/txAdmin runtime environment, Bloodline resources, MySQL architecture, database structures and development infrastructure.

[Deployment & Runtime Evidence](evidence/deployment-evidence.md)

## Community & Operational Context

Documents the community context surrounding the project, including 219 Discord members and approximately 200 whitelist applications.

[Community Adoption & Operational Context](evidence/community-adoption.md)

---

# Evidence Map

| Engineering Area | Case Study | Supporting Evidence |
|---|---|---|
| Multiplayer architecture | [TDM](docs/tdm-multiplayer-system.md) | [Engineering Challenges](evidence/engineering-challenges.md) |
| Petroleum economy | [Fuel System](docs/fuel-petroleum-economy.md) | [Development Timeline](evidence/development-timeline.md) |
| Vehicle lifecycle | [Vehicle Ecosystem](docs/vehicle-ecosystem.md) | [Engineering Challenges](evidence/engineering-challenges.md) |
| Emergency services | [EMS](docs/ems-emergency-services.md) | [Deployment Evidence](evidence/deployment-evidence.md) |
| Character management | [Dual Character](docs/dual-character-system.md) | [Engineering Challenges](evidence/engineering-challenges.md) |
| Persistent world state | [Tent Storage](docs/persistent-tent-storage.md) | [Engineering Challenges](evidence/engineering-challenges.md) |
| Transaction systems | [NPC Marketplace](docs/interactive-npc-marketplace.md) | [Engineering Challenges](evidence/engineering-challenges.md) |
| Overall architecture | [Architecture](docs/architecture.md) | [Deployment Evidence](evidence/deployment-evidence.md) |
| Development progression | — | [Development Timeline](evidence/development-timeline.md) |
| Operational context | — | [Community Adoption](evidence/community-adoption.md) |
| Player onboarding & gameplay loop | [Player Journey](evidence/player-journey.md) | [Engineering Evidence Index](evidence/evidence-index.md) |
| Database architecture & persistence | [Database Architecture](evidence/database/database-architecture.md) | [Schema Inventory](evidence/database/schema-inventory.md) |

---

# Development Approach

Development followed an iterative engineering cycle:

    Requirement
        |
        v
    Research & Design
        |
        v
    Implementation
        |
        v
    In-Game Testing
        |
        v
    Issue Discovery
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
    Deployment

When problems appeared, the affected state and resource boundaries were investigated, relevant technical references and existing framework patterns were reviewed where useful, the implementation was revised, and the behaviour was retested in-game.

Several systems went through many revisions before reaching their final working behaviour.

---

# Key Engineering Themes

Bloodline RP demonstrates practical work involving:

### Persistent State

Determining which information should survive sessions and storing it appropriately.

### Temporary State

Keeping match, preview, test-drive and interaction state isolated from permanent player data.

### Server Authority

Moving important gameplay and transaction decisions away from purely client-controlled state.

### Cross-Resource Integration

Connecting independently operating FiveM resources into complete gameplay lifecycles.

### Transaction Integrity

Ensuring operations involving money, inventory or persistent objects complete in a controlled sequence.

### Multiplayer State

Managing players, rooms, routing buckets, teams, scores and disconnect behaviour.

### Recovery

Handling invalid appearance state, interrupted TDM sessions, unsafe spawns and failed interactions.

### Iterative Debugging

Testing systems inside the actual game environment and revising behaviour based on observed edge cases.

---

# Technology Stack

| Area | Technology |
|---|---|
| Multiplayer platform | FiveM |
| Framework | QBCore |
| Primary scripting | Lua |
| Database | MySQL |
| Database bridge | oxmysql |
| Database administration | phpMyAdmin |
| Local development services | XAMPP / Apache / MySQL |
| Server administration | txAdmin |
| UI | FiveM NUI / HTML-based interfaces |
| Targeting / interaction | FiveM/QBCore ecosystem integrations |
| Version control & documentation | Git / GitHub |

---

# Third-Party Components & Attribution

Bloodline RP uses the wider FiveM open-source and commercial resource ecosystem.

Examples of external technologies or dependencies used within the environment include:

- FiveM
- QBCore
- oxmysql
- ox_target
- PolyZone
- pma-voice
- Appearance resources
- Banking resources
- Map resources
- Vehicle assets
- Other FiveM resources and dependencies

The presence of a third-party resource within the server does **not** represent a claim of original authorship.

This portfolio focuses on the Bloodline-specific development, modification, configuration, integration, debugging and system engineering performed around the server.

Where external resources were used, their original ownership and licensing remain with their respective authors.

---

# Public Repository Scope

The complete Bloodline RP production source code is **not publicly distributed**.

This repository instead provides:

- Technical architecture
- System case studies
- Engineering decisions
- Development methodology
- Debugging case studies
- Deployment evidence
- Community context
- Sanitised demonstrations

This approach provides evidence of the engineering project while protecting proprietary implementation details and server security.

---

# Security & Privacy

The repository intentionally excludes:

- Database passwords
- API keys
- FiveM/Cfx licence keys
- Tokens
- Webhooks
- Private certificates
- Security configuration
- Player IP addresses
- Discord identifiers
- Email addresses
- Private player information
- Production database records
- Administrative credentials

Any runtime or community screenshots published in this repository should be sanitised before publication.

---

# Demonstrations

Sanitised visual evidence will be organised under:

    demonstrations/
    |
    +-- runtime/
    +-- database/
    +-- tdm/
    +-- fuel/
    +-- vehicles/
    +-- ems/
    +-- characters/
    +-- storage/

These demonstrations are intended to complement the technical documentation without exposing private production source code.

---

# Repository Structure

    bloodline-rp-fivem/
    |
    +-- README.md
    |
    +-- docs/
    |   +-- architecture.md
    |   +-- tdm-multiplayer-system.md
    |   +-- fuel-petroleum-economy.md
    |   +-- vehicle-ecosystem.md
    |   +-- ems-emergency-services.md
    |   +-- dual-character-system.md
    |   +-- persistent-tent-storage.md
    |   +-- interactive-npc-marketplace.md
    |
    +-- evidence/
    |   +-- evidence-index.md
    |   +-- player-journey.md
    |   +-- development-timeline.md
    |   +-- engineering-challenges.md
    |   +-- deployment-evidence.md
    |   +-- community-adoption.md
    |   +-- database/
    |       +-- database-architecture.md
    |       +-- schema-inventory.md
    |
    +-- demonstrations/
        +-- architecture/
        +-- runtime/
        +-- database/
        +-- player-journey/
        +-- tdm/
        +-- fuel/
        +-- vehicles/
        +-- ems/
        +-- characters/
        +-- storage/

---

# Project Status

**Bloodline RP reached a functioning integrated FiveM server stage after approximately three months of development, testing, debugging and system integration.**

The project demonstrates the progression from individual gameplay features toward interconnected multiplayer software involving:

    Persistent State
          +
    Multiplayer Sessions
          +
    Database Architecture
          +
    Transaction Logic
          +
    Cross-System Integration
          +
    Failure Recovery
          +
    Community-Facing Operation

The public repository serves as the technical record of that engineering work while the production implementation remains private.
