---
title: Using the Stratis node for RPC
toc: true
toc_label: "Using the Stratis node for RPC"
toc_icon: "x"
categories: code_sample
date:   2018-04-09 16:16:01 -0600
post_importance: 5
---
# Using the Stratis node for RPC

This article will walk you through the steps of using the RPC API to make a Stratis address, and constructing your first transactions on the Stratis blockchain.
{: .notice--info}

### Prerequisites

You should have Visual Studio already installed.

Make sure Stratis Node with RPC is set up. You can either install the old and stable QT wallet (instructions [here](/install_qt/)) or you can install the new and beta Stratis C# Full Node (instructions [here](/install_fn/))

## Creating an application in Visual Studio

### Solution setup

Launch Visual Studio Community. Create a new "solution". Choose a template of type ".Net Core Console Application". Choose target framework 2.0. Give the project a name.

At the end of this you have a skeleton Hello World C# program. You can press F5 to run it, and it will write "Hello World!" to the console.

### Add NStratis nuget

Launch the Nuget package manager inside of Visual Studio. Search for NStratis, and add it to the Solution you created in the previous step. Add "using Nstratis;" as the first line of the Program.cs

Run the program again with F5. There should be no errors, but the output should be the same.

### Add some code that uses NStratis

You may still be waiting for the blockchain to fully synchronize. You cannot send any transactions until it finishes synching. However, you can do some things that are offline - For example, you can generate addresses.

#### Generate a vanity address

Some sample code that you can run while the blockchain synchs can be found [here](/vanity-address/). Replace the namespace "teststrat2" with the name you gave to your solution in the previous steps and paste it into the Program.cs

Build and run the code again. You should see a private key and address pair in the console. Copy the private key (the first part before the space) to a text file for later use.

You may want to customize the prefix in order to generate an address that starts with your desired prefix. However, the prefix should start with "S". All Stratis addresses start with "S". The longer the prefix the longer it will take to find an address that starts with that prefix. Consider a 3-4 letter prefix at most, unless you are very patient.

## Fund an address for testing - QT

The following instructions are for the QT wallet. There is a bounty for writing the instructions to do the equivalent for the Full Node.

### Load the QT Wallet console

Load the QT wallet software. Wait for the blockchain to be fully synchronized. Then go to "Help", "Debug Window", "Console".

### Import the private key generated earlier

In the console, type "importprivkey THE_PRIVATE_KEY A_LABEL" where THE_PRIVATE_KEY is the private key you generated and saved earlier, and A_LABEL is what you want to call that address, for example "testaddress".

If this was done correctly, there should be no error messages, and on the "receive" screen of the QT wallet you should be able to see the address you imported.

### Send Strat to address

Next you will send a small amount of Stratis to the address associated to the private key you imported. Because the minimum transaction fee is 0.0001, you don't need to send much. Approximately 0.001 should be enough to do some testing. If you do not have any Stratis coins to send to this address, you can contact us at the [SDF Discord](/discord/) and we will send you a small amount of Stratis for testing.

If you have no idea about how to get Stratis coins because you are absolutely new to crypto, please contact us at the SDF and we will help explain how to acquire some.

## Create and broadcast an OP_RETURN transaction

Use the code found in [this article](/op_return_write/) and modify it as instructed in the article to use the private key and address you generated earlier.

Running the code should write a message to the blockchain in a transaction. Inspect the blockchain in a block explorer to see the transaction you wrote. You should also be able to see the outgoing transaction in the QT wallet.

Congratulations! You've sent your first Stratis transaction.

If you have problems, please visit the SDF in [Discord](/discord/).