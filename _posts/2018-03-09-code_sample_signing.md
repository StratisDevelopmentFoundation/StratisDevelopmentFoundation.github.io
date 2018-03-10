---
title:  "Signing a message and Verifying a message"
date:   2018-03-09 16:16:01 -0600
categories: code_sample
post_importance: 3
---

# Signing a message and Verifying a message

```C#
public static string GetMessageSignature(string message, string privateKeyAsString)
{
    BitcoinSecret sourcePrivateKey = new BitcoinSecret(privateKeyAsString, Network.StratisMain);
    return sourcePrivateKey.PrivateKey.SignMessage(message);
}

public static bool VerifySignedMessage(string message, string signature, string address)
{
    var stratisAddress = new BitcoinPubKeyAddress(address, Network.StratisMain);
    return stratisAddress.VerifyMessage(message, signature);
}
```