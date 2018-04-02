---
title:  "Writing an OP_RETURN message to the blockchain"
date:   2018-04-01 01:16:01 -0600
permalink: op_return_write
categories: code_sample
post_importance: 2
---

# Writing an OP_RETURN message to the blockchain

When you create a transaction on the Stratis blockchain, it is possible to write a text message to the blockchain as well with arbitrary data.

The code below is a full c# console program that writes a text message to the blockchain in a format that is readable by anyone examining the blockchain.

It has a few requirements to use it:

1. You must have a node installed and synched
2. The node must have the RPC API active
3. You must have an address on the node with a small amount of Stratis for the transaction fee (at least 0.001, or more depending on the size of the message)
4. You must edit the code below so that the private key for the address mentioned in "3" above replaces the "PRIVATE_KEY_GOES_HERE" variable (use dumpprivkey from the QT wallet console to get the text private key)
5. You must edit the code below so that the address associated with the private key replaces "ADDRESS_GOES_HERE"
6. You must edit the code so that the "MESSAGE_GOES_HERE" is replaced by your message.
7. You must edit the code so that "NetworkCredential" username and password matches the ones you set in your rpc config, and the RPCClient port (default 16174) matches the port in your configuration

You should be able to create a new solution name "StratTest1" in Visual Studio Community, add the NStratis nuget package to it, and replace the code of Program.cs with the following:


