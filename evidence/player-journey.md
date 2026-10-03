# Bloodline RP — Player Journey & Integrated Gameplay Loop

## Purpose

This document presents the Bloodline RP player journey as an integrated gameplay loop rather than as a collection of isolated resources.

The documented systems connect character identity, spawning, jobs, money, shops, survival needs, vehicles, emergency services and persistent state.

---

## End-to-End Player Journey

A simplified player journey is:

    Player Connects
          |
          v
    Character System
          |
          v
    Character Selection
          |
          v
    Appearance Loading
          |
          v
    Spawn Selection
          |
          v
    Enter Roleplay World
          |
          +-----------------------------+
          |                             |
          v                             v
      Find Work                    Explore / RP
          |                             |
          v                             |
      Earn Money                       |
          |                             |
          +-------------+---------------+
                        |
                        v
                 Shops / Services
                        |
             +----------+----------+
             |          |          |
             v          v          v
           Food       Drinks    Other Items
             |          |          |
             +----------+----------+
                        |
                        v
                Continue Gameplay

At the same time, players can interact with vehicles, businesses, housing/gang systems, emergency services, police/crime systems, communication systems and competitive gameplay.

---

## 1A. Whitelist & First-Entry Onboarding

Bloodline RP uses a whitelist-based entry process before a player can enter the city.

The player journey begins outside the game:

    Discord Community
          |
          v
    Whitelist Application
          |
          v
    Admin Review
          |
          v
    Whitelist Granted
       [Passport Role]
          |
          v
    City Entry Link
          |
          v
    Character Creation
          |
          v
    Appearance / Clothing
          |
          v
    Enter Bloodline RP
          |
          v
    Starter Provisioning
       +-------------+
       |             |
       v             v
    Custom Car     $10,000 Cash
                     +
                   $10,000 Bank
          |
          v
    Begin Roleplay

The whitelist stage provides an access-control boundary between the public Discord community and the in-game city.

After approval, the player receives the required whitelist/passport role and the route into the city. The player then completes character creation and appearance/clothing selection before entering the roleplay environment.

On initial entry, the player receives the documented starter provision:

- A custom-made starter car
- $10,000 cash
- $10,000 bank balance

After onboarding, the player is free to participate in the wider Bloodline RP environment subject to the server's roleplay rules.

This creates a clear transition:

    Community
       |
       v
    Whitelist
       |
       v
    Identity Creation
       |
       v
    Starter Provisioning
       |
       v
    Open Roleplay

This onboarding flow should be considered part of the overall player journey rather than a separate administrative process.

---

## 1. Character Entry

The documented character lifecycle begins when a player connects to the server.

The Bloodline Dual Character System manages:

- Character ownership
- Character-slot permissions
- Persistent character data
- Appearance state
- Character selection
- Spawn selection
- Character loading
- Administrative controls
- Recovery from invalid appearance state

The documented lifecycle is:

    Player Connects
          |
          v
    Character System
          |
      +---+---+
      |       |
      v       v
    Slot 1  Slot 2
      |       |
      +---+---+
          |
          v
    Character Selection
          |
          v
    Appearance Loading
          |
          v
    Spawn Selection
          |
          v
      Enter World

Source: `docs/dual-character-system.md`

---

## 2. Entering the Roleplay World

Once the selected character is loaded, the player enters the wider Bloodline RP environment.

The architecture documentation describes Bloodline RP as a modular FiveM/QBCore server in which gameplay systems operate as separate resources while sharing framework services, persistent data and cross-resource events.

Major documented system groups include:

- Player and character systems
- Inventory
- Vehicle shop
- Vehicle keys
- Garage
- Vehicle engine
- Fuel company
- Fuel businesses
- Electronics
- EMS and emergency services
- Persistent tent storage
- Interactive NPC systems
- TDM and multiplayer sessions

Source: `docs/architecture.md`

---

## 3. Work & Earning Money

Bloodline RP includes multiple jobs and economic activities.

The broader gameplay model allows players to work within the server and earn money that can then be used across the wider roleplay economy.

