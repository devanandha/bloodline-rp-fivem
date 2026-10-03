# Bloodline RP Database Architecture — Sanitised Engineering Evidence

## Purpose

This document provides a sanitised overview of the persistent data architecture used by Bloodline RP.

It demonstrates how multiple gameplay systems were connected to persistent MySQL-backed state without publishing player records, credentials, private data or production database exports.

---

## Technology Context

Bloodline RP was developed around:

- FiveM
- QBCore
- MySQL
- oxmysql
- phpMyAdmin
- XAMPP / MySQL during local development
- txAdmin for server runtime and administration

The database formed part of the persistent state layer supporting multiple gameplay systems.

---

## High-Level Architecture

    FiveM Client
         |
         v
    Gameplay Resources
         |
         v
    Server-Side Validation
         |
         v
       oxmysql
         |
         v
        MySQL
         |
         v
    Persistent Game State

This allowed gameplay information that needed to survive reconnects or server restarts to be stored separately from temporary runtime state.

---

## Temporary vs Persistent State

Not every piece of gameplay information belongs in the database.

### Temporary State

Examples include:

- TDM match score
- Current TDM room
- Temporary match weapon
- Temporary test-drive access
- Tent placement preview
- NPC marketplace basket
- NUI state
- Spawn protection

### Persistent State

Examples include:

- Character information
- Vehicle ownership
- Fuel-business state
- EMS organisation data
- Character-slot entitlement
- Persistent tent ownership
- Business transactions
- Service history

A central engineering principle was therefore:

    Runtime State
         !=
    Persistent State

The system needed to determine which state should disappear at the end of a session and which state should survive.

---

## Fuel & Petroleum Persistence

The database architecture includes multiple tables associated with the Bloodline fuel and petroleum systems.

Examples documented during development include:

- `bloodline_depot_stock`
- `bloodline_fuelcompany`
- Fuel-company staff data
- Fuel-company ledger data
- Fuel-company favourites
- Fuel-business logs
- Fuel-company configuration
- Fuel-company staff relationships
- Depot state
- Station state
- Station-management data
- Player sales
- Refill jobs
- Worker state

Conceptually:

    Fuel System
        |
        +--> Depot Stock
        |
        +--> Company
        |
        +--> Staff
        |
        +--> Stations
        |
        +--> Transactions
        |
        +--> Delivery / Refill State

This supported the transition from a vehicle-fuelling mechanic into a wider petroleum economy.

---

## EMS Persistence

The EMS system includes persistent organisational and service data.

Documented Bloodline tables include:

- `bloodline_ems_accounts`
- `bloodline_ems_service_history`
- `bloodline_ems_staff`
- `bloodline_ems_transactions`

Conceptually:

    EMS System
        |
        +--> Staff
        |
        +--> Accounts
        |
        +--> Transactions
        |
        +--> Service History

This allows the EMS system to represent an organisation rather than only a temporary revive mechanic.

---

## Character-Slot Persistence

The dual-character system includes persistent slot-entitlement state.

A documented table is:

- `bloodline_dualcharacter_slots`

Conceptually:

    Player Identity
          |
          v
    Character Slot State
       /          \
    Slot 1       Slot 2
    Available    Controlled Access

The persistent slot state allows access rules to remain consistent across sessions.

Temporary cinematic previews and UI state remain separate from this persistent entitlement.

---

## Vehicle Persistence

The wider QBCore/Bloodline vehicle ecosystem uses persistent ownership data.

Conceptually:

    Vehicle Purchase
          |
          v
    Ownership Record
          |
          v
    Vehicle Identity
          |
          +--> Key Access
          |
          +--> Garage
          |
          +--> Engine
          |
          +--> Fuel
          |
          v
    Continued Ownership

The important engineering problem was not simply storing a vehicle.

Multiple resources needed to interpret the same persistent ownership relationship consistently.

This is documented separately in:

[Vehicle Ownership & Key Synchronisation](../technical/vehicle-ownership-key-sync.md)

---

## Persistent Tent State

