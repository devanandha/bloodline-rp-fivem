# Bloodline RP — Deployment & Runtime Evidence

## Overview

Bloodline RP progressed beyond development files into a working FiveM server environment.

This document records deployment and runtime evidence retained from the project, including:

- FiveM/txAdmin runtime
- Bloodline resources loading in the server environment
- MySQL database infrastructure
- Bloodline-specific database structures
- Local development services
- QBCore integration
- Third-party infrastructure dependencies

The purpose of this evidence is to demonstrate that the systems documented elsewhere in this repository formed part of an operational server environment.

Runtime and database screenshots demonstrate deployment and technical scope. They are not presented, by themselves, as proof of authorship of every resource visible in the environment.

---

# 1. Runtime Environment

Bloodline RP was operated as a FiveM server built around QBCore.

The runtime environment included:

    FiveM Server
         |
         v
      txAdmin
         |
         v
    QBCore Resources
         |
    +----+----------------+
    |                     |
    v                     v
Bloodline Resources   Third-Party Resources
    |                     |
    +----------+----------+
               |
               v
             MySQL
               |
               v
            oxmysql

txAdmin was used as part of the server runtime and administration environment.

---

# 2. Server Runtime Evidence

Retained development evidence includes the txAdmin live console showing the Bloodline server online and loading server resources.

Bloodline-named resources visible in the runtime environment included examples such as:

- `bloodline-fuel`
- `bloodline-vehicleshop`
- `bloodline-shop`
- `bloodline-gas`
- `bloodline-customs`

The same runtime also included external infrastructure and dependencies such as:

- `oxmysql`
- `bablo-banking`
- `lc_utils`
- `lc_truck_logi`

This distinction is important.

The presence of a resource in the runtime does not automatically mean it was originally authored for Bloodline RP.

Third-party frameworks and dependencies are treated as integrations unless separate project evidence supports custom development.

---

# 3. Resource Architecture

The server environment was organised into multiple resource groups.

Examples included:

    resources/
    |
    +-- [banking]
    +-- [cfx-default]
    +-- [clothing]
    +-- [graphics]
    +-- [jobs]
    +-- [maps]
    +-- [ox]
    +-- [peds]
    +-- [phone]
    +-- [qb]
    +-- [radio]
    +-- [standalone]
    +-- [system]
    +-- [vehicles]

Additional Bloodline-related resources also existed within the wider server environment.

This structure demonstrates that the project involved integration across multiple FiveM resource categories rather than operating as a single standalone script.

---

# 4. Bloodline Resource Layer

The QBCore resource environment contained numerous Bloodline-specific resource names associated with the wider project.

Examples observed during documentation included:

- `bloodline_admin`
- `bloodline_fuel`
- `bloodline_repair`
- `bloodline_welcomepack`
- `bloodline-customs`
- `bloodline-dualcharacter`
- `bloodline-electronics`
- `bloodline-ems`
- `bloodline-fuelbusiness`
- `bloodline-fuelcompany`
- `bloodline-garage`
- `bloodline-healthcontrol`
- `bloodline-help`
- `bloodline-illegalchemist`
- `bloodline-interactions`
- `bloodline-killnotify`
- `bloodline-loading`
- `bloodline-namecheck`
- `bloodline-notification`
- `bloodline-shops`
- `bloodline-tdm`
- `bloodline-tentstorage`
- `bloodline-trainingtdm`
- `bloodline-vehiclecontrol`
- `bloodline-vehicleengine`
- `bloodline-vehiclekeys`
- `bloodline-vehicleshop`
- `bloodline-weaponshop`
- `bloodline-worldcontrol`

This list is included as evidence of project scope.

It should not be interpreted as a claim that every resource was created entirely from first principles or without external frameworks, dependencies, examples or integrations.

The individual case studies in this repository provide more precise descriptions of the engineering work documented for the strongest Bloodline systems.

---

# 5. Database Environment

Bloodline RP used MySQL-backed persistence.

Development evidence includes phpMyAdmin connected to the Bloodline database environment.

The database was identified as:

    bloodline_qbcore

The environment contained both framework/application tables and dedicated Bloodline-specific tables.

---

# 6. Bloodline Database Structures

Examples of Bloodline-specific database structures observed during development include:

## Fuel & Petroleum Economy

    bloodline_fuelcompany
    bloodline_fuelcompany_ledger
    bloodline_fuelcompany_staff
    bloodline_fuel_business_logs
    bloodline_fuel_player_sales
    bloodline_fuel_refill_jobs
    bloodline_fuel_stations

Additional fuel-related structures were also present for depot, worker, management and operational state.

These support the persistent architecture documented in the Fuel & Petroleum Economy case study.

---

## EMS & Emergency Services

Observed structures included:

    bloodline_ems_accounts
    bloodline_ems_service_history
    bloodline_ems_staff
    bloodline_ems_transactions

