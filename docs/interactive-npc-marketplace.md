# Bloodline Interactive NPC Marketplace & Transaction System

## Overview

The Bloodline Interactive NPC Marketplace & Transaction System is a configurable NPC-based commerce system developed for Bloodline RP.

The system was designed to turn an NPC interaction into a complete transactional workflow rather than a simple item-selection menu.

It combines:

- NPC world interaction
- Product selection
- Basket state
- Payment validation
- Inventory delivery
- Handover animations
- Management controls
- Administrative configuration
- World placement
- Cross-resource integration

A simplified transaction lifecycle is:

    Player Approaches NPC
              |
              v
       Interaction Available
              |
              v
        Open Marketplace
              |
              v
        Product Selection
              |
              v
          Basket State
              |
              v
       Validate Transaction
              |
         +----+----+
         |         |
         v         v
       Fail      Success
         |         |
         v         v
      Reject    Payment
                   |
                   v
             Item Delivery
                   |
                   v
            Handover Scene

The implementation required coordination between the game world, interface state, player inventory, payment state and server-side transaction logic.

---

## Core Features

The marketplace includes:

- Interactive NPC
- Configurable products
- Product pricing
- Basket/cart functionality
- Cash transactions
- Transaction validation
- Inventory delivery
- Handover animations
- NPC placement
- Interaction targeting
- Management functionality
- Administrative controls
- Configurable marketplace behaviour
- Cross-resource integration

The objective was to provide a more immersive transaction experience than a standard static shop interface.

---

## System Architecture

    World NPC
       |
       v
    Player Interaction
       |
       v
    Marketplace UI
       |
       +-- Products
       +-- Prices
       +-- Basket
       |
       v
    Purchase Request
       |
       v
    Server Validation
       |
    +--+----------------+
    |                   |
    v                   v
Payment Validation   Inventory Validation
    |                   |
    +---------+---------+
              |
              v
       Complete Transaction
              |
         +----+----+
         |         |
         v         v
      Payment    Item Delivery
         |         |
         +----+----+
              |
              v
       Handover Sequence

---

## NPC Interaction

The marketplace begins with a physical NPC placed within the game world.

Instead of opening the marketplace globally, players interact with the configured NPC.

This connects the user interface to a specific world entity.

The interaction lifecycle can be represented as:

    NPC Spawned
        |
        v
    Player Approaches
        |
        v
    Interaction Target
        |
        v
    Marketplace Access

This makes world placement and interaction state part of the marketplace architecture.

---

## Marketplace Interface

The marketplace provides a user-facing interface for browsing available products and preparing a transaction.

The interface manages temporary state such as:

- Available products
- Product quantities
- Selected items
- Basket contents
- Total transaction value

Basket state exists before the final transaction and should therefore remain separate from persistent inventory state.

Conceptually:

    Product Catalogue
           |
           v
    Player Selection
           |
           v
      Basket State
           |
           v
       Confirmation
           |
           v
    Server Transaction

---

## Basket State

The basket allows multiple selection decisions to be represented before committing the final purchase.

This creates an important distinction between:

    Intended Purchase

and:

    Completed Purchase

Adding an item to a basket does not mean the item has been delivered.

The actual inventory and financial state should change only after the transaction has been validated and completed.

---

## Transaction Lifecycle

A marketplace transaction crosses several systems.

    Basket
      |
      v
    Purchase Request
      |
      v
    Validate Request
      |
      v
    Validate Payment
      |
      v
    Process Payment
      |
      v
    Deliver Items
      |
      v
    Complete Interaction

This required careful handling because the UI, player money and inventory represent different forms of state.

---

## Server-Side Validation

The marketplace should not rely entirely on values supplied by the user interface.

Important transaction information can be validated through server-side logic before the purchase is completed.

This includes concepts such as:

- Requested product
- Quantity
- Price
- Available funds
- Inventory delivery

The intended model is:

    Client Request
         |
         v
    Server Validation
         |
     +---+---+
     |       |
     v       v
   Invalid  Valid
     |       |
     v       v
   Reject  Process

