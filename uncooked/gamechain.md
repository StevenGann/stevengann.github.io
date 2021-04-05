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

## General Structure

At its core the platform is a traditional cryptocurrency comparable to Litecoin, using a similarly traditional Proof-of-Work mining system in order to offload the majority of infrastructure concerns to a network of miners.

Block rewards are issued as a Commodity not tied to any outside application called simply Gold. Because it is a Commodity on the chain like any other, external applications can interact with it the same way they would any other Commodity with the exception that gold cannot be issued, nor can transactions involving Gold be struck from the chain without a full rollback.

  

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
[UUID][Record Category][Commodity ID][Commodity Category][Quantity][Governor Public Key][Certificate]
```

* UUID: A 128-bit random UUID
* Record Category: A single-byte field indicating that the record is a Commodity Definition
* Commodity ID: A unique 8-32 byte alphanumerical (UTF-8) ID for the Commodity being defined
* Commodity Category: Single byte indicating the categorization of this Commodity Definition
* Quantity: 32-bit floating point number indicating the quantity of the Commodity being created
* Governor Public Key: RSA-4096 public key of the Governor authorizing the creation of the commodity
* Certificate: A cryptographic certificate signed using the Governor's RSA-4096 private key

The Commodity Category field contains one of the following:

* 0x00: New Currency, i.e. the creation of a new currency
* 0x01: New Item, i.e. the creation of a new item type
* 0x10: Additonal Currency, i.e. additional supply to an existing currency
* 0x11: Additional Items, i.e. additional supply to an existing item type

The newly created supplies of the Commodity will be issued to the authorizing Governor's wallet so they may distribute however they see fit.

The base currency used to reward miners, Gold, is defined in the Genesis block.


### Governance Action

One of the key unique features of the platform is the flexibility to alter almost anything on-chain via Governance Actions. These records can roll-back transactions, generate Commodities out of thin air, establish new rules for transactions, and much more. The core of this is the role of Governors.

A **Governor** is a specially designated user and wallet. They typically represent developers, corporate partners, or other interested parties. Governance Action records placed on the chain are then voted on by the Governors, with their votes recorded within new Governance Action records, and the governance Action only takes effect if the required minimum number of votes are cast and there is a sufficient majority.

Governors themselves must be established via a Governance Action, so becoming a Governor requires the approval of the majority of existing Governors. To avoid any chicken-and-egg questions, the Genesis Block of the chain appoints the first Governor. Governors may also be voted off the platform, the intent being the removal of malicious or negligent Governors who are not carrying out their duties voting on Governance Actions or are abusing their power over the ledger in ways that reduce faith in the platform.

The record for a Governance Action would look as follows:

```
[UUID][Record Category][Action Type][Action Data][Governor Public Key][Certificate]
```

* UUID: A 128-bit random UUID
* Record Category: A single-byte field indicating that the record is a Governance Action
* Action Type: A two-byte field indicating the variety of Governance Action
* Action Data: Data indicating how the Governance Action will be executed, dependent on the Action Type
* Governor Public Key: RSA-4096 public key of the Governor proposing the action
* Certificate: A cryptographic certificate signed using the Governor's RSA-4096 private key

The Action Type field contains one of the following:

* 0x0000: Vote on a previous action. 
  * Data: UUID of action, yea (0x01) or nae (0x00) vote
* 0x0001: Propose Governor
  * Data: Public key of proposed Governor
* 0x0002: Strike blockchain transactions
  * Data: Hashes of transactions to be struck

#### Regarding Striking Transactions

Unlike a more traditional distributed ledger where all currency is created by mining or similar mechanisms, and rolling back the blockchain to undo transactions is extremely significant, this platform is designed to be much more flexible. While it is not desireable to strike transactions from the chain, the process should be fairly painless for the platform as a whole. However, Governors should use caution with what transactions they strike and why, as this does risk eroding user faith in the platform. In cases of fraud or malicious exploits, strikes should be used to eliminate unfair advantages gained by specific users while not hurting users who were acting in good faith.





