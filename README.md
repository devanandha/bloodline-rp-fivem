# Bloodline RP — FiveM Roleplay Server

> A production multiplayer roleplay server built on the FiveM/QBCore ecosystem, developed, customised, deployed and operated over approximately three months.

Bloodline RP is a heavily customised FiveM roleplay server designed around interconnected gameplay, economy, vehicle, emergency-service, character and competitive multiplayer systems.

The project involved designing custom gameplay systems, integrating and extending QBCore resources, managing MySQL-backed persistence, building user interfaces, configuring multiplayer infrastructure, debugging cross-resource issues, deploying the server and supporting a live player community.

Rather than publishing the production source code, this repository documents the **system architecture, engineering decisions, development process, technical challenges, deployment and demonstrated functionality** of the project.

---

## Project Overview

**Platform:** FiveM  
**Framework:** QBCore  
**Primary Language:** Lua  
**Database:** MySQL  
**Database Administration:** phpMyAdmin  
**Development Environment:** XAMPP / Apache / MySQL  
**Project:** Bloodline RP  
**Development Period:** Approximately 3 months  
**Status:** Fully working multiplayer server

The server combines custom Bloodline systems with selected third-party FiveM/QBCore resources. Custom components were developed and integrated around a shared server architecture so that systems such as vehicles, fuel, character management, emergency services, businesses, storage and competitive multiplayer could operate together.

---

## Core Systems

### Fuel & Petroleum Economy

An interconnected fuel ecosystem covering vehicle refuelling, multiple fuel grades, fuel consumption, jerrycan handling, station inventory, wholesale fuel supply, company finances, staff permissions and tanker-based station replenishment.

The architecture connects:

`Fuel Company → Fuel Businesses → Fuel Stations → Vehicle Refuelling`

---

### Vehicle Ecosystem

A connected vehicle architecture covering:

- Vehicle dealership and purchasing
- Persistent vehicle ownership
- Vehicle keys and access control
- Locking and hotwire behaviour
- Garage storage and retrieval
- Job-specific vehicle access
- Engine-state management
- Fuel integration
- Test-drive isolation

The systems were designed to communicate across separate resources rather than operating as independent gameplay scripts.

---

### EMS & Emergency Services

A custom emergency-services system incorporating:

- Death and emergency interaction flow
- CPR/revival and treatment
- EMS duty management
- Staff ranks and permissions
- EMS payments and organisational finances
- Uniform handling
- EMS vehicle access
- Hospital interactions
- Lift and helipad functionality

---

### Dual Character System

A database-backed two-character system providing:

- Persistent character slots
- Administrative second-slot unlocking
- Character ownership validation
- Character creation
- Appearance integration
- Persistent character information
- Spawn selection
- Cinematic character previews
- Recovery handling for appearance-related issues

---

### TDM & Multiplayer Session System

An isolated competitive multiplayer environment running alongside the main RP server.

Features include:

- Private team rooms
- Public free-for-all environments
- Routing-bucket isolation
- Multiple playable maps
- Custom combat/damage rules
- TDM-only weapons and ammunition
- Match scoring
- Spawn protection
- Redzone handling
- Fall recovery
- Crash/reconnect recovery
- Temporary team clothing
- Custom NUI interface

The TDM environment was deliberately isolated so that normal RP inventory, economy, jobs, garages and emergency-service systems were not modified by competitive matches.

---

### Persistent Tent Storage

A deployable private-storage system incorporating:

- Purchasable tent packs
- World-object placement
- Placement validation
- Rotation and preview controls
- Persistent private inventory
- PIN-protected access
- Tent ownership
- Safe deletion rules
- Inventory-system compatibility handling

---

### Interactive NPC Marketplace

A roleplay marketplace system using an interactive NPC delivery workflow.

It includes:

- Interactive world location
- Custom shop interface
- Multi-item basket purchasing
- Business balance management
- Manager/admin controls
- NPC handover sequences
- Inventory delivery
- Animation and interaction states
- Cross-system item integration

---

## Engineering & Debugging

A significant part of Bloodline RP development involved resolving integration and runtime problems across interconnected resources.

