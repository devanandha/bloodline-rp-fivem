# Bloodline RP — Sanitised Database Schema Inventory

## Purpose

This document provides a sanitised inventory of selected database structures associated with the Bloodline RP server.

The objective is to connect documented engineering systems with persistent database structures while protecting player information, credentials, transaction records and proprietary implementation details.

This is **schema-level engineering evidence**, not a production database export.

---

## Evidence Model

The repository now provides several connected forms of evidence:

    Engineering Claim
          |
          v
    System Case Study
          |
          v
    Technical Evidence
          |
          v
    Database Structure
          |
          v
    Deployment / Visual Evidence

A database table alone does not establish original authorship.

Instead, the schema evidence supports the wider engineering documentation by demonstrating that the deployed environment contained persistent structures associated with the documented systems.

---

# EMS / Emergency Services

## `bloodline_ems_accounts`

**Purpose**

Supports persistent EMS financial/account state.

**Engineering relationship**

The EMS system includes organisational finances rather than functioning only as a temporary revive system.

**Related documentation**

- `docs/ems-emergency-services.md`
- `evidence/database/database-architecture.md`

---

## `bloodline_ems_staff`

**Purpose**

Supports persistent EMS staff information.

**Engineering relationship**

The documented EMS system includes staff management, ranks, duty-related workflows and organisational administration.

**Related documentation**

- `docs/ems-emergency-services.md`
- `evidence/development-timeline.md`

---

## `bloodline_ems_transactions`

**Purpose**

Supports persistent EMS transaction history.

**Engineering relationship**

This corresponds to the documented EMS financial and organisational workflows.

**Related documentation**

- `docs/ems-emergency-services.md`
- `evidence/database/database-architecture.md`

---

## `bloodline_ems_service_history`

**Purpose**

Supports persistent EMS service-history information.

**Engineering relationship**

This provides database-level evidence that EMS activity includes persistent historical state.

**Related documentation**

- `docs/ems-emergency-services.md`

---

# Dual Character System

## `bloodline_dualcharacter_slots`

**Purpose**

Supports persistent character-slot entitlement.

**Engineering relationship**

Bloodline RP supports two character slots with controlled access to the second slot.

The persistent slot record is separate from temporary character-preview, cinematic and interface state.

Conceptually:

    Player
      |
      v
    Slot Entitlement
      |
      +--> Slot 1
      |
      +--> Slot 2

**Related documentation**

- `docs/dual-character-system.md`
- `evidence/database/database-architecture.md`

---

# Fuel & Petroleum Economy

The Bloodline fuel architecture contains several persistent structures supporting depot, company, station, staff, sales and logistics state.

## `bloodline_depot_stock`

**Purpose**

Supports persistent central depot stock.

**Engineering relationship**

The petroleum architecture includes a central depot used by the wider fuel supply workflow.

**Related documentation**

- `docs/fuel-petroleum-economy.md`
- `evidence/technical/fuel-jerrycan-persistence.md`

---

## `bloodline_fuelcompany`

**Purpose**

Supports persistent fuel-company state.

**Engineering relationship**

The fuel system expanded beyond vehicle refuelling into company and petroleum-management workflows.

---

## Fuel Company Supporting Structures

Additional observed database structures are associated with areas including:

- Company staff
- Company ledger activity
- Company favourites
- Business logs
- Depot state
- Station management
- Player sales
- Refill jobs
- Worker state
- Active jobs

These structures support the wider relationship:

    Fuel Company
         |
         +--> Staff
         |
         +--> Finance / Ledger
         |
         +--> Depot
         |
         +--> Stations
         |
         +--> Sales
         |
         +--> Deliveries
         |
         +--> Workers

**Related documentation**

- `docs/fuel-petroleum-economy.md`
- `evidence/development-timeline.md`
- `evidence/database/database-architecture.md`

---

# Customs / Vehicle-Related Commerce

## `bloodline_customs_sales`

**Purpose**

Supports persistent sales information associated with the customs system.

**Engineering relationship**

Demonstrates persistent transaction state within a vehicle-related business workflow.

