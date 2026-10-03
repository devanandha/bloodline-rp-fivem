# Vehicle Ownership & Key Synchronisation — Sanitised Technical Evidence

## Purpose

This document provides a simplified technical representation of an integration problem solved during development of the Bloodline RP vehicle ecosystem.

It is **not production source code**.

The pseudocode and diagrams below are sanitised representations of the engineering logic. They demonstrate the relationship between vehicle purchasing, persistent ownership and vehicle-key access without publishing proprietary Lua implementation, internal event names, server configuration or security-sensitive logic.

---

## Engineering Problem

Vehicle purchasing in Bloodline RP involves more than spawning a vehicle after payment.

A successful purchase needs to establish several connected states:

    Player Selects Vehicle
             |
             v
      Purchase Request
             |
             v
      Validate Purchase
             |
             v
        Process Payment
             |
             v
     Create Ownership
             |
             v
      Persist Vehicle
             |
             v
       Grant Access
             |
             v
        Vehicle Keys

During development, testing exposed an integration problem where the vehicle purchase could complete successfully while immediate key access did not correctly reflect the newly created ownership state.

This meant that two systems could individually appear to work while the complete player workflow remained incorrect.

---

## Cross-Resource State

The purchase system and key system represent related but different responsibilities.

Conceptually:

    Vehicle Shop
        |
        +--> Purchase Validation
        |
        +--> Payment
        |
        +--> Vehicle Creation
        |
        +--> Persistent Ownership

                |
                v

          Vehicle Key System
                |
                +--> Ownership Recognition
                |
                +--> Access Permission
                |
                +--> Lock / Unlock
                |
                +--> Engine Access

The important engineering requirement was ensuring that the ownership transition became visible to the access system at the correct time.

---

## Desired Purchase Lifecycle

The intended workflow can be represented as:

    Purchase Request
          |
          v
    Validate Player
          |
          v
    Validate Vehicle
          |
          v
    Validate Funds
          |
          v
     Process Payment
          |
          v
    Create Vehicle Record
          |
          v
    Confirm Ownership
          |
          v
    Synchronise Key Access
          |
          v
      Purchase Complete

The purchase should not be treated as fully complete merely because payment succeeded.

The newly purchased vehicle also needs to become usable by its owner.

---

## Simplified Purchase Logic

The production implementation remains private.

Illustrative pseudocode:

    function purchaseVehicle(player, vehicleRequest):

        if not validVehicle(vehicleRequest):
            reject()
            return

        price = getAuthoritativePrice(vehicleRequest)

        if not playerHasRequiredFunds(player, price):
            reject()
            return

        vehicleIdentity =
            generateVehicleIdentity(vehicleRequest)

        ownershipCreated =
            createPersistentOwnership(
                player,
                vehicleIdentity
            )

        if not ownershipCreated:
            reject()
            return

        processPayment(
            player,
            price
        )

        synchroniseVehicleAccess(
            player,
            vehicleIdentity
        )

        confirmPurchase(
            player,
            vehicleIdentity
        )

This pseudocode demonstrates the lifecycle rather than reproducing the actual production implementation.

---

## Ownership State

A purchased vehicle represents persistent state.

Conceptually:

    Player
      |
      v
    Vehicle Ownership Record
      |
      +--> Vehicle Identity
      |
      +--> Owner Identity
      |
      +--> Vehicle Data
      |
      +--> Persistent Garage State

The access system needs to recognise this persistent relationship.

The important distinction is:

    Vehicle Exists

does not necessarily mean:

    Player Owns Vehicle

and:

    Player Owns Vehicle

does not automatically guarantee:

    Key System Has Updated Access State

These states need to be coordinated.

---

## Synchronisation Problem

The development issue can be simplified as:

    Purchase Succeeds
          |
          v
    Ownership Created
          |
          v
    Vehicle Spawned
          |
          v
    Player Attempts Access
          |
          v
    Key System Does Not Yet
    Reflect New Ownership

From the player's perspective, the purchase appeared incomplete even though the purchasing resource had completed its own operation.

This is an example of a cross-resource integration problem.

---

## Synchronisation Principle

The workflow was revised around the principle that access state should be updated as part of the successful ownership transition.

Conceptually:

    Persistent Ownership Created
               |
               v
       Ownership Confirmed
               |
               v
        Notify / Synchronise
          Access System
               |
               v
       Key State Updated
               |
               v
       Player Can Operate
          Owned Vehicle

Illustrative pseudocode:

    function synchroniseVehicleAccess(player, vehicle):

        ownership =
            lookupVehicleOwnership(vehicle)

        if ownership does not belong to player:
            reject()
            return

        grantVehicleAccess(
            player,
            vehicle
        )

        confirmAccessState(
            player,
            vehicle
        )

The key concept is that access is derived from a validated ownership relationship rather than from the visual presence of the vehicle alone.

---

## Persistent Ownership vs Temporary Access

The vehicle ecosystem also includes test-drive vehicles.

A test drive creates a different access problem.

The player needs temporary permission to use the vehicle without becoming its permanent owner.

The system therefore needs to distinguish:

    Purchased Vehicle
          |
          v
    Persistent Ownership
          |
          v
    Persistent Owner Access

