# Bloodline Fuel & Petroleum Economy

## Overview

The Bloodline Fuel & Petroleum Economy is an interconnected fuel-management ecosystem developed for Bloodline RP.

Rather than treating vehicle fuel as a simple standalone percentage, the system connects player vehicle refuelling with fuel stations, station businesses, petroleum stock, wholesale supply and delivery operations.

The objective was to create a persistent economy in which fuel moves through multiple layers of the server:

    Central Fuel Company / Depot
                |
                v
        Wholesale Fuel Supply
                |
                v
       Fuel Business / Station
                |
                v
        Station Fuel Inventory
                |
                v
          Player Purchase
                |
                v
        Vehicle Fuel System
                |
                v
       Vehicle Consumption

The implementation required coordination between gameplay logic, vehicle state, QBCore, inventory handling, persistent database records and business systems.

---

## Core Systems

The petroleum ecosystem is composed of several connected components:

- Vehicle fuel management
- Fuel station businesses
- Central fuel company
- Petroleum depot
- Wholesale fuel distribution
- Station inventory
- Fuel pricing
- Player refuelling
- Jerrycan refuelling
- Vehicle fuel consumption
- Business transactions
- Staff and management permissions
- Delivery jobs
- Persistent financial records

These components were designed to operate as parts of the same economy rather than as isolated resources.

---

## System Architecture

    Central Petroleum Supply
               |
               v
       Bloodline Fuel Company
               |
       +-------+-------+
       |               |
       v               v
    Depot Stock     Company Finance
       |
       v
    Wholesale Fuel
       |
       v
    Station / Fuel Business
       |
       +-- Station Stock
       +-- Pricing
       +-- Staff
       +-- Transactions
       +-- Deliveries
       |
       v
    Player Refuelling
       |
       +-- Regular
       +-- Plus
       +-- Premium
       +-- Diesel
       |
       v
    Vehicle Fuel State
       |
       v
    Fuel Consumption

---

## Vehicle Fuel System

The vehicle fuel component manages the interaction between players, fuel pumps and vehicles.

The system supports multiple fuel grades:

- Regular
- Plus
- Premium
- Diesel

Fuel behaviour is connected to vehicle tank capacity and fuel consumption.

Instead of treating every vehicle identically, the system was designed around vehicle-specific fuel state and physical refuelling interaction.

The implementation integrates with the QBCore environment and associated inventory and targeting systems.

---

## Physical Fuel Interaction

A key design objective was making refuelling behave like a physical interaction rather than simply opening a menu beside a vehicle.

The system includes:

- Fuel pump interaction
- Fuel nozzle handling
- Hose behaviour
- Vehicle fuel-cap targeting
- Refuelling animations
- Fuel quantity handling
- Jerrycan support

One of the more difficult development areas was determining the correct interaction point on different vehicles.

Vehicle models do not all place their fuel-cap area on the same side or in the same position.

The system therefore evolved toward dynamic vehicle-side fuel-cap targeting rather than relying on one fixed interaction point for every vehicle.

---

## Dynamic Fuel-Cap Targeting

### Problem

Early interaction approaches could make refuelling inconvenient or unreliable because vehicle fuel-cap positions vary between models.

A static target zone could appear on the wrong side of the vehicle or fail to provide a natural interaction point.

### Development

The targeting logic was revised to better associate the interaction with the vehicle and its expected fuel-cap side.

Testing also exposed differences between generic target zones and entity-based targeting.

### Result

Fuel interaction became associated more closely with the actual vehicle rather than relying entirely on a fixed world-space interaction zone.

This improved the consistency of refuelling across different vehicle models.

---

## Jerrycan System

Jerrycan behaviour became one of the most time-consuming parts of the fuel implementation.

The intended design was for a jerrycan to represent a limited physical fuel container rather than an unlimited refuelling item.

The system was designed around a maximum fuel capacity of:

    100 litres = 100 durability

As fuel is consumed from the container, its remaining state must persist correctly.

---

## Jerrycan Persistence Challenge

### Problem

During development, jerrycan state could behave incorrectly when inventory operations occurred.

A particularly important edge case involved removing and re-adding the item.

If remaining fuel was not persisted correctly, inventory movement could effectively reset the container and create unlimited fuel.

### Investigation

The issue required repeated testing of:

- Inventory metadata
- Item removal
- Item re-addition
- Slot behaviour
- Remaining fuel state
- Server/client synchronisation
- Refuelling continuation

### Resolution

Jerrycan state handling was moved toward server-authoritative persistence, with the remaining fuel represented through item metadata rather than depending only on temporary client state.

### Result

Removing and re-adding the same jerrycan no longer needed to recreate a full container.

The remaining fuel state could instead follow the item.

This prevented a state-reset exploit and made the container behave as a finite resource.

---

## Server Authority

Fuel transactions involve state that should not rely entirely on the client.

Important fuel operations therefore required server-side validation or persistence.

Examples include:

- Jerrycan remaining capacity
- Inventory changes
- Business stock
- Station stock
- Financial transactions
- Delivery completion
- Company state

This reduces the risk of temporary client state becoming the authoritative record for persistent economic data.

---

## Fuel Business System

Fuel stations operate as business entities within the larger petroleum economy.

The business layer manages operational elements such as:

- Station fuel stock
- Fuel purchases
- Fuel pricing
- Business funds
- Staff
- Managers
- Delivery operations
- Transaction records

This transforms fuel stations from simple map locations into persistent economic entities.

---

## Central Fuel Company

Above individual fuel stations is a central fuel-company layer.

This provides a wholesale supply model for the wider fuel economy.

