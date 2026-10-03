# NPC Marketplace Transaction Integrity — Sanitised Technical Evidence

## Purpose

This document provides a simplified technical representation of the transaction and state-management principles used in the Bloodline RP Interactive NPC Marketplace.

It is **not production source code**.

The pseudocode and diagrams below are sanitised representations of the documented engineering logic. They demonstrate how NPC interaction, temporary basket state, payment validation, inventory changes and transaction completion were coordinated without publishing proprietary Lua implementation, internal event names or security-sensitive configuration.

---

## Engineering Problem

The Bloodline NPC marketplace provides an interactive purchasing workflow rather than a simple static shop.

A simplified player journey is:

    Player Approaches NPC
            |
            v
      Start Interaction
            |
            v
       Open Shop UI
            |
            v
      Build Basket
            |
            v
     Submit Purchase
            |
            v
    Server Validation
            |
            v
      Process Payment
            |
            v
       Grant Items
            |
            v
    Handover / Confirm

The important engineering requirement was that the user interface should represent the player's requested transaction, but should not independently determine whether that transaction is valid.

---

## Client Request vs Server Authority

The marketplace contains information on both the client and server sides.

The client can manage temporary interaction state such as:

- Open shop interface
- Selected products
- Basket quantities
- Displayed totals
- Interface navigation

However, the final transaction requires authoritative validation.

Conceptually:

    Client
      |
      +--> Select Item
      |
      +--> Change Quantity
      |
      +--> Build Basket
      |
      +--> Submit Request
                |
                v
              Server
                |
                +--> Validate Player
                |
                +--> Validate Products
                |
                +--> Validate Quantities
                |
                +--> Calculate Authoritative Total
                |
                +--> Validate Payment
                |
                +--> Validate Inventory
                |
                +--> Commit Transaction

The basket therefore represents a request rather than proof that a purchase is valid.

---

## Temporary Basket State

The shopping basket is temporary state.

Conceptually:

    Shop Opens
        |
        v
    Empty Basket
        |
        v
    Add Products
        |
        v
    Modify Quantities
        |
        v
    Temporary Basket
        |
      +---+---+
      |       |
      v       v
    Cancel   Purchase
      |         |
      v         v
    Discard   Validate

Closing or cancelling the interaction should not itself modify persistent player inventory or money.

---

## Simplified Purchase Request

The production implementation remains private.

Illustrative pseudocode:

    function submitMarketplacePurchase(player, basket):

        if basket is empty:
            reject()
            return

        validatedItems =
            validateRequestedItems(basket)

        if validatedItems are invalid:
            reject()
            return

        total =
            calculateAuthoritativeTotal(
                validatedItems
            )

        if not playerHasRequiredCash(
            player,
            total
        ):
            reject()
            return

        if not inventoryCanAccept(
            player,
            validatedItems
        ):
            reject()
            return

        transaction =
            commitMarketplaceTransaction(
                player,
                validatedItems,
                total
            )

        if transaction failed:
            reject()
            return

        confirmPurchase(player)

This represents the transaction lifecycle rather than the production implementation.

---

## Authoritative Price Validation

The displayed basket total should not be treated as the final authority for payment.

Conceptually:

    Client Basket
         |
         v
    Requested Items
         |
         v
    Server Product Data
         |
         v
    Validate Products
         |
         v
    Calculate Total
         |
         v
    Validate Payment

This reduces dependence on client-provided transaction values.

The server determines whether the requested purchase corresponds to valid marketplace products and quantities.

---

## Transaction Integrity

A marketplace purchase affects at least two important player states:

    Player Money
         +
    Player Inventory

These changes need to correspond to the same successful transaction.

An undesirable state would be:

    Money Removed
         |
         v
    Item Grant Fails
         |
         v
    Player Loses Money
    Without Receiving Items

Another undesirable state would be:

    Item Granted
         |
         v
    Payment Fails
         |
         v
    Player Receives Item
    Without Valid Payment

The transaction therefore needs controlled sequencing and validation.

---

## Simplified Transaction Model

Conceptually:

    Purchase Request
          |
          v
    Validate Products
          |
          v
    Calculate Total
          |
          v
     Validate Cash
          |
          v
    Validate Inventory
          |
          v
    Begin Transaction
          |
          v
      Apply Changes
          |
          v
    Confirm Completion

The transaction should only be presented as successful after the required state changes have completed.

---

## Inventory Capacity

A valid product and sufficient money do not necessarily guarantee that a purchase can complete.

The player's inventory also needs to accept the requested items.

Conceptually:

    Valid Basket
        |
        v
    Enough Cash?
      /      \
    No        Yes
    |          |
    v          v
 Reject    Inventory
           Capacity?
           /      \
         No        Yes
         |          |
         v          v
       Reject     Continue

This prevents the transaction workflow from assuming that item delivery will always succeed.

---

## NPC Interaction Lifecycle

The NPC is more than a visual object.