This helps keep persistent transaction state separate from temporary client interface state.

---

## Cash Transaction Handling

The marketplace includes cash-based purchasing.

A successful transaction therefore needs to coordinate two important state changes:

    Player Money
        |
        v
      Payment

and:

    Marketplace
        |
        v
    Item Delivery

The transaction should only be considered complete when the required conditions have been satisfied.

This creates a gameplay equivalent of a transactional workflow.

---

## Transaction Integrity

A key design consideration is avoiding partial transactions.

For example, an undesirable state would be:

    Payment Removed
          |
          v
    Item Delivery Fails

or:

    Item Delivered
          |
          v
    Payment Not Processed

The marketplace workflow therefore needs to coordinate payment and item delivery so that the final state represents the intended purchase.

This is similar to the transaction-integrity considerations used elsewhere in Bloodline RP, including persistent tent placement and vehicle purchasing.

---

## Handover Sequence

Successful purchases include a physical handover interaction.

Instead of ending immediately after a menu confirmation, the marketplace can transition into an in-world exchange sequence.

Conceptually:

    Purchase Successful
             |
             v
       Prepare Scene
             |
             v
      NPC / Player State
             |
             v
     Handover Animation
             |
             v
       Transaction End

This improves immersion while introducing additional temporary gameplay state that must be cleaned up correctly after the interaction.

---

## Interaction State

During a transaction, the system may temporarily control aspects of player or NPC behaviour.

Temporary interaction state can include:

- Animation state
- Positioning
- Interface state
- Transaction state
- NPC behaviour

This state should end when the interaction completes or is cancelled.

It must not become part of the player's persistent state.

---

## NPC World Placement

The marketplace NPC needs to exist at an appropriate world location and orientation.

Development therefore included work around:

- NPC coordinates
- Heading
- Interaction position
- Player approach
- Visual placement
- Interaction range

Small coordinate or orientation differences can significantly affect whether an NPC interaction feels natural.

World placement therefore required in-game testing rather than relying only on configuration values.

---

## World Placement Debugging

### Problem

An NPC can technically spawn successfully while still being poorly positioned for player interaction.

Potential issues include:

- Incorrect heading
- Unnatural positioning
- Interaction point misalignment
- Player positioning during transactions
- Animation alignment

### Approach

Placement and interaction behaviour were tested in the actual game environment and adjusted iteratively.

### Result

The marketplace interaction could be positioned more naturally within the intended environment.

---

## Interaction Targeting

The NPC marketplace depends on reliable interaction detection.

The interaction system needs to identify when the player is attempting to use the marketplace and associate that action with the correct NPC.

This introduces integration between:

- World entity
- Targeting/interaction system
- Marketplace resource
- User interface

A failure in any one of these layers can make an otherwise functional marketplace inaccessible.

---

## Management Functionality

The marketplace includes management functionality beyond ordinary customer interaction.

Management state can be separated from the normal purchasing workflow.

Conceptually:

    Marketplace
       |
       +--> Customer Interface
       |
       +--> Management Functions
       |
       +--> Administrative Functions

This allows different levels of functionality to be exposed according to the user's role or permissions.

---

## Administrative Controls

Administrative functionality supports controlled management of the marketplace system.

Administrative operations are separated from ordinary transactions so players cannot access management functionality simply because they can interact with the NPC.

This demonstrates role-based separation between:

- Customer actions
- Management actions
- Administrative actions

---

## Vehicle-Key Integration

Development also required integration with Bloodline vehicle-access behaviour in relevant interaction scenarios.

This demonstrates an important characteristic of the wider server architecture: individual systems do not operate entirely independently.

Where a marketplace interaction intersects with vehicle state, the correct vehicle-access behaviour must continue to be respected.

This required consideration of the Bloodline Vehicle Ecosystem rather than treating the marketplace as an isolated resource.

---

## Cross-System Integration

The marketplace interacts with multiple parts of the server.