Documented job/activity areas include:

- Delivery and logistics activities
- Fishing
- Fuel-related delivery/work systems
- Other integrated job resources

The development timeline also documents a wider resource environment containing jobs, banking, vehicles, phone systems, maps and other gameplay resources.

The intended gameplay relationship is:

    Job / Activity
          |
          v
      Earn Money
          |
          v
       Player Economy
          |
          v
    Purchase / Services
          |
          v
    Continue Roleplay

This document does not claim that every individual job resource was originally developed from scratch for Bloodline RP; the repository distinguishes custom Bloodline development from framework and third-party integrations.

---

## 4. Survival & Consumables

Bloodline RP uses food and drink as part of the player's ongoing gameplay experience.

Players need food and drinks during normal roleplay and can purchase consumables through the server's shop ecosystem.

This creates a recurring economic relationship:

    Work
      |
      v
    Earn Money
      |
      v
    Purchase Food / Drinks
      |
      v
    Maintain Character
      |
      v
    Continue Playing

The shop ecosystem includes 24/7-style retail functionality as well as specialised shops.

---

## 5. Shops & Commerce

The server contains multiple types of shops and commercial interactions.

Confirmed examples include:

- 24/7 shops for food and drinks
- Fishing shop for fishing-related items
- Truck shop for trucking-related items
- Clothing shops
- Haircut/barber shops
- Gun/weapon shop
- Electronics business/shop systems
- NPC marketplace systems
- Customs sales/business systems
- Other documented business and transaction systems

The architecture should therefore be understood as a wider commerce layer rather than a single shop resource.

---

## 6. Vehicles & Transport

Vehicles form a connected part of the player journey.

The documented vehicle architecture connects:

    Vehicle Shop
          |
          v
    Vehicle Purchase
          |
          +---------> Persistent Vehicle Ownership
          |
          v
    Vehicle Keys
          |
          v
       Garage
          |
          v
    Vehicle Access
          |
          v
    Engine / Fuel
          |
          v
    Normal Gameplay

The architecture documentation also records vehicle-related functionality including vehicle shops, keys, garages, engine behaviour, test drives, job vehicles and fuel state.

---

## 7. Fuel & Petroleum Economy

Fuel is not documented as a simple vehicle percentage.

The Bloodline fuel system connects:

    Central Fuel Company / Depot
                |
                v
        Wholesale Supply
                |
                v
       Fuel Business / Station
                |
                v
        Station Inventory
                |
                v
          Player Purchase
                |
                v
        Vehicle Fuel System
                |
                v
       Vehicle Consumption

The documented ecosystem includes multiple fuel grades, station businesses, petroleum stock, delivery operations, jerrycan functionality, business transactions and persistent database state.

Source: `docs/fuel-petroleum-economy.md`

---

## 8. Housing & Gang Gameplay

Bloodline RP also includes gang housing.

Gangs can purchase gang houses, providing a persistent roleplay/world feature beyond ordinary character spawning.

This is represented at the ecosystem level as:

    Gang
      |
      v
    Gang House
      |
      v
    Gang Roleplay / Organisation

Only the confirmed gang-house functionality is claimed here. Additional housing features are not described unless supported by separate evidence.

---

## 9. Emergency & Medical Gameplay

The EMS system provides an integrated emergency-service lifecycle.

A documented EMS interaction can be represented as:

    Player Emergency
          |
          v
      EMS Response
          |
          v
    Medical Assessment
          |
       +--+--+
       |     |
       v     v
      CPR  Treatment
       |     |
       +--+--+
          |
          v
       Recovery
          |
          v
    Service Record
          |
          v
    Payment / EMS Account

The EMS ecosystem includes emergency medical interactions, CPR/revive, treatment, duty management, staff records, rank-based permissions, financial accounts, service payments, uniforms, emergency vehicles, garage integration, lift/helipad access and persistent records.

Source: `docs/ems-emergency-services.md`

---

## 10. Police & Crime Response