The company system includes concepts such as:

- Central depot stock
- Wholesale fuel pricing
- Company funds
- Staff permissions
- Station supply
- Fuel transfers
- Management operations

The central depot was designed around a substantially larger fuel reserve than an individual station.

A configured depot capacity used during development was:

    100,000 litres

Fuel could then move from the central supply layer toward individual stations.

---

## Fuel Distribution

Fuel movement follows a supply-chain model:

    Central Depot
         |
         v
    Wholesale Supply
         |
         v
    Delivery Operation
         |
         v
      Fuel Station
         |
         v
     Station Stock
         |
         v
    Player Purchase

Delivery operations connect the business economy to physical gameplay.

Configured delivery quantities during development included 1,000-litre supply movements.

This means station inventory can conceptually be replenished through logistics rather than existing as an unlimited resource.

---

## Economy Integration

The fuel ecosystem connects several different forms of state.

### Physical State

- Vehicle fuel level
- Fuel tank capacity
- Jerrycan capacity
- Station stock
- Depot stock

### Financial State

- Player payments
- Station revenue
- Wholesale cost
- Business funds
- Company funds

### Organisational State

- Employees
- Managers
- Company staff
- Permissions
- Station ownership

### Transaction State

- Fuel purchases
- Deliveries
- Business transactions
- Company transactions
- Sales records

Managing these states together required the fuel system to operate as more than a standalone vehicle script.

---

## Persistent Data

The petroleum economy uses persistent database-backed state.

Development and deployment evidence includes dedicated Bloodline database structures for areas such as:

- Fuel company
- Company staff
- Fuel ledger
- Fuel workers
- Fuel managers
- Active jobs
- Business logs
- Player sales
- Refill jobs
- Fuel stations
- Depot state

The public repository does not contain production database records.

Database references are documented only to demonstrate the persistence model and system architecture.

---

## Cross-System Integration

The fuel ecosystem interacts with other Bloodline RP components.

Examples include:

- Vehicle systems
- Inventory
- Player economy
- QBCore
- Targeting
- Business management
- Database persistence
- Notifications
- Delivery gameplay

This required integration testing because changes to one part of the fuel lifecycle could affect another.

For example:

    Jerrycan
        |
        v
    Inventory Metadata
        |
        v
    Refuelling Logic
        |
        v
    Vehicle Fuel State

and:

    Fuel Company
        |
        v
    Depot Stock
        |
        v
    Business Delivery
        |
        v
    Station Stock
        |
        v
    Player Transaction

---

## Selected Engineering Challenges

### Challenge 1 — Vehicle Fuel-Cap Position

**Problem**

Different vehicle models made a single fixed refuelling interaction point unreliable.

**Resolution**

The interaction model was revised toward dynamic vehicle-side targeting and vehicle-associated interaction.

**Result**

Fuel interaction became more consistent across different vehicles.

---

### Challenge 2 — Jerrycan Capacity Reset

**Problem**

Inventory operations could create incorrect jerrycan capacity behaviour if remaining fuel existed only as temporary state.

**Resolution**

Remaining container state was persisted through inventory metadata with server-side handling.

**Result**

The jerrycan behaved as a finite fuel container instead of being reset through normal inventory movement.

---

### Challenge 3 — Client and Server State

**Problem**

Fuel state exists across multiple gameplay and persistence layers.

Relying only on client state could produce inconsistencies between inventory, vehicle and economic records.

**Resolution**

Persistent and economically important state was handled through server-side logic and database-backed records where appropriate.

**Result**

The fuel ecosystem gained a clearer separation between temporary interaction state and persistent authoritative state.

---

### Challenge 4 — Connecting Multiple Fuel Systems

**Problem**

Vehicle refuelling, station businesses and the central petroleum company initially represent different domains.

They still need to operate as one economy.

**Resolution**

The systems were connected through stock movement, pricing, delivery and transaction flows.

**Result**

Fuel became a supply-chain system rather than an isolated vehicle mechanic.

---

## Development & Debugging

The fuel ecosystem required extensive iterative debugging.

The development process followed a pattern similar to:

    Requirement
        |
        v
    Initial Implementation
        |
        v
    In-Game Testing
        |
        v
    Behavioural Issue
        |
        v
    Investigation
        |
        v
    Code / State Revision
        |
        v
    Retesting
        |
        v
    Integration Testing
        |
        v
    Working Behaviour

The jerrycan implementation in particular required repeated investigation because inventory metadata, capacity, item movement and continuous fuel usage had to remain synchronised.

This debugging process formed a significant part of the development work rather than simply configuring an existing fuel resource.

---

## Technical Areas Demonstrated

The petroleum ecosystem provides evidence of practical work involving:

- Lua-based FiveM development
- QBCore integration
- Client/server state management
- MySQL-backed persistence
- Inventory metadata
- Vehicle state
- Dynamic interaction targeting
- Business logic
- Economic modelling
- Persistent transactions
- Permission systems
- Cross-resource integration
- Supply-chain modelling
- Edge-case testing
- Exploit prevention
- Iterative debugging

---

## Production Source

The complete production implementation is maintained privately.

This public case study documents the architecture, behaviour, integration and debugging process without publishing proprietary production source code, private server configuration, credentials or security-sensitive implementation details.

Third-party framework and infrastructure components remain attributed to their respective authors and are not presented as original Bloodline RP development.

---

## Status

**The Bloodline Fuel & Petroleum Economy was implemented as part of the completed Bloodline RP server, connecting vehicle fuel mechanics with station businesses, petroleum supply, persistent inventory and economic state.**
