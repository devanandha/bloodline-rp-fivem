# Bloodline RP — System Architecture

## Overview

Bloodline RP was developed as a modular FiveM/QBCore multiplayer server in which gameplay systems operate as separate resources while sharing core framework services, persistent data and cross-resource events.

The objective was not simply to install independent scripts, but to create a functioning server environment in which systems such as vehicles, fuel, characters, emergency services, businesses, storage and competitive multiplayer could operate together.

---

## High-Level Architecture

    BLOODLINE RP
         |
         +-----------------------+
         |                       |
       QBCore                  MySQL
         |                       |
         |                 Persistent State
         |
         +-- Player Systems
         |     +-- Dual Character
         |     +-- Appearance
         |     +-- Inventory
         |
         +-- Vehicle Systems
         |     +-- Vehicle Shop
         |     +-- Vehicle Keys
         |     +-- Garage
         |     +-- Vehicle Engine
         |
         +-- Economy Systems
         |     +-- Fuel Company
         |     +-- Fuel Businesses
         |     +-- Electronics
         |
         +-- EMS & Emergency Services
         |
         +-- Persistent Tent Storage
         |
         +-- Interactive NPC Systems
         |
         +-- TDM & Multiplayer Sessions

---

## Core Platform

### FiveM

FiveM provides the multiplayer runtime used by Bloodline RP.

### QBCore

QBCore provides the underlying roleplay framework and common player, job and gameplay services used throughout the server.

Bloodline-specific resources were developed and integrated around this framework.

### MySQL

Persistent server state is stored in MySQL.

Database-backed functionality includes areas such as:

- Character information
- Character-slot state
- Vehicle ownership
- Business data
- Fuel-company data
- Fuel-station data
- Staff and manager information
- Sales and transaction history
- Persistent storage

---

## Major System Relationships

### Vehicle Architecture

The vehicle environment consists of several cooperating resources rather than one monolithic vehicle script.

    Vehicle Shop
         |
         v
    Vehicle Purchase
         |
         +------------------> Persistent Vehicle Ownership
         |                              |
         v                              v
    Vehicle Keys <------------------ Garage
         |                              |
         v                              v
    Access / Locking              Store / Retrieve
         |
         v
    Vehicle Engine
         |
         v
    Fuel State

This architecture required cross-resource handling for purchasing, ownership, key assignment, test drives, job vehicles, garage retrieval, engine behaviour and fuel state.

---

### Fuel & Petroleum Economy

The fuel system was designed as an interconnected economic chain.

    Fuel Company
         |
         | Wholesale Supply
         v
    Fuel Businesses
         |
         | Station Inventory
         v
    Fuel Stations
         |
         | Retail Purchase
         v
    Player Vehicles

The broader system incorporates fuel inventory, company finances, wholesale pricing, station stock, tanker delivery, vehicle refuelling and fuel consumption.

---

### Character Architecture

    Player Connection
         |
         v
    Character Selector
         |
         +-- Slot 1
         |
         +-- Slot 2 (Controlled Unlock)
         |
         v
    Ownership Validation
         |
         v
    Character Data
         |
         +-- Identity
         +-- Job
         +-- Money
         +-- Metadata
         +-- Position
         +-- Appearance
         |
         v
    Spawn Selection

Character state is persisted and validated before the selected character enters the server environment.

---

### EMS Architecture

    Player Emergency
         |
         v
    Bloodline EMS
         |
         +-- Death / Emergency Flow
         +-- CPR / Revival
         +-- Treatment
         +-- Duty Management
         +-- Staff Permissions
         +-- Payments
         +-- Uniforms
         +-- EMS Vehicles

The EMS system connects emergency gameplay with staff management, organisational finances and vehicle access.

---

### TDM Isolation Architecture

The TDM environment was intentionally separated from normal RP state.

    Normal RP
         |
         v
    TDM Entry
         |
         v
    Session / Routing Bucket
         |
         +-- Private Team Room
         |
         +-- Public FFA
         |
         v
    Temporary TDM State
         |
         +-- Weapon
         +-- Ammunition
         +-- Damage Rules
         +-- Team State
         +-- Score
         +-- Spawn / Zone Rules
         |
         v
    TDM Exit
         |
         v
    Restore Normal RP State

Normal RP inventory, economy, jobs, garages and EMS functionality remain isolated from the competitive environment.

---

## Cross-System Integration

One of the primary engineering challenges in Bloodline RP was ensuring that independently operating resources maintained consistent player and vehicle state.

Examples include:

- Vehicle purchases issuing immediate vehicle access.
- Garage-spawned vehicles synchronising with the key system.
- Police and EMS vehicles receiving job-specific access rules.
- Test-drive vehicles bypassing normal ownership and hotwire behaviour temporarily.
- Fuel state affecting engine behaviour.
- Character appearance persisting across character sessions.
- TDM state remaining isolated from normal RP systems.
- Inventory interfaces cooperating with custom NUI resources.

---

## Persistence

Persistent data is used where gameplay state must survive reconnects or server restarts.

Examples include:

- Character information
- Character-slot access
- Vehicle ownership
- Business configuration
- Business transactions
- Fuel-company inventory
- Station inventory
- Staff permissions
- Tent ownership and storage

Temporary state is used where persistence would be undesirable, such as TDM weapons, temporary test-drive access and session-specific multiplayer state.

---

## Design Principles

### Modular Resources

Major gameplay functionality was separated into dedicated resources.

### Persistent Where Necessary

Ownership, characters, businesses and other long-term state were database-backed.

### Temporary Where Appropriate

Competitive gameplay and test-drive state were kept separate from permanent RP state.

### Cross-System Compatibility

Resources were tested together rather than treated as isolated components.

### Failure Recovery

Where practical, systems included handling for disconnects, resource restarts, invalid state and interrupted gameplay flows.

### Private Production Implementation

The production source remains private. Public documentation describes architecture and engineering behaviour without exposing proprietary implementation or sensitive server configuration.

---

## Architecture Status

This document represents the high-level architecture of the completed Bloodline RP server.

Individual system case studies provide more detailed information about design decisions, integration challenges, debugging and validation.
