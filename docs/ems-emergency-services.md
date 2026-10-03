# Bloodline EMS & Emergency Services

## Overview

Bloodline EMS & Emergency Services is an integrated emergency-response and medical-roleplay system developed for Bloodline RP.

The system was designed to support the complete lifecycle of an EMS interaction rather than treating revival as a single standalone action.

It connects player medical state with EMS staff, treatment workflows, duty status, organisational permissions, payments, service history, uniforms, vehicles and emergency-service infrastructure.

A simplified workflow is:

    Player Emergency
          |
          v
    EMS Response
          |
          v
    Medical Assessment
          |
     +----+----+
     |         |
     v         v
    CPR     Treatment
     |         |
     +----+----+
          |
          v
       Recovery
          |
          v
    Service Record
          |
          v
    Payment / EMS Account

The system therefore combines gameplay mechanics with organisational and financial state.

---

## Core Features

The EMS ecosystem includes:

- Emergency medical interactions
- CPR and revive workflows
- Player treatment
- EMS duty management
- Staff records
- Rank-based permissions
- EMS financial accounts
- Service payments
- Night-duty bonus handling
- Service history
- EMS uniforms
- Emergency vehicles
- EMS garage integration
- Lift and helipad access
- Management functionality
- Administrative controls
- Persistent database-backed records

---

## System Architecture

    Player Medical State
            |
            v
       EMS Interaction
            |
       +----+----+
       |         |
       v         v
    CPR/Revive  Treatment
       |         |
       +----+----+
            |
            v
       Service Outcome
            |
       +----+----+
       |         |
       v         v
    Service Log  Payment
                     |
                     v
              EMS Financial State

    EMS Staff
       |
       +-- Duty State
       +-- Rank
       +-- Permissions
       +-- Uniform
       +-- Vehicle Access
       +-- Management Access

---

## Emergency Response Workflow

The medical system was designed around an EMS response lifecycle.

Rather than allowing emergency interactions to exist independently, the workflow connects the patient's state with the responding EMS member.

A typical flow is:

    Emergency Occurs
          |
          v
    EMS Staff Respond
          |
          v
    Patient Interaction
          |
          v
    Treatment / CPR
          |
          v
    Recovery / Revive
          |
          v
    Service Completion
          |
          v
    Financial & Service Record

This creates a structured emergency-service workflow within the roleplay environment.

---

## CPR & Revive

CPR and revival form part of the medical interaction layer.

The EMS workflow needs to coordinate:

- Player medical state
- EMS interaction state
- Treatment actions
- Successful recovery
- Service completion

This is particularly important because player death and recovery interact with other server systems.

The EMS implementation therefore needed to coexist with normal Bloodline RP gameplay while allowing specialised systems, such as TDM, to maintain separate competitive death behaviour.

---

## Medical Treatment

EMS interactions include treatment functionality in addition to revival.

The system was designed so that medical-service actions could form part of an organised EMS workflow rather than being represented only by a generic player interaction.

Treatment state can therefore be associated with the wider service lifecycle and organisational records.

---

## EMS Staff System

EMS operates as an organisation with persistent staff state.

The staff layer supports concepts such as:

- EMS employees
- Staff ranks
- Duty status
- Management permissions
- Administrative permissions
- Service activity

This creates a distinction between ordinary players and authorised EMS personnel.

---

## Rank & Permission Management

Not every EMS member should have the same organisational permissions.

The system therefore includes role-based access concepts.

Different levels of staff can be associated with different operational or management capabilities.

This allows the EMS resource to support both frontline medical gameplay and organisational administration.

A simplified model is:

    EMS Organisation
          |
          v
       Staff Member
          |
       +-- Rank
       +-- Duty State
       +-- Permissions
       +-- Service Activity

---

## Duty Management

EMS staff can operate within an active duty state.

Duty status provides an important boundary between:

    EMS Character

and:

    EMS Character Currently Working

This distinction can be used by other EMS functions when determining access to emergency-service functionality.

It also provides the basis for operational features such as duty-related payment or bonus handling.

---

## Night-Duty Bonus

The EMS financial model includes support for night-duty bonus behaviour.

This adds time-sensitive operational logic to the EMS system rather than treating every service period identically.

The feature demonstrates the connection between:

- Duty state
- Working period
- EMS staff
- Payment logic

---

## EMS Financial System

Emergency-service activity is connected to a dedicated organisational financial layer.

The system includes concepts such as:

- EMS account balance
- Service payments
- Transactions
- Staff-related financial activity
- Organisational funds

This means EMS operates as both a gameplay service and a managed organisation.

---

## City Council / EMS Account

The financial workflow includes an organisational account used for EMS-related transactions.

A simplified transaction model is:

    EMS Service
         |
         v
    Payment Calculation
         |
         v
    Transaction
         |
         v
    Organisational Account
         |
         v
    Persistent Financial Record

This separates organisational finance from temporary client-side state.

---

## Service History

EMS activity can generate persistent service-history information.

This provides a record of completed EMS-related activity rather than allowing the interaction to disappear entirely after the immediate gameplay event.

Service history contributes to the administrative and operational side of the system.

---

## Persistent Data

The EMS ecosystem uses database-backed persistence.

Development and deployment evidence includes dedicated Bloodline database structures for areas including:

- EMS accounts
- EMS service history
- EMS staff
- EMS transactions

These persistent structures demonstrate that organisational and financial EMS state extends beyond a single player session.

Production database contents and private player information are not included in the public repository.

---

## EMS Vehicles

Emergency-service operations also require vehicle access.

EMS vehicles form part of the wider Bloodline Vehicle Ecosystem while using specialised job-based access rules.

