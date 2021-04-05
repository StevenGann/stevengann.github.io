# A Proposal For A Platform-Agnostic Distributed Communication Platform

### By Steven Gann


## The Problem

There is a lot of standards for wide-area digital communications, from TCP/IP to LoRa, that are quite useful for many applications and many are very well supported across platforms. Unfortunately, if a developer doesn't want to establish their own infrastructure then the only practical choice is the Internet. Alternative endpoints may be used, but ultimately the Internet will be the primary conduit, and with that comes issues of privacy and security. 

## The Solution

A platform-agnostic protocol layer that ensures security and privacy for high-latency, low-bandwidth applications such as IoT.

EchoNet is intended to be as platform-agnostic as possible, assuming only the following:
- Endpoint and Hub nodes are capable of strong cryptography
- Nodes are capable of sending and receiving digital data reliably and losslessly

That's it. The medium may be the Internet and transmission may be via TCP/IP, UDP, HTTP, FTP, or anything else. Messages can be embedded in files or IRC chat or e-mails, or even sent via morse code on 2m radio.

A **Message** is defined simply:

-   16 byte Message UUID
-   16 byte Recipient ID
- 4096 byte (or less) Message
-   32 byte SHA-256 hash of the above
-  256 byte RSA-2048 Public key
-           RSA Certificate
-    1 byte Age Indicator

Nodes include Endpoints and Hubs and the roles are not mutually exclusive.

**Endpoints** assemble messages and transmit them to one or more Hubs.

**Hubs** receive messages from other nodes, validate their SHA-256 hash and RSA certificate, and if valid retransmit to all known nodes with the Age Indicator decremented, if the Age Indicator is still greater than 0. Hubs should also maintain a log of recent message UUIDs and/or hashes in order to avoid retransmitting a message they have received before.

To clarify the logic of the Hub:

```
On Message Received:
    IF Message.Hash is Valid
    AND Mesage.Certificate is Valid
    AND RecentMessages does not contain Message:
        Message.AgeIndicator--.
        ADD Message to RecentMessages.
        IF Message.AgeIndicator > 0:
            FOR EACH Node
                Send Message to Node.
```

The Age Indicator field of the message is an essential component of the EchoNet system because it puts a limit on how far the message can travel across the network. Ideally, an Endpoint should have an approximate idea of how far the message will need to travel and set the Age Indicator as low as possible before sending to a Hub in order to avoid burdening the network more than necessary.

Similarly, Hubs may implement more restrictions on messages, such as discarding very large messages, clamping the Age Indicator to a maximum value, decrementing by more than 1, or other measures to control how much traffic flows through them and beyond them.