from:

    Test-Drive Vehicle
          |
          v
    Temporary Permission
          |
          v
    Temporary Vehicle Access

This distinction is important because normal ownership checks should not incorrectly prevent legitimate test-drive usage.

---

## Simplified Test-Drive State

Illustrative pseudocode:

    function startTestDrive(player, vehicle):

        createTemporaryTestDriveState(
            player,
            vehicle
        )

        grantTemporaryVehicleAccess(
            player,
            vehicle
        )

        startTestDriveTimer()

The vehicle is usable during the test-drive lifecycle, but this does not create normal purchased ownership.

---

## Test-Drive Cleanup

Temporary access should disappear when the test drive ends.

Conceptually:

    Start Test Drive
          |
          v
    Temporary Access
          |
          v
    Player Tests Vehicle
          |
          v
    Test Drive Ends
          |
          v
    Remove Temporary Access
          |
          v
    Remove / Return Vehicle

Illustrative pseudocode:

    function endTestDrive(player, vehicle):

        removeTemporaryAccess(
            player,
            vehicle
        )

        clearTestDriveState(
            player
        )

        cleanupTestVehicle(
            vehicle
        )

The temporary permission should not become permanent ownership.

---

## Ownership Validation

Vehicle access decisions should consider the context in which the vehicle exists.

A simplified access model is:

    Player Requests Vehicle Access
                |
                v
        Access Context?
        /             \
    Permanent        Temporary
    Ownership        Test Drive
       |                |
       v                v
    Validate          Validate
    Ownership         Session
       |                |
       +-------+--------+
               |
               v
          Allow Access

Other legitimate access contexts can be handled separately according to server rules.

The important engineering principle is that access should correspond to a valid gameplay relationship.

---

## Plate / Vehicle Identity Consistency

Vehicle integrations frequently depend on a shared vehicle identifier.

If different resources interpret that identifier differently, ownership and access checks can become inconsistent.

Conceptually:

    Vehicle Shop
         |
         v
    Vehicle Identifier
         |
         +----------+
         |          |
         v          v
      Database    Key System
         |          |
         +----+-----+
              |
              v
       Same Identity

Consistent vehicle identity handling is therefore important when coordinating multiple resources.

---

## Vehicle Lifecycle

The ownership/key issue is part of a wider vehicle lifecycle:

    Vehicle Shop
         |
         v
      Purchase
         |
         v
      Ownership
         |
         v
        Keys
         |
         v
       Garage
         |
         v
       Engine
         |
         v
        Fuel
         |
         v
    Continued Use

A failure at one integration boundary can affect the complete lifecycle even when the individual resources remain operational.

---

## Debugging Approach

Resolving vehicle-access issues required examining the workflow across multiple resources rather than only the vehicle shop.

The investigation included:

- Purchase completion
- Payment state
- Vehicle identity
- Ownership persistence
- Vehicle spawning
- Key/access state
- Plate/identifier consistency
- Test-drive state
- Temporary permissions
- Engine access
- Garage ownership behaviour

The debugging question therefore changed from:

    "Why can't the player use the vehicle?"

to:

    "At what point does persistent ownership become recognised by every resource that depends on it?"

This helped isolate the integration boundary.

---

## Failure-State Thinking

A reliable workflow needs to consider partial completion.

Examples of undesirable states include:

    Payment Completed
          |
          v
    Ownership Not Created

or:

    Ownership Created
          |
          v
    Access Not Synchronised

or:

    Test Drive Started
          |
          v
    Temporary Access Missing

or:

    Test Drive Ended
          |
          v
    Temporary Access Remains

Thinking about these intermediate states helped identify where lifecycle coordination was required.

---

## Engineering Outcome

The vehicle workflow was developed around the relationship between persistent ownership and vehicle access rather than treating purchasing and keys as completely independent features.

The work demonstrates practical experience involving:

- Persistent ownership
- Cross-resource synchronisation
- Vehicle identity management
- Client/server coordination
- Access validation
- Temporary permissions
- Test-drive lifecycle management
- Database-backed state
- Transaction sequencing
- Failure-state analysis
- Integration debugging

---

## Relationship to the Wider Vehicle Ecosystem

The ownership/key synchronisation problem forms part of the broader Bloodline vehicle architecture.

    Vehicle Ecosystem
          |
          +--> Vehicle Shop
          |
          +--> Ownership
          |
          +--> Keys
          |
          +--> Test Drives
          |
          +--> Garage
          |
          +--> Engine
          |
          +--> Fuel

The issue is documented separately because it demonstrates how a seemingly small player-facing problem can originate from state synchronisation between multiple resources.

---

## Supporting Documentation

See:

- [Vehicle Ecosystem](../../docs/vehicle-ecosystem.md)
- [Engineering Challenges](../engineering-challenges.md)
- [Development Timeline](../development-timeline.md)
- [Engineering Evidence Index](../evidence-index.md)

---

## Evidence Boundary

This document intentionally does not publish:

- Production Lua source
- Internal event names
- Vehicle-key implementation details
- Security logic
- Resource configuration
- Database credentials
- Server credentials
- Player identifiers

The pseudocode describes the engineering architecture, state transitions and integration problem rather than reproducing the private production implementation.
