---
title: Protocol Registry
author_profile: false
permalink: /projects/protocol_registry/
categories: projects
short_overview: A second layer on top of the Stratis blockchain allowing for community created protocols and applications
---
# Protocol Registry

## Overview

The Stratis team has created a blockchain and blockchain software that allows users of the software to move STRAT coins between addresses. In addition to this basic use of a blockchain, it is also possible to write arbitrary data to the blockchain.

The benefit of writing data to the blockchain is that the data written is immutable (can't be changed) because all the other nodes in the system record the data. There is also the additional benefit that only users who can prove ownership of an address can make these writes to the blockchain, thereby adding an authentication mechanism that can be used to prove someone is who they say they are.

If the data written follows a specific format (as specified by a protocol) and it is read by other clients that are aware of this protocol, then this allows for certain types of applications to be built.

### An example protocol

One simple example is a voting protocol. One client could write to the blockchain some text that looks like this:

```
Voting Protocol Version 1.0
Question: What is the best flavor of ice cream?
Answers:
1 - Chocolate
2 - Strawberry
3 - Other (Specify)
```

Client software running on other computers that is aware of this voting protocol would be scanning for messages of this format in the blockchain. The user of that software would be notified of the vote, and they could write to the blockchain a response that would be seen by all users of the software. Their response might look like:

```
Voting Protocol Version 1.0
Response to Question: What is the best flavor of ice cream?
Answers: 3 - Other - Pineapple
```

Keep in mind that these example texts are human readable - In an actual implementation, these messages would be in a format that is much more compressed.

The end result of this is that users would be able to conduct votes on the blockchain. In combination with other features of the blockchain, this could have many uses.

However, there must be an agreed upon format for how the messages are written to the blockchain. This is the only way for software to be written in a way that allows for all users of the system to agree on what is and isn't a valid vote.

## Current Status

## How to contribute to this project