Examples included:

- Vehicle key state not synchronising immediately after vehicle purchase
- Test-drive vehicles incorrectly triggering normal hotwire behaviour
- Job vehicles requiring specialised access rules
- Fuel nozzle targeting behaving differently across vehicle dimensions
- Jerrycan metadata and durability persistence
- Character appearance fallback issues
- Inventory UI conflicts with custom interfaces
- World props spawning above or below terrain
- Multiplayer players being affected by another player's room departure
- TDM damage conflicting with normal GTA/QBCore death behaviour
- Unsafe map spawn positions and collision loading
- Restoring normal player state after leaving isolated gameplay sessions

These issues were addressed through iterative testing, debugging, configuration changes, integration work and repeated validation in the running server environment.

---

## Architecture

Bloodline RP follows a modular resource architecture.

```text
                         BLOODLINE RP
                              │
             ┌────────────────┴────────────────┐
             │                                 │
          QBCore                           MySQL
             │                                 │
     ┌───────┼────────┐                Persistent Data
     │       │        │
 Gameplay  Economy  Player Systems
     │       │        │
     │       │        ├── Dual Character
     │       │        ├── Appearance
     │       │        └── Inventory
     │       │
     │       ├── Fuel Company
     │       ├── Fuel Businesses
     │       ├── Electronics
     │       └── Other Businesses
     │
     ├── Vehicle Ecosystem
     ├── EMS
     ├── Tent Storage
     ├── NPC Marketplace
     └── TDM
```

More detailed architecture documentation will be maintained in the `/docs` and `/architecture` sections of this repository.

---

## Development Approach

Development followed an iterative workflow:

`Requirement → Design → Implementation → Integration → Testing → Debugging → Validation → Deployment`

Individual systems frequently required multiple revisions after testing them against real FiveM/QBCore behaviour.

The project therefore represents not only feature implementation, but also practical experience in multiplayer state management, persistent data, resource integration, gameplay logic, UI integration, troubleshooting and server operations.

---

## Community & Operations

Bloodline RP progressed beyond a local development environment into an operational multiplayer community.

The project included:

- Server deployment and administration
- Discord-based community management
- Player onboarding
- Whitelist/application management
- Live troubleshooting
- Resource updates
- Gameplay testing
- Ongoing server configuration

Technical and community evidence will be documented separately in this repository.

---

## Repository Scope

The complete Bloodline RP production source code is **not publicly distributed**.

Bloodline RP contains custom production systems developed specifically for the live server. The complete implementation is maintained privately to protect proprietary implementation details, server security and the integrity of custom gameplay systems.

This repository therefore focuses on:

- System architecture
- Engineering decisions
- Development methodology
- Technical case studies
- Debugging and problem solving
- Deployment evidence
- Demonstrations
- Sanitised technical documentation

Where appropriate, diagrams and illustrative examples may be provided without exposing production implementation.

---

## Third-Party Components & Attribution

Bloodline RP is built within the wider FiveM and QBCore ecosystem and integrates selected third-party resources.

Third-party frameworks, libraries, maps, assets and resources remain the work of their respective authors.

This portfolio distinguishes between:

- **Custom Bloodline systems**
- **Modified or integrated resources**
- **Third-party infrastructure and dependencies**

No third-party component is represented as original Bloodline development.

---

## Security & Privacy

Public documentation intentionally excludes:

- Server credentials
- Database passwords
- API keys and tokens
- FiveM/Cfx license keys
- Security configuration
- Webhooks
- Private player information
- Player identifiers
- Production database records
- Proprietary production source code

---

## Documentation

Detailed technical documentation and evidence will be added progressively:

- System Architecture
- Fuel & Petroleum Economy
- Vehicle Ecosystem
- EMS & Emergency Services
- Character System
- TDM Multiplayer Architecture
- Persistent Storage
- Engineering Challenges
- Development Timeline
- Deployment Evidence
- Community & Project Impact

---

## Project Status

**Bloodline RP is a completed and operational FiveM/QBCore multiplayer server project.**

This repository serves as the public technical case study and engineering record of its development.