The server includes police roleplay and crime-response functionality.

The confirmed high-level flow is:

    Crime / Incident
          |
          v
    Police Response
          |
          v
    Police Station
          |
          v
    Cells / Detention

Police functionality is represented at the ecosystem level here. Specific mechanisms such as MDT workflows, evidence systems, warrants or sentencing are not claimed in this document unless separately supported by repository evidence.

---

## 11. Communication & Social Systems

The wider Bloodline environment includes integrated communication resources.

The documented development environment includes:

- Phone systems
- Radio
- Voice-related systems
- Bloodline chat functionality

These systems provide communication channels that connect players and server organisations during normal roleplay.

---

## 12A. Administration & Rule Enforcement

Bloodline RP also includes an administrative oversight layer.

Administrators monitor the server and player activity to help enforce the server's roleplay rules.

The high-level model is:

    Player Activity
          |
          v
    Administrative Monitoring
          |
      +---+---+
      |       |
      v       v
    Compliant  Rule Violation
      |           |
      v           v
   Continue    Consequences
    Playing

The administrative layer therefore sits across the wider player journey rather than being limited to a single gameplay system.

Players are expected to follow Bloodline RP rules while participating in the city. Where administrators identify rule-breaking behaviour, consequences can be applied according to the server's rules and administrative procedures.

This provides an additional governance layer alongside the technical gameplay systems.

---

## 12. Competitive Gameplay

Bloodline RP also contains a separate competitive/TDM environment.

The documented architecture intentionally isolates TDM state from normal RP state.

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
         v
    TDM Exit
         |
         v
    Restore Normal RP State

This separation allows competitive gameplay to operate without incorrectly carrying temporary TDM state into the normal roleplay environment.

---

## 13. Persistent World

The player journey is backed by persistent database infrastructure.

The documented architecture uses MySQL for persistent server state including areas such as:

- Character information
- Character-slot state
- Vehicle ownership
- Business data
- Fuel-company data
- Fuel-station data
- Staff and manager information
- Sales and transaction history
- Persistent storage

The result is a server environment in which important state can survive beyond a single gameplay session.

---

## 14. Overall Gameplay Loop

The wider Bloodline RP experience can therefore be represented as:

    +--------------------+
    |  Character / Spawn |
    +---------+----------+
              |
              v
    +--------------------+
    |   Enter RP World   |
    +---------+----------+
              |
       +------+------+
       |             |
       v             v
    Work / Jobs   Explore / RP
       |             |
       v             |
    Earn Money       |
       |             |
       +------+------+
              |
              v
    +--------------------+
    | Shops / Services   |
    +---------+----------+
              |
       +------+------+
       |      |      |
       v      v      v
     Food   Items  Clothing
       |      |      |
       +------+------+
              |
              v
    +--------------------+
    | Vehicles / Housing |
    | Businesses / Jobs  |
    | Police / EMS       |
    | Communication      |
    +---------+----------+
              |
              v
    +--------------------+
    | Persistent State   |
    |      MySQL         |
    +--------------------+

This loop operates alongside specialised systems such as the Bloodline fuel/petroleum economy and TDM environment.

---

## Evidence Boundary

Bloodline RP combines:

- Custom Bloodline systems
- Heavily customised gameplay workflows
- QBCore framework functionality
- Integrated resources
- Third-party dependencies
- Shared infrastructure

The repository therefore does not treat the presence of a resource as proof that every component was written from scratch.

The purpose of this document is to demonstrate how the documented systems fit together into a coherent roleplay environment.

Detailed engineering evidence is provided separately for selected systems.

---

## Related Documentation

- [System Architecture](../docs/architecture.md)
- [Dual Character System](../docs/dual-character-system.md)
- [EMS & Emergency Services](../docs/ems-emergency-services.md)
- [Fuel & Petroleum Economy](../docs/fuel-petroleum-economy.md)
- [Development Timeline](development-timeline.md)
- [Database Architecture](database/database-architecture.md)
- [Schema Inventory](database/schema-inventory.md)