These correspond to persistent organisational, service and financial state within the EMS ecosystem.

---

## Dual Character System

Observed persistent slot data included:

    bloodline_dualcharacter_slots

This supports the persistent second-character access model documented in the Dual Character System case study.

---

## Additional Business Systems

The database environment also included structures associated with additional Bloodline business functionality, including areas such as:

- Customs
- Electronics
- Fuel businesses
- Shops
- Vehicle-related state

These provide additional evidence of the broader server architecture beyond the headline case studies.

---

# 7. Local Development Services

The development environment used XAMPP during local development.

Retained evidence shows services including:

- Apache
- MySQL

running through the XAMPP control environment.

MySQL was operating through the standard development database port:

    3306

Apache services were configured through the local development environment.

This supported database administration and local development workflows.

---

# 8. Database Administration

phpMyAdmin was used during development to inspect and manage the MySQL database environment.

This provided visibility into:

- Database tables
- Persistent records
- Schema structures
- Resource-created data
- Bloodline-specific persistence

Database inspection was useful during debugging because many gameplay issues involved the relationship between temporary in-game state and persistent server state.

---

# 9. Persistence Architecture

At a high level, deployed state followed a model similar to:

    FiveM Client
         |
         v
    Bloodline Resource
         |
         v
    FiveM Server
         |
         v
    Server-Side Logic
         |
         v
       oxmysql
         |
         v
        MySQL

Different systems used this architecture for different forms of persistence.

Examples include:

    Character
        |
        v
    Slot State
        |
        v
      MySQL

    EMS
     |
     v
Staff / Finance / History
     |
     v
   MySQL

    Fuel Economy
        |
        v
Stock / Sales / Company State
        |
        v
      MySQL

This allowed important server state to survive beyond individual client sessions.

---

# 10. Third-Party Infrastructure

Bloodline RP was not built in isolation from the FiveM ecosystem.

The server used external technologies and resources including:

- FiveM
- QBCore
- oxmysql
- ox_target
- PolyZone
- pma-voice
- progressbar
- Appearance resources
- Banking resources
- Map resources
- Vehicle assets
- Other FiveM dependencies

These components provided framework or infrastructure functionality.

They are not presented as original Bloodline RP creations.

The engineering contribution documented in this repository focuses on custom systems, modifications, integrations, debugging and the construction of the overall Bloodline RP server environment.

---

# 11. Runtime Integration

The deployment environment demonstrates that Bloodline systems operated within a larger server architecture.

For example:

    QBCore
       |
       +--> Character Systems
       |
       +--> Vehicle Systems
       |
       +--> Inventory
       |
       +--> Jobs
       |
       +--> Bloodline Systems
                 |
                 +--> Fuel
                 +--> EMS
                 +--> TDM
                 +--> Vehicles
                 +--> Businesses
                 +--> Character Management
                 +--> Persistent Storage

The project therefore required both individual system development and integration with existing platform components.

---

# 12. Evidence Interpretation

Deployment evidence should be interpreted carefully.

## What the evidence demonstrates

The retained runtime and database material supports that:

- A Bloodline RP FiveM server environment existed.
- The environment used QBCore.
- Bloodline-specific resources were present in the runtime/resource environment.
- The project used MySQL persistence.
- Bloodline-specific database structures existed.
- Multiple documented systems were represented within the deployed environment.
- Third-party and Bloodline resources operated within the same server architecture.

## What the evidence does not prove by itself

A screenshot of:

- A resource folder
- A database table
- A txAdmin console
- A running dependency

does not independently establish original authorship of the underlying implementation.

For that reason, this repository combines deployment evidence with:

- System architecture
- Development timeline
- Engineering challenge documentation
- Debugging history
- System case studies
- Demonstrations

Together these provide a more complete technical project record.

---

# 13. Evidence Safety

Public deployment evidence must be sanitised before publication.

Screenshots should not expose:

- Database passwords
- API keys
- FiveM/Cfx licence keys
- Tokens
- Webhooks
- Server credentials
- Private certificates
- Player IP addresses
- Player identifiers
- Discord identifiers
- Email addresses
- Private database records
- Security configuration
- Administrative credentials

Any screenshot containing such information should be redacted or excluded.

---

# 14. Production Source

The complete production source code is maintained privately.

The public repository focuses on:

- Architecture
- Engineering decisions
- Development history
- Debugging
- Deployment
- Demonstrations
- Sanitised technical evidence

This provides technical evidence of the project without distributing proprietary implementation details or weakening server security.

---

# Deployment Status

**Bloodline RP reached a working FiveM server stage with Bloodline-specific resources, QBCore integration and MySQL-backed persistent systems operating within the server environment.**

The deployment evidence complements the individual system case studies and demonstrates that the documented work formed part of a wider integrated server rather than a collection of disconnected design concepts.
