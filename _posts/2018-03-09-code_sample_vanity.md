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

                // Write the WIF and corresponding address if the address matches the desired format
                if (address.ToUpper().StartsWith(prefix.ToUpper()))
                {
                    string privateKeyForDisplay = privateKey.GetWif(Network.StratisMain).ToWif();
                    return privateKeyForDisplay + " " + address;
                }
            }
        }
    }
}
```

## Multi-threaded generation and input validation

So you want to use all the CPU available on your computer to generate a fancy vanity address? 
Here is how you can do that multi-threaded, and there is additional input validation since O 
and I are not allows in address due to ambiguity. 

Additionally this example does generation based upon input variables, so it's more easy to use 
without recompiling and you can even share it with your friends. Start first with two characters, and you 
will quickly learn that it is much harder to generate 3 characters, and even harder with more.

Sometimes the threads find a match almost at the same time, so multiple private key and addresses might 
appear in the console output.

Example on performance with threads: Generating address with prefix "SASA" with a single thread took
22 seconds on high-end CPU. With 8 threads, it took 1-2 seconds. Results may vary on different computers,
and there is an element of (random) luck involved.


```cs
using System;
using NBitcoin;

namespace teststrat2
{
    class Program
    {
        static void Main(string[] args)
        {
            if (args.Length == 0)
            {
                Console.WriteLine("You must supply the address prefix. You can also specify number of threads, as the second parameter. E.g. \"dotnet teststart2.dll \"HI\" 10\".");
                return;
            }

            var prefix = args[0];

            var prefixUpper = prefix.ToUpper();

            if (prefixUpper.Contains("O") || prefixUpper.Contains("I") || prefixUpper.Contains("0"))
            {
                Console.WriteLine("The characters O, I and 0 (zero) is not allowed in addresses.");
                return;
            }

            var count = 4;

            if (args.Length == 2)
            {
                int.TryParse(args[1], out count);
            }

            var clock = Stopwatch.StartNew();
            Task[] tasks = new Task[count];

            Console.WriteLine($"Starting to generate Stratis vanity address with prefix \"{prefix}\" and using {count} threads.");

            CancellationTokenSource cts = new CancellationTokenSource();

            for (int i = 0; i < count; i++)
            {
                tasks[i] = Task.Factory.StartNew(() => VanityGenerate(prefix, cts.Token));
            }

            Task.WaitAny(tasks);

            cts.Cancel();

            Console.WriteLine($"{clock.ElapsedMilliseconds}ms to generate private key and address. Press enter to exit.");
            Console.ReadLine();
        }

        private static void VanityGenerate(string prefix, CancellationToken cancellationToken)
        {
            Key privateKey;
            while (true)
            {
                if (cancellationToken.IsCancellationRequested)
                {
                    return;
                }

                privateKey = new Key();
                string address = privateKey.PubKey.GetAddress(Network.StratisMain).ToString();

                // Write the WIF and corresponding address if the address matches the desired format
                if (address.ToUpper().StartsWith(prefix.ToUpper()))
                {
                    string privateKeyForDisplay = privateKey.GetWif(Network.StratisMain).ToWif();
                    Console.WriteLine(privateKeyForDisplay + " " + address);
                    return;
                }
            }
        }
    }
}

```

## Security Concerns

This process should only be done on a secure system. Do not allow the private keys to be seen by anyone who you don't want to have access to the funds that will be stored at the corresponding Stratis address.