```cs
using System;
using System.Linq;
using System.Net;
using System.Security.Cryptography;
using System.Text;
using System.Collections.Generic;

using NBitcoin;
using NBitcoin.RPC;

namespace StratTest1
{
    class Program
    {
        static void Main(string[] args)
        {
            string sourcePrivateKey = "PRIVATE_KEY_GOES_HERE";
            List<string> destinationsList = new List<string>()
            {
                "ADDRESS_GOES_HERE"
            };
            string messageText = "MESSAGE_GOES_HERE"
            double burnAmount = 0;

            var transaction = WriteMessageToBlockchain(sourcePrivateKey, destinationsList, messageText, burnAmount);
        }

        private static Transaction WriteMessageToBlockchain(string stratPrivateKey, string dataToWrite, double burnAmount)
        {
            // helper for when no destination is provided
            // (writes only to source address)
            List<string> destinations = null;
            return WriteMessageToBlockchain(stratPrivateKey, destinations, dataToWrite, burnAmount);
        }

        private static Transaction WriteMessageToBlockchain(string stratPrivateKey, string destination, string dataToWrite, double burnAmount)
        {
            // helper for when only one destination is provided
            List<string> destinations = new List<string>()
            {
                destination
            };
            return WriteMessageToBlockchain(stratPrivateKey, destinations, dataToWrite, burnAmount);
        }

        private static Transaction WriteMessageToBlockchain(string stratPrivateKey, List<string> destinations, string dataToWrite, double burnAmount, bool disposeDust = true)
        {
            if (dataToWrite.Length > 45000) //somewhere between 45k and 50k is our character limit, with some wiggle room due to number of non-dust inputs
            {
                throw new System.InvalidOperationException("Message is too large. Use less data.");
            }

            // RPC is the API we set up to interact with the blockchain
            var rpc = GetRPC();

            // set up various network and fee variables
            double oneSatoshi = 0.00000001; //seems like a minimum for the required transaction that must accompany writing to the blockchain
            double minFee = 0.0001; //stratis min fee
            int transactionFeeSatoshisPerByte = 10;
            int maxTransactionSizeBytes = 100000;
            int maxTransactionFeeSatoshis = maxTransactionSizeBytes * transactionFeeSatoshisPerByte;
            int changeOutputBytes = 40; //number of bytes that a change output typically adds
            int signingInputBytes = 82; // number of bytes that signing the transaction typically adds per input
            int inputBytes = 40 + signingInputBytes; //number of bytes that an input typically adds, for max dust size calculations
            int worthwhileInputSatoshis = inputBytes * transactionFeeSatoshisPerByte; // using an input of this size or smaller would actually cost more in fees than the input is worth, if constants above are correct
            int satoshisPerCoin = 100000000;
            var burnAmountSatoshis = burnAmount * satoshisPerCoin;

            // format message
            var chunks = ChunksUpto(dataToWrite, 40); //40 is max size of op return, so divy it up into chunks of this size
            var formattedChunks = chunks.Select(c => Encoding.UTF8.GetBytes(c));

            //convert private key to the source address so we can query it for funds
            BitcoinSecret sourcePrivateKey = new BitcoinSecret(stratPrivateKey, Network.StratisMain);
            BitcoinAddress sourceAddress = sourcePrivateKey.GetAddress();

            // getting unspent coins from a specific address using RPC
            BitcoinAddress[] sourceAddressArray = new BitcoinAddress[] { sourceAddress };
            var unspentCoins = rpc.ListUnspent(0, 9999999, sourceAddressArray); //9999999 is a short hand for "give me as many unspent coins as you have". maybe maxint or something would be better here

            Transaction sendTx = new Transaction(); //transaction that we will add inputs and outputs too, and then send to rpc

            // ADD INPUTS TO TRANSACTION

            Money changeBalance = new Money(0); //this will hold how much change we have left over from this transaction
            foreach (var unspentCoin in unspentCoins)
            {
                string opCode = unspentCoin.ScriptPubKey.ToString();
                if (unspentCoin.IsSpendable && opCode.StartsWith("OP_DUP OP_HASH160") && unspentCoin.Address == sourceAddress && unspentCoin.Amount.Satoshi > worthwhileInputSatoshis) // filtering for appropriate unspent outputs and ignoring "dust"
                {
                    var txInput = new TxIn()
                    {
                        PrevOut = unspentCoin.OutPoint,
                        ScriptSig = sourceAddress.ScriptPubKey
                    };
                    sendTx.Inputs.Add(txInput);
                    changeBalance = changeBalance + unspentCoin.Amount;
                    if (changeBalance.Satoshi > maxTransactionFeeSatoshis + burnAmountSatoshis)
                    {
                        break; // we have enough inputs to meet our needs for this transaction
                    }
                }
            }

            // ADD MESSAGE TO TRANSACTION

            double totalFees = 0;
            bool burnAdded = false;
            foreach (var messageChunk in formattedChunks)
            {
                var messageBurn = oneSatoshi;
                if (!burnAdded && burnAmount > oneSatoshi)
                {
                    messageBurn = burnAmount; // for the first message, add the requested burnAmount
                }
                burnAdded = true;
                TxOut messageTxOut = new TxOut()
                {
                    Value = new Money((decimal)messageBurn, MoneyUnit.BTC),
                    ScriptPubKey = TxNullDataTemplate.Instance.GenerateScriptPubKey(messageChunk)
                };
                totalFees = totalFees + oneSatoshi;
                sendTx.Outputs.Add(messageTxOut);
            }

            // TAG DESTINATION ADDRESSES

            if (destinations != null) //only need to add destination address if its different than the source address
            {
                foreach (var destination in destinations)
                {
                    //destination addresses can be used so that this message shows up on the transaction list of more than address.
                    BitcoinAddress destAddress = BitcoinAddress.Create(destination, Network.StratisMain);
                    if (destAddress != sourceAddress)
                    {
                        TxOut destTxOut = new TxOut()
                        {
                            Value = new Money((decimal)oneSatoshi, MoneyUnit.BTC),
                            ScriptPubKey = destAddress.ScriptPubKey
                        };
                        sendTx.Outputs.Add(destTxOut);
                        totalFees = totalFees + oneSatoshi;
                    }
                }
            }

            // REQUIRED MESSAGE OUTPUTS
            // need number of outputs equal to number of message chunks otherwise tx is rejected as non standard

            var additionalTxAddress = sourceAddress;
            if (disposeDust)
            {
                // since these are dust transactions that cannot be profitably collected later, we might as well just dispose of them to another address
                // in fact we find that have a large amoung of dust in a stratis wallet actually slows down the QT wallet due to increased CPU requirements
                // since sending them to a specific address might be seen as an attempt to steal them (despite spending them having a negative value), we'll just generate a random address instead and not save the private key
                additionalTxAddress = new Key().PubKey.GetAddress(Network.StratisMain);
            }
            while (sendTx.Outputs.Count - formattedChunks.Count() + 1 < formattedChunks.Count())
            {
                TxOut additionalTxOut = new TxOut()
                {
                    Value = new Money((decimal)oneSatoshi, MoneyUnit.BTC),
                    ScriptPubKey = additionalTxAddress.ScriptPubKey
                };
                sendTx.Outputs.Add(additionalTxOut);
                totalFees = totalFees + oneSatoshi;
            }

            // SET FEES
            var signingBytes = signingInputBytes * (sendTx.Inputs.Count - 1); // estimating number of bytes that signing will add. Can't sign yet because change output isnt added. We need to add our fees without signing.
            var estimatedSize = sendTx.GetVirtualSize() + changeOutputBytes + (signingBytes);
            totalFees = totalFees + minFee * Math.Ceiling(estimatedSize / 1000.0); // one minFee per 1000 bytes
            totalFees = Math.Min(totalFees, maxTransactionFeeSatoshis * satoshisPerCoin); //fee can't be larger than max fee

            // RETURN CHANGE TO SELF
            // all remaining coins back to original address
            // unspent amount is implicitly given as fees
            TxOut changeBackTxOut = new TxOut()
            {
                Value = new Money(((changeBalance.ToDecimal(MoneyUnit.BTC) - (decimal)totalFees)), MoneyUnit.BTC),
                ScriptPubKey = sourceAddress.ScriptPubKey
            };
            sendTx.Outputs.Add(changeBackTxOut);


            // Sign the transaction using the specified private key
            sendTx.Sign(sourcePrivateKey, false);

            // final check to make sure size isn't too big
            if (sendTx.GetVirtualSize() > maxTransactionSizeBytes)
            {
                throw new System.InvalidOperationException("Transaction is too large. Use less inputs/outputs.");
            }

            // Broadcast Transaction
            SendTransactionRpc(rpc, sendTx);

            return sendTx;
        }

        public static RPCClient GetRPC()
        {
            // Be sure you have the following parameters enabled on the Stratis node (default port: 16174)
            // default location of the config file : C:\Users\Administrator\AppData\Roaming\Stratis\stratis.conf
            /*
####RPC Settings####
# Activate RPC Server (default: 0)
server = 1
#Where the RPC Server binds (default: 127.0.0.1 and ::1)
rpcbind = 127.0.0.1
#Ip address allowed to connect to RPC (default all: 0.0.0.0 and ::)
rpcallowedip = 127.0.0.1
rpcuser = user
rpcpassword = pass*/

            // The information (ip address) and Credential to connect to the node
            var credentials = new NetworkCredential("user", "pass");
            return new RPCClient(credentials, new Uri("http://127.0.0.1:16174/"), Network.StratisMain);
        }

        static IEnumerable<string> ChunksUpto(string str, int maxChunkSize)
        {
            //divides a string into an array of strings of a certain size.
            for (int i = 0; i < str.Length; i += maxChunkSize)
                yield return str.Substring(i, Math.Min(maxChunkSize, str.Length - i));
        }

        public static void SendTransactionRpc(RPCClient rpc, Transaction tx)
        {
            //            rpc.SendRawTransactionAsync(tx);  //async was causing some problems so I switched to non async, don't recall the specifics
            rpc.SendRawTransaction(tx);
        }
    }
}

```

After running the code above, you should be able to use a blockexplorer like [chainz](http://chainz.cryptoid.info/strat) to inspect the transaction and see the OP_RETURN. Simply go to the address you wrote with in Chainz, and click on the transaction ID.

## Security Concerns

Saving a private key to file could be a security risk. Therefore, do not put significant funds on the address involved, and be careful about who has access to your file system.