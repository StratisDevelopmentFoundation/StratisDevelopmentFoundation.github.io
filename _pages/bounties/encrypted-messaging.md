---
title:  "Encrypted Messaging"
categories: inactive_bounty coding
post_importance: 2
bounty: 50
---
Earn {{ page.bounty }} STRAT by completing this task.
{: .notice--info}

# Bounty Description

If Alice and Bob know each other's public keys, and they know their own private keys, then they can use ECDH to have a "shared secret". See [this article](https://medium.com/proof-of-working/encrypted-messaging-on-the-neo-blockchain-631bbfcf99f6) for a brief overview.

With a shared secret, Alice can encrypt a message using the shared secret as the password, and then Bob can decrypt it using the shared secret.

This means that an address on the Stratis blockchain could send a message to another address on the Stratis blockchain using an OP_RETURN transaction. If it is encrypted using a shared secret, only the recipient could read the message.

We already have code to write OP_RETURNs [here](/op_return_write/). We just need to encrypt it using ECDH first. "Bouncy Castle" is a library of encryption related functionality that is already being used in the Stratis Full Node. It should support ECDH.

The task for this bounty is to create an API endpoint in the Stratis Full Node that can use ECDH to send encrypted messages by OP_RETURNs to an address, and another API endpoint that can decrypt the message.