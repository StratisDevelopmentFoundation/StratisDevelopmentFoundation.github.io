---
title:  "Generating private keys"
date:   2018-03-09 16:16:01 -0600
categories: code_sample
post_importance: 1
---

# Generating private keys

The private key for a Stratis address allow you to spend the funds at that address. You can also derive the address from the private key.

Therefore, a private key is sort of like the password for a Stratis address.

If you are making, for example, a payment system on Stratis, you will need to use code similar to the following to make an address where your software can receive money.

You will need to save the private keys securely in a database, after generating them, so that you can later send the money. If you do not save the private keys, any money sent to that address will be locked forever.

```cs
// somewhere up above, 'using NBitcoin;'

private static string GenerateAddressForPayment()
{
    Key privateKey;
    privateKey = new Key();
    var AddressString = privateKey.PubKey.GetAddress(Network.StratisMain).ToString();
    var privateKeyString = privateKey.GetWif(Network.StratisMain).ToWif().ToString();
    //Store the privateKeyString in a database somewhere
    return AddressString;
}
```

## Security Concerns

Care must be taken in the management of private keys that are generated in this way. If written to a database, anyone with access to that database could potentially steal all the money at these addresses. Managing access to stored private keys is beyond the scope of this article, but be aware that anyone who knows the private keys can take the funds. Furthermore, if the database containing the private keys is destroyed, all money contained in those addresses is lost. Therefore backups are also an important part of properly managing private keys.