The tent-storage system transforms an inventory item into a persistent world-storage object.

Conceptually:

    Inventory Item
          |
          v
    Valid Placement
          |
          v
    Persistent Tent
          |
          +--> Owner
          |
          +--> Position
          |
          +--> Access State
          |
          v
    Private Storage

The persistence layer allows the tent to represent more than a temporary client-side object.

Transaction sequencing for this system is documented in:

[Persistent Tent Transaction Integrity](../technical/tent-transaction-integrity.md)

---

## Business & Commerce Data

Bloodline RP also contains persistent data associated with business and commerce systems.

Examples observed in the database architecture include tables associated with:

- Customs sales
- Customs settings
- Electronics managers
- Electronics sales
- Electronics settings
- Fuel businesses
- Oil-rig activity
- Shops
- Vehicle-related state
- Other business-management workflows

Examples of documented table names include:

- `bloodline_customs_sales`
- `bloodline_customs_settings`
- `bloodline_electronics_managers`
- `bloodline_electronics_sales`
- `bloodline_electronics_settings`

These tables demonstrate that several gameplay systems required persistent organisational or transaction state.

---

## State Relationships

The wider architecture can be represented as:

    Player / Character
           |
     +-----+--------------------+
     |             |            |
     v             v            v
    Vehicles      EMS       Businesses
     |             |            |
     v             v            v
    Ownership     Staff      Transactions
     |             |            |
     +-------------+------------+
                   |
                   v
                 MySQL

Other systems such as character slots, tents and fuel infrastructure also interact with the persistence layer.

---

## Server Authority

Persistent database operations should be coordinated through server-side logic rather than allowing the client interface to act as the authority.

Conceptually:

    Client Request
          |
          v
    Server Validation
          |
          v
    Authorised State Change
          |
          v
    Database Operation
          |
          v
    Confirm Result
          |
          v
    Client Update

This pattern appears throughout the documented Bloodline engineering work.

---

## Persistence & Recovery

Persistent state becomes particularly important when considering:

- Player reconnects
- Resource restarts
- Server restarts
- Interrupted gameplay sessions
- Ownership validation
- Organisational state
- Transaction history

Temporary runtime state can be reconstructed or discarded depending on the system.

Persistent state should remain available where the gameplay design requires continuity.

---

## Database Evidence Interpretation

The existence of a table demonstrates that the deployed server contained persistent data structures associated with that system.

It does **not**, by itself, prove that every table or underlying framework component was originally authored for Bloodline RP.

Bloodline RP was built on QBCore and integrated third-party resources alongside custom-developed and modified systems.

Database evidence should therefore be interpreted together with:

- Engineering case studies
- Technical evidence
- Development history
- Deployment evidence
- Testing evidence
- Visual demonstrations

This avoids using database presence alone as a claim of original authorship.

---

## Security & Privacy

Public database evidence must not expose production or player data.

The following should remain private:

- Player names
- Character identifiers
- Licence identifiers
- Discord identifiers
- IP addresses
- Email addresses
- Authentication information
- Passwords
- Database credentials
- API keys
- Tokens
- Webhooks
- Private transaction records
- Private inventory contents

Only sanitised schema-level information should be used as public portfolio evidence.

---

## Supporting Evidence

See:

- [System Architecture](../../docs/architecture.md)
- [Deployment Evidence](../deployment-evidence.md)
- [Engineering Evidence Index](../evidence-index.md)
- [Fuel & Jerrycan Persistence](../technical/fuel-jerrycan-persistence.md)
- [Vehicle Ownership & Key Synchronisation](../technical/vehicle-ownership-key-sync.md)
- [Tent Transaction Integrity](../technical/tent-transaction-integrity.md)

---

## Evidence Boundary

This document intentionally describes the database at an architectural level.

It does not publish:

- Database exports
- Production SQL dumps
- Credentials
- Private player records
- Personally identifiable information
- Authentication data
- Security configuration

The objective is to demonstrate persistence architecture while protecting the live implementation and community data.