The relationship can be represented as:

    EMS Staff
       |
       v
    Duty / Permission
       |
       v
    EMS Garage
       |
       v
    Emergency Vehicle
       |
       v
    Emergency Response

This demonstrates cross-system integration between EMS and vehicle-management resources.

---

## EMS Garage

The EMS garage provides access to emergency-service vehicles without requiring the normal privately owned vehicle-purchase lifecycle.

This requires the vehicle ecosystem to distinguish between:

- Personally owned vehicles
- Temporary vehicles
- Job-authorised vehicles
- EMS vehicles

The EMS garage therefore represents a specialised access path within the wider vehicle architecture.

---

## Uniform Management

EMS staff can use role-appropriate uniform functionality.

Uniform state is associated with the operational EMS experience and helps distinguish active emergency-service personnel within the roleplay environment.

This introduces another connection between staff state, duty behaviour and player appearance.

---

## Lift & Helipad Access

The EMS environment also includes infrastructure interactions such as lift and helipad access.

These features support movement through the emergency-service environment and access to specialised operational areas.

Helipad access also connects naturally with emergency aviation and EMS vehicle workflows.

---

## Administrative Controls

The EMS system includes management and administrative functionality.

Administrative controls are separated from ordinary medical interactions so that operational configuration and staff management can be restricted appropriately.

This helps maintain a distinction between:

    Patient Interaction

    Frontline EMS Operations

    EMS Management

    Administrative Control

Each represents a different permission context.

---

## Cross-System Integration

The EMS ecosystem interacts with several other areas of Bloodline RP.

Examples include:

- Player state
- QBCore
- Vehicle systems
- Job permissions
- Character appearance
- Financial systems
- Database persistence
- User interfaces
- Emergency vehicles
- TDM isolation

A normal emergency flow may involve:

    Player State
        |
        v
    EMS Interaction
        |
        v
    Treatment
        |
        v
    Recovery
        |
        +--> Service History
        |
        +--> Financial Transaction

Meanwhile:

    EMS Staff State
        |
        +--> Duty
        +--> Rank
        +--> Uniform
        +--> Vehicle Access
        +--> Management Access

This makes EMS a multi-system workflow rather than a single revive function.

---

## Separation From Competitive Gameplay

Bloodline RP also contains the isolated Bloodline TDM multiplayer system.

Competitive TDM damage and elimination behaviour was intentionally designed to remain separate from normal RP medical and EMS flows.

Conceptually:

    Normal RP Injury
           |
           v
       EMS Workflow

while:

    TDM Combat
        |
        v
    TDM Elimination
        |
        v
    TDM Respawn Logic

This separation prevents competitive gameplay from incorrectly triggering normal roleplay emergency-service behaviour.

---

## Selected Engineering Challenges

### Challenge 1 — Coordinating Medical and Player State

**Problem**

Medical interactions depend on the player's current state and must avoid conflicting transitions.

**Approach**

EMS actions were structured around a controlled emergency-response workflow.

**Result**

Treatment and recovery could operate as part of an organised medical lifecycle rather than isolated actions.

---

### Challenge 2 — Organisational Permissions

**Problem**

Frontline EMS staff, managers and administrators require different levels of access.

**Approach**

Staff state and rank-based permission concepts were incorporated into the EMS system.

**Result**

Operational and management functionality could be separated according to organisational role.

---

### Challenge 3 — Persistent Financial State

**Problem**

EMS payments and organisational funds should not exist only as temporary client state.

**Approach**

EMS account and transaction information was connected to persistent database-backed records.

**Result**

Financial activity could remain associated with the EMS organisation across sessions.

---

### Challenge 4 — Vehicle Access

**Problem**

EMS vehicles should not follow exactly the same access model as privately purchased vehicles.

**Approach**

EMS vehicle access was integrated through specialised job and garage workflows.

**Result**

Emergency-service personnel could access operational vehicles while normal ownership rules remained applicable elsewhere.

---

### Challenge 5 — Separating EMS From TDM

**Problem**

Competitive multiplayer elimination and normal RP medical death represent different gameplay contexts.

Allowing both to use the same recovery flow would create conflicts.

**Approach**

TDM combat and respawn state were kept isolated from normal EMS workflows.

**Result**

Competitive sessions could manage their own elimination lifecycle while EMS remained responsible for normal roleplay medical interactions.

---

## Development Approach

The EMS system followed the same iterative engineering process used across Bloodline RP:

    Requirement
        |
        v
    Workflow Design
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
    Debugging / Integration
        |
        v
    Retesting
        |
        v
    Working Behaviour

Because EMS interacts with player state, permissions, finance and vehicles, testing required consideration of the complete workflow rather than only individual functions.

---

## Technical Areas Demonstrated

The EMS & Emergency Services system provides evidence of practical work involving:

- Lua-based FiveM development
- QBCore integration
- Player-state management
- Workflow design
- Role-based permissions
- Staff management
- Duty-state management
- Financial logic
- Persistent transactions
- MySQL-backed records
- Service-history persistence
- Vehicle-system integration
- Job-specific vehicle access
- Character appearance integration
- Cross-resource state management
- Gameplay testing
- Administrative functionality

---

## Production Source

The complete production implementation is maintained privately.

This public case study documents the system architecture, functionality, persistence model and integration approach without distributing proprietary production source code, private player information, database contents, credentials or security-sensitive configuration.

Third-party framework and infrastructure components are not presented as original Bloodline RP development.

---

## Status

**Bloodline EMS & Emergency Services was implemented as part of the completed Bloodline RP server, providing an integrated medical, organisational, financial and emergency-response workflow.**