The interaction lifecycle includes:

    NPC Exists
        |
        v
    Player Enters
    Interaction Range
        |
        v
    Target / Interaction
        |
        v
      Shop UI
        |
        v
    Transaction
        |
        v
    Handover Scene
        |
        v
    Return to Gameplay

Testing therefore included both transaction behaviour and world interaction behaviour.

---

## NPC Placement & Heading

NPC-based systems depend on correct world positioning.

Development required adjusting:

- NPC coordinates
- NPC heading
- Interaction position
- Target accessibility
- Player-facing orientation

A technically functioning marketplace can still provide a poor interaction if the NPC faces the wrong direction or the target area is difficult to access.

This required iterative in-game testing rather than relying only on configuration values.

---

## Handover State

Successful purchases can trigger a handover sequence.

Conceptually:

    Transaction Confirmed
            |
            v
      Start Handover
            |
            v
    Temporary Animation
            |
            v
      Complete Scene
            |
            v
    Clear Temporary State

The handover presentation occurs after the transaction has been validated.

The animation or scene should not itself determine whether the transaction is financially valid.

---

## UI State Management

The marketplace interface creates temporary NUI state.

A simplified lifecycle is:

    Open Marketplace
          |
          v
      NUI Focus
          |
          v
      Basket State
          |
          v
    Purchase / Cancel
          |
          v
       Close UI
          |
          v
    Release NUI Focus
          |
          v
    Normal Gameplay

Testing needs to consider both the transaction and the interface lifecycle.

A successful purchase with incorrectly retained UI focus would still produce a broken player experience.

---

## Vehicle-Key Integration

Some marketplace-related workflows can interact with other server systems such as vehicles and access permissions.

This demonstrates a wider integration principle:

    Marketplace Action
           |
           v
    Server Validation
           |
           v
    Related Resource
           |
           v
    Synchronised State

Where another resource is involved, successful marketplace behaviour depends on both systems agreeing on the resulting state.

This is another example of why cross-resource testing is necessary.

---

## Failure-State Thinking

Useful debugging questions included:

- What happens if the basket is empty?
- What happens if an invalid product is requested?
- What happens if a quantity is invalid?
- What happens if the player lacks enough cash?
- What happens if inventory capacity is insufficient?
- What happens if item delivery fails?
- What happens if payment succeeds but another step fails?
- What happens if the interface closes during the interaction?
- What happens if NUI focus remains active?
- What happens if the NPC interaction target is inaccessible?
- What happens if another integrated resource does not recognise the resulting state?

These questions help expose problems that may not appear when only the successful transaction path is tested.

---

## State Model

The marketplace can be represented as several connected forms of state:

    Player
      |
      +--> Temporary Basket State
      |
      +--> Money State
      |
      +--> Inventory State
      |
      +--> UI State
      |
      +--> Interaction State

The basket and UI are temporary.

Money and inventory changes affect persistent gameplay state.

The transition between them therefore requires validation.

---

## Debugging Approach

Development and testing required examining the complete marketplace lifecycle.

Investigation included:

- NPC placement
- NPC heading
- Interaction targeting
- Shop opening
- Basket behaviour
- Quantity handling
- Product validation
- Price calculation
- Cash validation
- Inventory capacity
- Item delivery
- Transaction completion
- Handover behaviour
- NUI focus
- Integrated resource behaviour

This changed the engineering question from:

    "Does the shop UI work?"

to:

    "Does the complete transaction remain valid across UI, money, inventory and world interaction state?"

---

## Engineering Outcome

The marketplace workflow was developed around authoritative transaction validation rather than trusting temporary client-side basket state.

The work demonstrates practical experience involving:

- Client/server architecture
- Transaction validation
- Server-authoritative state
- Temporary versus persistent state
- Inventory integration
- Economy integration
- Failure-state handling
- NPC interaction systems
- World-coordinate debugging
- UI state management
- Cross-resource integration
- End-to-end gameplay testing

---

## Relationship to Other Bloodline Systems

The NPC marketplace is part of a wider integrated server architecture.

    NPC Marketplace
          |
          +--> Interaction System
          |
          +--> NUI
          |
          +--> Player Economy
          |
          +--> Inventory
          |
          +--> World NPC State
          |
          +--> Related Resources

The system is documented separately because it provides a useful example of transaction integrity across several gameplay components.

---

## Supporting Documentation

See:

- [Interactive NPC Marketplace](../../docs/interactive-npc-marketplace.md)
- [Engineering Challenges](../engineering-challenges.md)
- [Development Timeline](../development-timeline.md)
- [Engineering Evidence Index](../evidence-index.md)

---

## Evidence Boundary

This document intentionally does not publish:

- Production Lua source
- Internal server event names
- Marketplace security logic
- Private configuration
- Database credentials
- Server credentials
- Player identifiers
- Private inventory data
- Anti-cheat or security implementation

The pseudocode documents the engineering architecture, transaction lifecycle and debugging principles without reproducing the private production implementation.
