---
title:  "Creating a vanity address"
date:   2018-03-09 16:16:01 -0600
categories: code_sample
post_importance: 4
---

# Creating a vanity address

A vanity address is the name for an address that starts with a recognizable word.

If you generate enough addresses, eventually you will find an address that has words you want in it. The longer the word you want, the longer it will take to be found, on average.

The code below takes a prefix (prefix must start with S, like all Stratis addresses) and generates addresses until the start of the address matches the requested prefix. It then prints the address/private key to the screen, and to a text file, and halts.

```cs
// somewhere up above:
// using NBitcoin;
// using System.IO;


private static void VanityGenerate(string prefix)
{
    string fileName = @"StratisVanity.txt";
    Key privateKey;
    while (true)
    {
        privateKey = new Key();

        // Write the WIF and corresponding address to a file if the address matches the desired format
        if (privateKey.PubKey.GetAddress(Network.StratisMain).ToString().ToUpper().StartsWith(prefix.ToUpper()))
        {
            Console.WriteLine(privateKey.GetWif(Network.StratisMain).ToWif() + " " + privateKey.PubKey.GetAddress(Network.StratisMain).ToString());
            File.AppendAllText(fileName, privateKey.GetWif(Network.StratisMain).ToWif() + Environment.NewLine);
            File.AppendAllText(fileName, privateKey.PubKey.GetAddress(Network.StratisMain).ToString() + Environment.NewLine);
            break;
        }
    }
}
```

## Security Concerns

Writing private keys to a text file is insecure. Anything with access to the file system can potentially leak the private keys generated in this way. This process should only be done on a secure system.