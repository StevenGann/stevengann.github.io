# A Proposal For A Blockchain Platform For Inter-Game Cooperation And Trade

### By Steven Gann


## The Problem

Many games, including multiplayer games and single-player games connected to online distribution platforms like Steam and Amazon, feature in-game currency and/or collictibles in the form of useable items, cosmetic items, badges, and achievements.
Some of these games are able to interact in some limited capacity through extensive cooperation between the developers, but usually only on shared platforms such as Steam. Alternatively, some games have attempted to interact with existing blockchains and cryptocurrencies such as Bitcoin or Ethereum, but between technical limitations, limited developer control and freedom, and the varied reputations of general cryptocurrencies, none have gained significant traction for more than short fad explosions, such as CryptoKitties.

## The Solution

A decentralized blockchain and cryptocurrency platform to provide numerous facilities for both developers and players.

**Developers can...**

* Use blockchain identities for user verification, reducing liability for holding user credentials
* Develop games that allow out-of-game trading and currency exchange without supporting an e-commerce platform
* Allow players to use items and currency from other games without requiring cooperation from developers of the other games

**Players can...**

* Use one identity across all games that support the platform, or several if they choose
* Synergize by trading items and currency from one game to purchase items or currency in another game
* Enjoy costmetic or functional items from one game in other games

## Blockchain Object Definitions

The blockchain records will contain several different kinds of records in order to meet the different needs and functionality required by the different blockchain users. These records are all categorized into one of four categories.

1. Commodity Definition
2. User Declaration
3. Transaction
4. Governance Action

### Commodity Definition

The most basic record is a commodity definition. Commodity quantities are represented as 32-bit floating-point values. Commodities are also sub-classified into Currencies and Items, with the chief distinction being that Items must always be in whole units, whereas currencies can use fractional quantities.

The record defining a new Commodity would follow this format:

```
[Record category][Commodity ID]
```