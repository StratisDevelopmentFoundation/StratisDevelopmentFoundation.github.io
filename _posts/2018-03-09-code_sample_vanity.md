---
title:  "Creating a vanity address"
date:   2018-03-09 16:16:01 -0600
permalink: /vanity-address/
categories: code_sample
post_importance: 4
---

# Creating a vanity address

A vanity address is the name for an address that starts with a recognizable word.

If you generate enough addresses, eventually you will find an address that has words you want in it. The longer the word you want, the longer it will take to be found, on average.

The code below takes a prefix (prefix must start with S, like all Stratis addresses) and generates addresses until the start of the address matches the requested prefix. It then prints the address/private key to the screen and halts.

You should edit the code below so that the prefix "SA" is change to something else that starts with S.

```cs
using System;
using NBitcoin;

namespace teststrat2
{
    class Program
    {
        static void Main(string[] args)
        {
            Console.WriteLine(VanityGenerate("SA"));
        }

        private static string VanityGenerate(string prefix)
        {
            Key privateKey;
            while (true)
            {
                privateKey = new Key();
                string address = privateKey.PubKey.GetAddress(Network.StratisMain).ToString();
                string privateKeyForDisplay = privateKey.GetWif(Network.StratisMain).ToWif();
                // Write the WIF and corresponding address if the address matches the desired format
                if (address.ToUpper().StartsWith(prefix.ToUpper()))
                {
                    return privateKeyForDisplay + " " + address;
                }
            }
        }
    }
}
```

## Security Concerns

This process should only be done on a secure system. Do not allow the private keys to be seen by anyone who you don't want to have access to the funds that will be stored at the corresponding Stratis address.