---

## `bloodline_customs_settings`

**Purpose**

Supports persistent configuration/state associated with the customs system.

**Engineering relationship**

Shows that the associated gameplay/business system contains persistent settings rather than relying entirely on temporary runtime state.

---

# Electronics Business

## `bloodline_electronics_managers`

**Purpose**

Supports persistent management state for the electronics business.

---

## `bloodline_electronics_sales`

**Purpose**

Supports persistent electronics sales information.

---

## `bloodline_electronics_settings`

**Purpose**

Supports persistent configuration/state for the electronics business.

Together these structures demonstrate a recurring Bloodline architecture pattern:

    Business
       |
       +--> Management
       |
       +--> Sales
       |
       +--> Configuration
       |
       v
    Persistent State

---

# Other Persistent Systems

The database environment also contained structures associated with other Bloodline and integrated server systems, including areas such as:

- Oil-rig activity
- Rentals
- Sales
- Shops
- Vehicle state
- Tent/storage workflows
- Parcel/delivery activity
- Phone systems
- MDT systems

Not every database table in the server represents custom Bloodline authorship.

QBCore, third-party resources and integrated systems also use the shared persistence environment.

---

# Cross-System Persistence

The selected schema evidence demonstrates that Bloodline RP is not based solely on temporary client-side gameplay.

Multiple systems depend on persistent state:

    MySQL
      |
      +--> EMS Organisation
      |
      +--> Character Slots
      |
      +--> Fuel Economy
      |
      +--> Businesses
      |
      +--> Vehicles
      |
      +--> Storage
      |
      +--> Transaction History

The database therefore forms part of the shared persistence layer connecting multiple server resources.

---

# Temporary State Is Intentionally Different

Several important Bloodline systems also use state that should **not** necessarily be stored permanently.

Examples include:

- Current TDM room
- Temporary TDM score
- Temporary TDM weapons
- Spawn protection
- Test-drive permission
- Tent placement preview
- NPC marketplace basket
- Temporary UI state

This distinction is important:

    Persistent State
          !=
    Temporary Gameplay State

The engineering challenge is determining which state belongs in each category and ensuring transitions between them remain consistent.

---

# Evidence Interpretation

This inventory should not be interpreted as a claim that every database table in the Bloodline RP database was designed or authored from first principles for the project.

Bloodline RP uses:

- QBCore
- Third-party resources
- Integrated resources
- Modified systems
- Custom-developed Bloodline systems

The purpose of this evidence is narrower:

> To demonstrate that the documented Bloodline environment contained real persistent database structures associated with the server's gameplay, business and organisational systems.

Original-development claims should be evaluated together with the relevant case studies, technical evidence, development history and demonstrations.

---

# Privacy & Security

No public schema evidence should contain:

- Player names
- Character names linked to real individuals
- Licence identifiers
- Discord identifiers
- IP addresses
- Email addresses
- Passwords
- Database credentials
- API keys
- Tokens
- Webhooks
- Private financial records
- Private inventory records
- Authentication information

Screenshots should be reviewed and redacted before publication.

---

# Planned Visual Evidence

A sanitised database screenshot can later be stored at:

    demonstrations/database/bloodline-database-schema.png

The screenshot should show only safe schema/table-level information.

It should not expose database rows containing private player or operational data.

---

# Supporting Documentation

See:

- [Database Architecture](database-architecture.md)
- [System Architecture](../../docs/architecture.md)
- [Deployment Evidence](../deployment-evidence.md)
- [Engineering Evidence Index](../evidence-index.md)
- [EMS System](../../docs/ems-emergency-services.md)
- [Fuel & Petroleum Economy](../../docs/fuel-petroleum-economy.md)
- [Dual Character System](../../docs/dual-character-system.md)

---

## Evidence Boundary

This document contains a selected and sanitised schema inventory.

It intentionally does not publish:

- SQL dumps
- Production queries
- Database rows
- Player records
- Credentials
- Private transaction data
- Security configuration

The objective is to provide verifiable architectural context without exposing the production database or community data.
