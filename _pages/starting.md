---
title: Getting Started on Stratis for developers
permalink: /starting/
toc: true
toc_label: "Getting Started"
toc_icon: "x"
post_importance: 3
bounty: 15
---
# Getting Started with Stratis Development

This article will walk you through the steps of setting up a Stratis development environment, making a Stratis address, and constructing your first transactions on the Stratis blockchain.
{: .notice--info}

This is an unfinished article. Earn {{ page.bounty }} STRAT by improving/completing this article. Edit it [here](https://github.com/StratisDevelopmentFoundation/StratisDevelopmentFoundation.github.io/edit/master/{{ page.path }}).
{: .notice--warning}

## Setting up your development environment

### Choose a suitable hosting environment

First you must have a computer capable of handling the development. This is typically a OSX or Windows machine. There is no suitable visual studio for running on Linux.

You will likely need 5-10 GB of space for Visual Studio and the Stratis blockchain, and at least 4GB of RAM. Most modern computers should be sufficient.

If you do not have a suitable computer, or you wish to do your development in the cloud on a VPS, see the Azure Dev Environment instructions [here](/azure_dev/)

### Install Visual Studio Community

As you might expect, for this step you simply search google for "Visual Studio Community Download" and download and install from there. See in depth install instructions [here](/install_vs_windows/).

### Install a Stratis node

While Visual Studio is downloading/installing, proceed to installing and syncing a Stratis Node. You can either install the old and stable QT wallet (instructions [here](/install_qt/)) or you can install the new and beta Stratis C# Full Node (instructions [here](/install_fn/))

If you are interested in making changes to the way the node works, or examining it's source code, you should choose the Stratis C# Full Node. Otherwise, if you simply are using the node to broadcast transactions that are externally created by other C# applications, you can use the QT wallet.

After you have installed the wallet, follow the instructions linked above to synch the blockchain and enable the RPC API.

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

## Where to go from here

### Exploration through debugging

You can use Visual Studio debugging to step through the above code as they run. You can explore the contents of variables, and learn a lot about how the blockchain transactions work.

### Moving coins around

You could write your own transaction methods by modifying the sample code above to not send OP_RETURN messages, but to simply move funds around. Remember to use small amounts of stratis while you are still learning so that you don't accidentally create an invalid transaction or one with 99% fees.

### Dig into the Stratis Full Node code

If you are a more advanced developer, you can start looking at the Stratis Full Node code. While in the scenarios above we have created our own VS solutions that use RPC calls to interact with the Stratis blockchain, a different approach is to take the Stratis Full Node code and extend it so that your application code is running in the same application as the full node. This takes a much more advanced developer to figure out where to modify the code in the full node, but you can do more advanced tasks with this. One example is that you could make the full node, as it synchronizes the blockchain, index all the OP_RETURN messages in the blockchain history into a searchable database. In fact, that example is one thing we are working on at the SDF. Contact us if you would like to help.

### Join an SDF project

Take a look at some of the [projects](/projects/) that we are working on at the SDF. Get involved in the community. Not only will you earn Stratis coins, but you will also learn blockchain development skills.
