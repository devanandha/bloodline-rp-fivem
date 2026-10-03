# Fuel Jerrycan Persistence — Sanitised Technical Evidence

## Purpose

This document provides a simplified technical representation of one of the persistence problems solved during development of the Bloodline RP fuel system.

It is **not production source code**.

The examples below are sanitised pseudocode designed to demonstrate the engineering logic without publishing proprietary implementation details, server configuration or security-sensitive code.

---

## Engineering Problem

The Bloodline fuel system supports reusable jerrycans whose remaining fuel capacity needs to persist correctly.

During development, an important problem appeared when remaining fuel existed only as temporary interaction state.

A problematic lifecycle could look like:

    Jerrycan Capacity = 100
            |
            v
       Player Uses 40
            |
            v
    Remaining Capacity = 60
            |
            v
    Inventory Item Removed /
          Re-added
            |
            v
    Incorrect Reset = 100

This would allow the item's fuel state to become inconsistent with its actual previous usage.

The engineering requirement was therefore:

> Remaining fuel must follow the persistent inventory item rather than exist only inside the current client interaction.

---

## State Model

The system can be represented using three layers of state.

    Player Interaction
            |
            v
    Temporary Fuel Operation
            |
            v
    Server Validation
            |
            v
    Persistent Item Metadata

The important distinction is:

    Temporary
    ---------
    Active refuelling
    Nozzle interaction
    Current transfer operation

    Persistent
    ----------
    Jerrycan remaining capacity
    Item identity / inventory state

---

## Simplified Persistence Logic

The production implementation remains private.

The following pseudocode illustrates the persistence principle:

    function useJerrycan(player, item, requestedAmount):

        storedFuel = readPersistentFuel(item)

        if storedFuel <= 0:
            reject("Jerrycan is empty")
            return

        amountToUse = minimum(requestedAmount, storedFuel)

        performFuelTransfer(amountToUse)

        remainingFuel = storedFuel - amountToUse

        updateItemMetadata(
            item,
            remainingFuel
        )

The important operation is not the subtraction itself.

The important engineering decision is that:

    remainingFuel

is written back to persistent inventory state.

---

## Preventing State Reset

When the item returns to inventory, the system should recover the existing stored value rather than blindly assigning the default capacity.

Conceptually:

    Item Loaded
        |
        v
    Existing Fuel Metadata?
       /             \
     Yes              No
      |                |
      v                v
    Restore         Initialise
    Existing         Default
     Value           Capacity

Simplified pseudocode:

    function loadJerrycan(item):

        if item.hasFuelMetadata:
            capacity = item.savedFuel
        else:
            capacity = DEFAULT_CAPACITY

        return capacity

This prevents an already-used item from being treated as a newly created full jerrycan.

---

## Server Authority

Persistent gameplay state should not depend entirely on a value supplied by the client.

A simplified model is:

    Client Requests Fuel Use
              |
              v
        Server Receives
              |
              v
      Validate Inventory
              |
              v
       Read Stored State
              |
              v
      Validate Requested Use
              |
              v
       Update Persistent
            Metadata
              |
              v
        Confirm Result

Illustrative pseudocode:

    onFuelUseRequest(player, itemReference, requestedAmount):

        item = serverInventoryLookup(
            player,
            itemReference
        )

        if item does not exist:
            reject()
            return

        availableFuel = readFuelMetadata(item)

        validAmount = validateFuelTransfer(
            requestedAmount,
            availableFuel
        )

        newFuelLevel =
            availableFuel - validAmount

        saveFuelMetadata(
            item,
            newFuelLevel
        )

        confirmFuelTransfer(
            player,
            validAmount
        )

Again, this is architectural pseudocode rather than the production implementation.

---

## Transaction Principle

The fuel operation can be considered as a state transition:

    BEFORE

    Jerrycan
    fuel = 60

        |
        | Player transfers 20
        v

    VALIDATE OPERATION

        |
        v

    AFTER

    Jerrycan
    fuel = 40

The persistent state should represent the completed operation.

If the operation does not complete successfully, the persistent value should not incorrectly represent fuel that was never transferred.

---

## Debugging Approach

Resolving the issue required examining the complete item lifecycle rather than only the visible refuelling interaction.

The investigation included:

- How the jerrycan entered inventory
- Where remaining capacity was stored
- How capacity changed during use
- What happened when the item was removed
- What happened when it was added again
- Whether default values overwrote existing values
- Which state was controlled by the client
- Which state needed server-side persistence

This transformed the problem from:

    "Why did the fuel number reset?"

into:

    "Where is the authoritative state stored throughout the complete item lifecycle?"

That distinction was important to resolving the issue.

---

## Engineering Outcome

The revised architecture treated remaining jerrycan capacity as persistent item state.

This allowed the remaining capacity to survive inventory lifecycle operations instead of behaving only as temporary client state.

The case demonstrates practical work involving:

- Persistent inventory metadata
- Client/server state separation
- Server-side validation
- Stateful item behaviour
- Transaction-style updates
- Inventory integration
- Debugging across resource boundaries
- Failure-state investigation

---

## Relationship to the Wider Fuel System

Jerrycan persistence is only one part of the wider Bloodline petroleum architecture.

    Petroleum Economy
          |
          +--> Central Depot
          |
          +--> Fuel Businesses
          |
          +--> Station Stock
          |
          +--> Vehicle Refuelling
          |
          +--> Jerrycan State

The persistence problem is documented separately because it provides a focused example of debugging state across inventory and fuel systems.

---

## Supporting Documentation

See:

- [Fuel & Petroleum Economy](../../docs/fuel-petroleum-economy.md)
- [Engineering Challenges](../engineering-challenges.md)
- [Development Timeline](../development-timeline.md)
- [Engineering Evidence Index](../evidence-index.md)

---

## Evidence Boundary

This document intentionally does not publish:

- Production Lua source
- Resource configuration
- Security logic
- Server credentials
- Inventory identifiers
- Private player data
- Database credentials

The pseudocode describes the engineering pattern and state lifecycle rather than reproducing the private production implementation.