Examples include:

- QBCore
- Player inventory
- Player cash
- NPC entities
- Interaction targeting
- User interface
- Animations
- Permissions
- Notifications
- Vehicle access

A typical purchase may cross these systems:

    NPC
     |
     v
    Interaction System
     |
     v
    Marketplace UI
     |
     v
    Server Transaction
     |
    +----------------+
    |                |
    v                v
 Player Money     Inventory
    |                |
    +-------+--------+
            |
            v
     Handover Scene

This makes the marketplace a useful example of multi-resource gameplay integration.

---

## Selected Engineering Challenges

### Challenge 1 — Coordinating UI and Server State

**Problem**

The basket exists in the interface, but money and inventory represent authoritative gameplay state.

Treating the basket itself as a completed transaction could create inconsistent state.

**Approach**

The purchase workflow separates temporary basket selection from server-side transaction processing.

**Result**

UI selection and persistent gameplay state remain logically distinct.

---

### Challenge 2 — Transaction Integrity

**Problem**

Payment and item delivery must remain coordinated.

A partially completed transaction could result in either lost money or unintended items.

**Approach**

The purchase process was structured as a controlled sequence of validation, payment and delivery.

**Result**

Marketplace purchases follow a defined transactional lifecycle.

---

### Challenge 3 — NPC Placement

**Problem**

Correct configuration values do not necessarily produce a natural in-game interaction.

**Approach**

NPC position, orientation and interaction behaviour were tested and revised within the game world.

**Result**

World interaction became better aligned with the intended player experience.

---

### Challenge 4 — Handover Scene State

**Problem**

Adding an animated handover creates additional temporary state after the transaction itself has been approved.

**Approach**

The handover was treated as a defined stage of the marketplace lifecycle.

**Result**

The transaction could transition from interface-based purchasing into an in-world interaction and then return to normal gameplay.

---

### Challenge 5 — Cross-Resource Behaviour

**Problem**

Marketplace interactions can intersect with other server systems, including inventory, notifications and vehicle access.

**Approach**

The marketplace was tested as part of the wider Bloodline environment rather than only as a standalone resource.

**Result**

The system could participate in the broader server architecture while maintaining its own transaction lifecycle.

---

## Development Approach

The marketplace evolved through repeated implementation and in-game testing.

A typical development cycle was:

    Requirement
        |
        v
    Initial Marketplace
        |
        v
    In-Game Testing
        |
        v
    Interaction / State Issue
        |
        v
    Investigation
        |
        v
    Code Revision
        |
        v
    Transaction Testing
        |
        v
    UI / World Testing
        |
        v
    Working Behaviour

Development continued beyond basic purchasing functionality into presentation, world placement, interaction behaviour and integration.

---

## Technical Areas Demonstrated

The Interactive NPC Marketplace provides evidence of practical work involving:

- Lua-based FiveM development
- QBCore integration
- NPC entity management
- World-coordinate handling
- Interaction targeting
- Client/server communication
- Server-side validation
- Transaction logic
- Inventory integration
- Player economy integration
- Temporary basket state
- UI integration
- Animation and scene handling
- Permission management
- Administrative functionality
- Cross-resource integration
- Gameplay testing
- Iterative debugging

---

## Security & Privacy

The public repository does not expose:

- Production source code
- Administrative credentials
- Server credentials
- Private player data
- Security configuration
- Private transaction records

The marketplace is documented at the architectural and engineering level.

---

## Production Source

The complete production implementation is maintained privately.

This public case study documents the architecture, transactional workflow, NPC interaction model, integrations and development process without distributing proprietary production source code or security-sensitive configuration.

QBCore and other third-party infrastructure are not presented as original Bloodline RP development.

---

## Status

**The Bloodline Interactive NPC Marketplace & Transaction System was implemented as part of the completed Bloodline RP server, providing an integrated NPC commerce workflow combining world interaction, basket management, server-side transactions, inventory delivery and immersive handover behaviour.**
